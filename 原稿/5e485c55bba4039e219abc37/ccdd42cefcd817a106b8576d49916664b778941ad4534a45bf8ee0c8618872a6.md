## 前言

不知道大家在开发网站的时候有没有过一个这样的需求，我想要在数据库发生数据变化的时候“提醒”我的后端，“奥，数据库发生变化，你应该采取一些行动”。最经典的场景就是聊天室功能了——假设在你的设计中，聊天室的每一个消息都是插入到数据库 messages 表的一行，那么为了让聊天室有人发送消息的时候，别人也能收到消息并在UI上进行更新，就需要在 messages 表发生变化的时候让数据库“告诉”后端，有一条新的消息被插入数据库了，然后后端再让前端发生数据更新（这往往可以通过 websocket 或者 sse 实现）。

但是通常情况下，后端到数据库的请求总是单向的，我们只能让后端请求数据库，而不能让数据库请求后端。这导致如果我们想要实现聊天室的实时信息同步，就需要不断让后端对数据库的 messages 表进行轮询，如果 messages 表非常大，那么这会是一件非常低效且丑陋的事情，那么有没有什么好方法实现呢？有的兄弟有的，supabase 提供了强大的 Realtime 功能，允许我们直接通过后端对 supabase 数据库的某张表进行监听，而且还允许我们对监听的消息施加过滤器（比如我只想监听某个聊天频道发出的消息）。

---

## Realtime 功能的核心——WAL

在正式讲解之前，很多朋友可能会很好奇，supabase 是如何实现数据库到后端的通信的？

我们都知道 supabase 的后端是 PostgreSQL，和大部分现代化的数据库一样，PostgreSQL 为了避免磁盘内存数据不同步，并不会直接将数据的变更写入磁盘，而是会先将本次操作写入一个 WAL 文件中（Write-Ahead Logging的简写，因为写 WAL 是追加写，这意味着是顺序写入，速度比随机写到数据库磁盘上要快很多，也可靠很多），然后数据库的一个守护进程再将 WAL 里面的操作施加并写入到磁盘上。那么假设我们现在允许后端监听数据库的全部改动，从而完成我们的聊天室功能，是不是只要监听 WAL 文件即可？每当 WAL 文件有操作被顺序写入，我们就把这层操作过滤后发送给后端。没错，这就是 supabase 的 Realtime 功能实现的基本原理。非常简单吧！

---

## Realtime 实战——Realtime + SSE 实现聊天室消息同步

想要在一个 Nextjs 项目中实现一个聊天室，在不使用任何现成库的情况下，恐怕 Realtime + Nextjs SSE 是最快的做法了。

### 1. Service Role 权限创建 supabase 客户端

首先，我们需要注意一点，Realtime 功能默认是只有 Service Role 权限才能使用的，所以我们需要用 Service Role 来创建 supabase 客户端：

```typescript
import { Database } from '@/types/database.types';
import { createClient } from '@supabase/supabase-js';

const NEXT_SUPABASE_URL = process.env.NEXT_SUPABASE_URL || '';
const NEXT_SUPABASE_SERVICE_ROLE_KEY = process.env.NEXT_SUPABASE_SERVICE_ROLE_KEY || '';

export const serviceRoleSupabase = createClient<Database>(NEXT_SUPABASE_URL, NEXT_SUPABASE_SERVICE_ROLE_KEY);
```

### 2. 创建 Realtime 监听通道的后端 SSE 接口代码

我们将实时接收消息的接口定义为一个 SSE，假设代码为 `receive-stream/route.ts`，然后我们的消息全部存储在 `messages` 这个表格里面，这个表格有一个 `room_id` 代表当前这条消息所属的聊天室频道。

那么当前端进入 `room_id` 这个房间时，它对应的监听变化的 SSE 接口代码如下：

```typescript
import supabase, { serviceRoleSupabase } from '@/lib/supabaseClient';
import { NextRequest } from 'next/server';

export const runtime = 'nodejs'; // ⚠️ 不能是 edge

export async function GET(req: NextRequest) {
    // 鉴权和数据校验
	// ...

    const stream = new ReadableStream({
        start(controller) {
            const channel = serviceRoleSupabase
                .channel(`room:${roomId},user:${session.user.id}`)
                .on(
                    'postgres_changes',
                    {
                        event: 'INSERT',
                        schema: 'public',
                        table: 'messages',
                        filter: `room_id=eq.${roomId}`,
                    },
                    payload => {                        
                        controller.enqueue(
                            `data: ${JSON.stringify(payload.new)}\n\n`
                        );
                    }
                )
                .subscribe();

            req.signal.addEventListener('abort', () => {
                supabase.removeChannel(channel);
                controller.close();
            });
        },
    });

    return new Response(stream, {
        headers: {
            'Content-Type': 'text/event-stream',
            'Cache-Control': 'no-cache',
            Connection: 'keep-alive',
        },
    });
}
```

这里有一个很坑的点：创建监听频道时的 `channel` 方法，内部传入的参数是监听频道的 name，实际上，这个还是一个 key，如果你建立了两个相同 name 的监听通道，后面建立的会覆盖之前的，这个过程并不会报错，导致你最早建立的监听在前端没有任何动静。所以对于 name 的构造是有讲究的，你需要保证不同用户的监听的 name 是不一样的。除非你在后端自己再写一层，supabase 是不提供同名监听通道的复用的。

> Nextjs 的 SSE 模块也是支持 NextAuth，如果你的鉴权方案是采用的 NextAuth，那么就可以原封不动迁移到上面的代码中。

### 3. 前端请求 SSE 完成消息的实时同步

然后，我们只需要在前端进入聊天室的时候，发起 SSE 请求即可：

```typescript
    const eventSourceRef = useRef<EventSource | null>(null);

    useEffect(() => {
        if (!visible || !chatroom?.id || !session?.user?.id) {
            return;
        }

        // 防止重复订阅
        if (eventSourceRef.current) {
            eventSourceRef.current.close();
        }

        const es = new EventSource(
            `/api/chat/receive-stream?roomId=${chatroom.id}`
        );
        eventSourceRef.current = es;

        es.onmessage = (event) => {
            try {
                const newMessage = JSON.parse(event.data) as CoachLinkTable['messages']['Insert'];
				// 处理新的数据，加入聊天信息
            } catch (e) {
                console.error('SSE message parse error', e);
            }
        };

        es.onerror = (err) => {
            console.error('SSE error', err);
            es.close();
            eventSourceRef.current = null;
        };

        return () => {
            es.close();
            eventSourceRef.current = null;
        };
    }, [visible, chatroom?.id, session?.user?.id]);
```

这样，消息的传递就实时了

![图片](https://kirigaya.cn/api/storage/image/2a2cccf5-21af-4656-8ebe-044001072e45.webp)

这下实时同步，真就 Realtime 了。