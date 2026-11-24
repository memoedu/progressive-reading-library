## 问题描述

我运行了一个 springboot 的项目，内部使用了 SLF4J（Simple Logging Facade for Java）记录日志，但是启动服务后，输出到 powershell 上的中文却乱码了：

```
2024-04-25 23:00:08.086  INFO 28248 --- [           main] c.s.config.WebMvcConfiguration           : 寮€濮嬫敞鍐岃嚜瀹氫箟鎷︽埅鍣?..

2024-04-25 23:00:08.390  INFO 28248 --- [           main] org.redisson.Version                     : Redisson 3.13.6
2024-04-25 23:00:08.588  INFO 28248 --- [sson-netty-4-13] o.r.c.pool.MasterPubSubConnectionPool    : 1 connections initialized for /118.25.174.59:6379
```

这很明显是字符集不符合导致的。我尝试了常规的调整字符集的方法，比如

```bash
chcp 65001
```

但是并没有解决问题。

---

## 问题解决

使用 powershell 的对象方式调整字符集就可以了：

```bash
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
```

这个应该是因为 SLF4J 底层采用 Log4J 重定向到 powershell 的字符编码是 gbk，所以只是调整 powershell 的编码集是没有用的。要直接调整 console out 这个 buffer 的字符集。

最终成功展示中文日志：

```
2024-04-26 00:53:57.907  INFO 8852 --- [           main] c.s.config.WebMvcConfiguration           : 开始注册自定义拦截器...
2024-04-26 00:53:58.173  INFO 8852 --- [           main] org.redisson.Version                     : Redisson 3.13.6
2024-04-26 00:53:58.353  INFO 8852 --- [sson-netty-4-20] o.r.c.pool.MasterPubSubConnectionPool    : 1 connections initialized for /118.25.174.59:6379
```