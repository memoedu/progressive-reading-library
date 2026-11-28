<div align='center'>
<iframe
height=86 width=330
src="//music.163.com/outchain/player?type=2&id=1921761273&auto=1&height=66"
></iframe>
<p class="img-caption">非常喜欢的一首歌</p>
</div>

> 简单记个笔记，这个问题其实我两年前就解决了，最近刚好想不起那个环境变量了。

问题是这样的，当你使用新版的nodejs，17.x 以上，在运行vue或者其他一些框架的新版本时，node会报错：`ERR_OSSL_EVP_UNSUPPORTED`，如下图：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-367e5361d9551db0ff765b6be497e294_1440w.png" style="width: 80%;"/>
<p class="img-caption">烦人的ssl错误</p>
</div>

原因是node v17中的OpenSSL3.0对允许算法和密钥大小增加了严格的限制，参考[https://nodejs.org/es/blog/release/v17.0.0](https://nodejs.org/es/blog/release/v17.0.0)

解决方法很简单，只需要将环境变量`NODE_OPTIONS`的值赋为：`"--openssl-legacy-provider"`即可。

只需要在`npm run serve`前运行下面的语句：
```bash
# windows powershell
$Env:NODE_OPTIONS="--openssl-legacy-provider"

# Linux & Mac
export NODE_OPTIONS="--openssl-legacy-provider"
```



