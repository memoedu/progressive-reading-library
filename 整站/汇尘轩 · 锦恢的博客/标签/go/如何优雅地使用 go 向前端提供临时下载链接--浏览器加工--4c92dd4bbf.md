## 前言

>  这是一个困扰了我三年的问题，不过就在昨天，我终于想到了一个完美的解决方案。

相信许多读者在构建数据服务器时都遇到过一个考量：如果前端用户想要下载一个服务器上的资源，我要如何把这个资源安全地给用户呢？

一般而言，有两种方法可以达到这个效果：

1. 直接返回真实 http 资源链接
2. 造轮子

---

### 不优雅的方法一：直接返回真实 http 资源链接

后端的代理服务器（Nginx，Apache）将文件资源的文件夹开放出去，比如我们要把服务器上路径为 `/data/common` 的文件夹让外部可以被访问，访问的路由为 `hostname/data`，那么 `nginx` 可以这么写：

```nginx
server {
    listen 443 ssl;
    ...
    ...

    location /common/ {
        root /data;
        /* 其他配置，详见 https://kirigaya.cn/blog/article?seq=106 */
        autoindex on;
    }
}
```

如果前端用户需要 `/data/common/dataset_1.zip` 这个资源，后端根据一些规则，直接将真实的下载链接 `https://hostname/data/dataset_1.zip`  返回给前端，假设返回的数据格式为：

```json
{ "code": 200, "data": "https://hostname/data/dataset_1.zip" }
```

这个时候，前端就可以通过创建、挂载 `a` 标签的方式在浏览器里面开启资源下载了：

```javascript
...
// 假设前端的 axios 函数为 reqDownloadFile
const res = await reqDownloadFile({...});
if (res.code === 200) {
    // 通过某种方式获取下载文件名
    fileName = ...;
    
    const anchor = document.createElement('a');
    anchor.href = res.url;
    anchor.download = fileName;
    document.body.appendChild(anchor);
    anchor.click();
    
    // 销毁
    document.body.removeChild(anchor);
}
...
```

这么做最简单，但是有一个致命的缺点：不安全。

下载开始时，用户是可以看到下载链接是什么的，只需要点击浏览器下载按钮 -> 完整下载记录，虽然最新版本的 Chrome 对完整的下载记录做了保护，只显示下载文件名和下载的 hostname，但是用于仍然可以通过右击复制下载链接的方式获取真实下载链接。这样，用户就可以脱离你的网站对这个资源链接为所欲为，比如写一个高频请求脚本，来请求你的这个资源，刷光你的服务器流量或者吃光你的服务器带宽，让其他服务饿死。因此，大部分情况下，除非是内网自己乐呵乐呵，只要上了公网，都不太推荐各位这么设计自己的资源服务。



### 不优雅的方法二：造轮子

好吧，我承认，这个其实不能算不优雅的方法，只不过，对于时间有限，有着看不完的动漫和打不完的steam的游戏的我而言，不优雅。

“不优雅的方法二”简单说就是：把资源下载的通道完全由后端服务进程掌握，前端和后端一起紧密配合完成资源的下载，而不走 nginx 配置的 HTTP 下载通道。这么做最大的好处就是可以几乎 100% 自定义资源下载的所有行为，那么想要安全传输数据就很简单了。

> 但是！但是！你这么做相当于是在自己实现一部分 http 3.0 协议的功能，有点吃力不讨好，你需要考虑断链重传、分包传输等复杂问题。

可以给诸位看看我刚进大学时写的一个这样的方法，后端实现函数名为 `StupidDownloadFile`：

```go
func StupidDownloadFile(c *gin.Context) {
	// 防高频中间件
    ...
    
    // 鉴权中间件
    ...
    
    // 安全层中间件
    ...
    
    
    fileKey := c.Query("key")
    
    // 根据 fileKey 得到需要传给前端的文件路径和文件名
    filePath := ...
	fileName := ...
    
	fileByte, err := os.ReadFile(filePath)
	fileLength := len(fileByte)
	fileLengthString := strconv.Itoa(fileLength)

	if err != nil {
		c.JSON(http.StatusOK, gin.H{"code": 4002, "msg": "服务器文件打开错误！可能是服务器的自我防护进程被触发导致所有存储文件被删除！"})
		return
	}

	c.Writer.Header().Add("length", fileLengthString)

	c.JSON(http.StatusOK, gin.H{"code": 200, "data": gin.H{
		"leftTime": leftTime,
		"fileName": fileName,
		"base64":   fileByte,
	}})
}
```

它负责根据前端需要请求的文件ID在中间件里找到对应的服务器文件路径和文件名，然后将文件读入内存中，并将读入的字节、文件名直接给前端。为了让前端能够实时显示下载进度，我还在响应头里面加了一个 `length` 参数，来让前端代码可以计算出当前下载进度。

假设 `StupidDownloadFile` 被绑定到了路由 `/api/download` 上，是一个 `GET` 方法。前端代码如下：

