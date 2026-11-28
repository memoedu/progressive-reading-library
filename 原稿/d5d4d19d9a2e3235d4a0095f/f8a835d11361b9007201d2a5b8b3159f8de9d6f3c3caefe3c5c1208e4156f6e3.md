最近刚好和几个老朋友准备玩玩MC叙叙旧，结果发现第三方的服务非常不靠谱，既然如此，那么为什么不自己搭建一个minecraft的服务器呢？

简单尝试了一下，发现非常简单，我的服务器是x86架构的CentOS 7的，其他的linux服务器的搭建方法应该都差不多。假设你已经有了一台云服务器，并且你和你的朋友的PC上已经下载了java版本的mc（我是从xbox平台上购买下载的版本）。下面都以1.19.1版本为例子。

接下来，首先我们需要下载对应版本的server，前往 https://mcversions.net/ 然后找到对应版本的server jar，如下图。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-e4b985f5501aaa253ec0838324f99568_1440w.png" style="width: 70%;"/>
</div>

将下载得到的server.jar传送到你的服务器。然后情况稍稍有点复杂：如果你的mc版本低于等于1.7，那么要确保你的服务器上有jdk-8，否则，推荐使用jdk-16。java的运行时是向下兼容的，由于我的版本比较新，我就直接下载完整的java19了。

> 很多云服务器的默认仓库yum install或者apt install的jdk版本会是jdk-11，此时不建议卸载该版本，推荐局部安装一个新的java。

由于我的服务器上已经存在了jdk-11，并且上面还装载了我的neo4j数据库，我不能卸载它，所以可以局部安装java19。前往 https://www.oracle.com/java/technologies/downloads/ 下载对应版本的java。如果你的服务器也是x86架构的，直接下载 x64 Compressed Archive 就行。把下载完成的压缩包扔到你的服务器上。

ssh到你的服务器，先解压java19：

```bash
tar -xf jdk-19_linux-x64_bin.tar.gz
```

假设你解压出来的java文件夹为jdk-19.0.1，那么接下来测试一下这玩意儿能不能直接用：

```bash
./jdk-19.0.1/bin/java --help
```

没报错说明java19局部安装成功。

然后同目录下创建一个1.19.1，把之前的server.jar扔进去。然后我们开始启动它：
```bash
cd 1.19.1
../jdk-19.0.1/bin/java -Xms1024M -Xmx1024M -jar server.jar nogui
```

其中`Xms`代表初始分给java虚拟机的内存堆区，Xmx代表java虚拟机可拥有的最大内存堆区，根据服务器的硬件条件量力而行。根据测试，最小1024调成会稳定一些。太小mc server可能就无法启动了。当然这两个参数在允许的条件下肯定是越大越好，更大的内存能存下更多的地图数据，地图生成算法也可以空间换时间，加快地区加载速度。

第一次运行会生成server必要的配置文件，并报错退出，你会在server.jar的目录下得到这些文件

```
banned-ips.json      eula.txt   ops.json           usercache.json  world
banned-players.json  libraries  server.jar         versions
crash-reports        logs       server.properties
```

然后根据报错提示，你需要同意MC的eula协议，打开eula.txt，将eula的false改成true即可。

然后再次启动server：

```bash
../jdk-19.0.1/bin/java -Xms1024M -Xmx1024M -jar server.jar nogui
```

等待地图的初始化和创建，等到看到Done的字样出现在控制台上时，就说明创建完成了。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-4930891982d04976e1a90c6da5420864_1440w.png" style="width: 50%;"/>
</div>

等一下！现在你还无法通过填入公网IP的方式接入你的服务器，因为大部分的云服务器厂商都做了防火墙，java版本的MC的默认联网接口**25565**是没有出现在放行端口列表里面的，所以在enjoy yourself之前，请前往云服务器厂商的网站，为你的端口25565放行，比如我使用的是腾讯云，放行界面如下：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-ec4c2d1d66aed6f3f579c51050df84b1_1440w.png" style="width: 70%;"/>
</div>

放行后，你就可以将你的IP分享给你的好朋友，然后开始愉快地玩耍了。

---

关于server地一些属性，可以在server.properties里面改，这里面定义了地图种子，难度，白名单啥啥啥等一系列游戏属性，关于server.properties的介绍，此处就不再赘述了，也比较简单。