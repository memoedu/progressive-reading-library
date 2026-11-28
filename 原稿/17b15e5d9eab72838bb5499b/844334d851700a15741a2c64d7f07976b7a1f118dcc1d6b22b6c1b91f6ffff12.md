最近需要帮朋友搭建SpinalHDL环境，所以涉及到了Scala的安装，正好写一下博客，记录一下。

个人认为Ubuntu上安装Scala，其实很简单。

<div align=center>
<img src="https://picx.zhimg.com/80/v2-1e7705dc839f5876aed7cf5863c9da11_1440w.jpeg"/>
</div>

## 安装java了吗？

首先的首先，你需要先安装Java，并确保JAVA_HOME被添加到了环境变量中：

```bash
$ java -version
openjdk version "11.0.20.1" 2023-08-24
OpenJDK Runtime Environment (build 11.0.20.1+1-post-Ubuntu-0ubuntu120.04)
OpenJDK 64-Bit Server VM (build 11.0.20.1+1-post-Ubuntu-0ubuntu120.04, mixed mode, sharing)
$ echo $JAVA_HOME                                                                                                                                                                            
/usr/lib/jvm/java-11-openjdk-amd64
```

## 下载 scala 解释器

先找到Scala位于Github上的release仓库：https://github.com/lampepfl/dotty/tags

找个稳定的版本进去，然后移动到最下面，找到压缩包（Scala的官方安装包是即开即用的，不需要编译也不需要安装）：


<div align=center>
<img src="https://pic1.zhimg.com/80/v2-ea7e70beaa068b0cd62a8bff061d3fa6_1440w.png" style="width: 80%;"/>
</div>

复制上述红框框中的链接，然后在Ubuntu中输入：

```bash
$ cd ~/Download
$ wget https://github.com/lampepfl/dotty/releases/download/3.2.2/scala3-3.2.2.tar.gz
$ tar -xvf scala3-3.2.2.tar.gz
$ sudo mv scala3-3.2.2 /usr/local/scala
```

阁下需要的scala解释器就是`/usr/local/scala/bin/scala`

## 加入环境变量

因此，我们需要将scala的真实路径放到环境变量中，打开你的`.bashrc`或者`.zshrc`，加入如下的语句：


```bash
export SCALA_HOME=/usr/local/scala
export PATH=$PATH:$SCALA_HOME/bin
```

刷新一下你的终端，然后测试一下scala解释器：

```bash
$ scala
Welcome to Scala 3.2.2 (11.0.20.1, Java OpenJDK 64-Bit Server VM).
Type in expressions for evaluation. Or try :help.

scala> 1 + 1
val res0: Int = 2
```

bingo !

---

## 下载构建工具 sbt

sbt是Scala上最常用的构建工具，如果下载了Scala，一般情况下我都会建议你下载这款构建工具，安装方式的网页在这里：https://www.scala-sbt.org/download.html

我这里长话短说：

```bash
$ cd ~/Download
$ vim download_sbt.bash
```

写入如下内容：

```bash
echo "deb https://repo.scala-sbt.org/scalasbt/debian all main" | sudo tee /etc/apt/sources.list.d/sbt.list
echo "deb https://repo.scala-sbt.org/scalasbt/debian /" | sudo tee /etc/apt/sources.list.d/sbt_old.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add
sudo apt-get update
sudo apt-get install sbt
```

保存退出后，输入：
```bash
$ source download_sbt.bash
```

现在就可以使用sbt了，但是第一次使用初始化时间较长，你可以先让它自动下载一些工具包：

```bash
$ sbt
downloading sbt launcher 1.9.4
copying runtime jar...
[info] [launcher] getting org.scala-sbt sbt 1.9.4  (this may take some time)...
```

等到上述过程都结束后，就算是安装完成了。