```javascript
import axios from "axios";
import { ref } from 'vue';

const requests = axios.create({
    baseURL : "/api",
    timeout: 300000
});

// 显示下载进度
// 应与 template 中的显示进度的组件绑定，做到实时显示进度
const percentage = ref(0);

async function download(key) {
    const res = await requests({
        url: '/download', method: 'GET',
        params: { key },
        onDownloadProgress: (e) => {
            const fileLength = e.srcElement.getResponseHeader("length")
            let total = parseInt(fileLength);
            percentage.value = Math.min(e.loaded / total, 1);
        },
    });
   if (res.code == 200) {
       let uInt8Array = base64toBlob(res.data.base64);
       let blob = new Blob([uInt8Array]);
       let url = URL.createObjectURL(blob);
       let anchor = document.createElement("a");
       anchor.href = url;
       anchor.download = res.data.fileName;
       anchor.click();
       URL.revokeObjectURL(url);
       percentage.value = 1;
   } else {
       percentage.value = 0;
       console.log("下载出错！");
   }
}
```

这样，在下载中我们就可以依靠 `onDownloadProgress` 回调函数来获取下载进度。而且全程没有任何服务器资源链接会被暴露了，go 后端可以几乎完全控制下载细节，防止任何漏网之鱼。

但是这么做有几个巨大的缺点：

1. 后端需要一次性读入一个文件，如果文件很大，那么服务器的内存资源可能会吃不消（这个时候你就需要造轮子了，什么分包下载，内存复用等等，但是会浪费你打游戏的时间）
2. 前端负载大：从代码中可以看到，前端代码需要获取 `res.data.base64` ，这意味着当前页面的 V8 解释器需要向用户的操作系统申请 `res.data.base64` 这么大的内存。不同的浏览器的 V8 引擎实现不一样，从而申请一个很大的内存的行为不一样，如果申请失败会导致下载失败。如果申请成功，也会占用很大的内存。而且使用 JS 实现，总归没有浏览器的内嵌 HTTP 实现快速。

总而言之，这么做虽然安全了，但是计算机的执行效率降低了。真是鱼与熊掌不可兼得，不是吗？

<div align=center>
<img src="https://picx.zhimg.com/80/v2-6e2eb9d47e74a9e550662bd1da378088_1440w.jpeg" style="width: 40%;"/>
</div>



那么有没有什么优雅的方法呢？

---

## 优雅的下载方法：symbol link 创建临时下载链接

### 原理讲解

首先我们要明确一点，如果想要尽可能高效地完成下载，我们最好要借助默认的下载行为，也就是让服务器提供一个可供浏览器直接下载的服务器资源链接，但是我们又不希望这个链接在其他地方被使用，听着似乎很矛盾，但是我找到了一种方法可以实现它，而且实现方法非常简单，那就是通过 nginx + 临时软链接 的方式。

做法非常简单，假设现在用户向服务器请求下载，这个时候，服务器可以先创建一个临时的软链接指向真实的资源所在的路径，然后将这个软链接在 nginx 代理中表示的路径返回给前端，并且在前端开始下载的时候，直接删除这个软链接。这样，一方面，我们可以直接使用浏览器下载静态资源的优化策略，另一方面，我们可以保证这个下载链接只能在有限时间内被使用（比如5秒）。

> 友情提示：只要下载链接被建立，就算删除软链接也不会影响下载进程。

### 代码实现 go + javascript

代码实现非常简单，首先是后端代码：

```go

func DownloadFile(c *gin.Context) {
	// 防高频中间件
    ...
    
    // 鉴权中间件
    ...
    
    // 安全层中间件
    ...
    
    fileKey := c.Query("key")
    
    // 根据 fileKey 得到需要传给前端的文件路径和文件名
    filePath := ...
	fileName := ...

    safeLinkName := uuid.New().String()
    safeLinkPath := filepath.Join(symbolLinkDir, safeLinkName)
    
    err := os.Symlink(filePath, safeLinkPath)
    if err != nil {
        fmt.Println("fail to create symbol link %s -> %s", filePath, safeLinkPath)
        c.JSON(http.StatusOK, gin.H{ "code": 4001, "data": "fail to create share link" })
        return
    }
    
    downloadLink := fmt.Sprintf("https://hostname/下载链接/%s/%s", symbolLinkDirName, safeLinkName)
    
    c.JSON(http.StatusOK, gin.H{ "code": 200, "data": downloadLink })

    // 5 秒后删除链接
    go func ()  {
        time.Sleep(5 * time.Second)
        os.Remove(safeLinkPath)
    }()
}
```

而且软链接的创建成本并不高，就算是高并发场景，这套方案也能适配。

接下来是前端代码：

```javascript
...
// 假设前端的 axios 函数为 reqDownloadFile
const res = await reqDownloadFile({key});
if (res.code === 200) {
    // 通过某种方式获取下载文件名
    fileName = ...;
    
    const anchor = document.createElement('a');
    anchor.href = res.url;
    anchor.download = fileName;
    document.body.appendChild(anchor);
    anchor.click();
    
    // 销毁
    document.body.removeChild(anchor);
}
...
```

我们可以简单写个网站来测试一下效果，我先随便找个文件下载一下：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-470587d9a840b1ed1f756f4216395576_1440w.png" style="width: 60%;"/>
</div>



打开“完整下载记录”，可以看到 Chrome 隐藏了真实下载链接：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-580e48001d1351c8d07cb406a4cc054e_1440w.png" style="width: 60%;"/>
</div>



但是总有热心观众会右击下载链接，复制下载链接，然后放到浏览器里面去，这时你会发现你得到了一个 404，这是因为后端的软链接已经被删除了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-7734e547ab6828449bfe8eb3840696e6_1440w.png" style="width: 60%;"/>
</div>



bingo！如此，我们就完成了一个优雅的文件传输。
