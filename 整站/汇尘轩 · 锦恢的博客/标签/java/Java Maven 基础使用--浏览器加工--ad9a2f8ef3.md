## 前言

好久没碰 Java 了，最近在做一个数据平台的项目，虽然后端不是我操刀，但是作为整个项目的负责人，我需要了解项目的每一个细节，所以简单写一下这个项目用到的 maven.


:::info
[maven-3.9.8 安装链接](https://dlcdn.apache.org/maven/maven-3/3.9.8/binaries/apache-maven-3.9.8-bin.zip)
maven 的部分插件不支持 jdk > 20 的版本，推荐安装 Java SE 20 
:::
---

## pom.xml

这个文件记录着当前 maven 项目使用的依赖库的基本信息，类似于 npm 的 package.json。配合 maven 指令，有如下的基本用法

```bash
# 寻找当前目录的 pom.xml 并执行依赖库更新
mvn install

# 根据 pom.xml 中的项目组织关系进行编译
mvn compile

# 根据 pom.xml 将项目编译成 jar 编译到与 pom.xml 同级的 target 文件夹中
mvn package

# 清除 target 文件夹
mvn clean
```

与 package.json 不同的是，pom.xml 还支持嵌套构建：

```
📁XXX-App
   ├─📁XXX-common
   │  ├─📁src
   │  └─🏷️pom.xml
   ├─📁XXX-VO
   │  ├─📁src
   │  └─🏷️pom.xml
   ├─📁XXX-server
   │  ├─📁src
   │  └─🏷️pom.xml
   └─🏷️pom.xml
```

最外层的 pom.xml 中定义了内部的三个文件夹的模块：

```xml
<project>
    ...
    <modules>
        <module>XXX-common</module>
        <module>XXX-VO</module>
        <module>XXX-server</module>
    </modules>
    ...
</project>
```

这样，在使用 `mvn` 指令时，maven 会根据 pom.xml 的 modules 数组递归执行所有 `mvn xxx` 指令。 

---

## 使用 maven 编译并运行 springboot 项目

大部分人用 java 的 maven 也是就是构建后端应用，如何使用命令行无论在开发还是部署都是很重要的事情（本人不喜欢用大型IDE，只用 vim 和 vscode）。

在 pom.xml 中加入 springboot 运行插件：

```xml
<project>
    ...
	<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
    ...
</project>
```

然后在命令行中输入如下的指令就能进行编译和运行了：

```powershell
mvn compile; mvn spring-boot:run
```

> 如果遇到不同步的错误，记得使用 mvn clean 清除后再运行上面的指令。

---

## maven 换源

如果速度太慢，可以给 maven 更换阿里源，找到 maven 安装目录的 `conf/settings.xml` 文件，找到里面的 mirrors 标签，增加一项：

```xml
  <mirrors>
     <mirror>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <mirrorOf>central</mirrorOf>
    </mirror>
  </mirrors>
```