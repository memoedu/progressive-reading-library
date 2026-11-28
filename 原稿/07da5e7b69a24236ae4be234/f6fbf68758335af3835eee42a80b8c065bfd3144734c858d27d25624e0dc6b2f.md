## 前言
这是一篇我在几年前在知乎上写的文章： https://zhuanlan.zhihu.com/p/501477368

因为知乎的搜索功能做得太烂，最近需要使用时又在寻找。所以转录到这边。

---

## 正题
Go语言这些年对设计空间的缩减带来的开发维护效率大家有目共睹，大家也都知道Gin是Go语言下的一个轻量级框架（所以你会发现作为一门年轻语言的年轻框架，Gin的源码解读文章那么多）。那么作为小小白，我们应该如何快速使用Gin构建一个能够基础的后端呢？这篇文章就非常浅显得给出一个简单的使用Gin搭建后端的小案例。

### 项目结构
使用Gin搭建后端的结构非常非常简单，我们先来创建一下项目结构：

<div align=center>
<img src="https://pica.zhimg.com/v2-31d8a53bff83734d3e19366d0deca6a2_r.jpg" style="width: 80%;"/>
</div>

解释一下各个模块的含义：

- `main.go`	入口文件
- `router`	定义路由的地方
- `controller`	定义每个路由的处理函数


### 第零步：初始化项目
为了让Go能够自动导入库等操作，任何一个Gin项目在开始之前都需要进行初始化，由于本项目文件夹就是GinExample，所以我们的mod名就叫GinExample：

```bash
go mod init GinExample
```

整个项目中只有gin是外部包，我们先在router中引入，由于项目很简单，所以router文件夹下我们只创建router.go一个文件，并import 一下gin：

```go
package router

import (
	"github.com/gin-gonic/gin"
)
```

更新一下mod：

```bash
go mod tidy
```

下载gin完成后，项目初始化完成。

------

一个路由的定义什么的其实很简单，不过各个模块是紧耦合的，所以下面的步骤在完成之前，IDE会报错。锦恢只是尝试用最少的文字讲解一个路由的搭建过程。

### 第一步：创建路由创建函数

我们先在router.go中编写整个路由创建的入口函数，函数名随便起：

```go
package router

import (
	"github.com/gin-gonic/gin"
)

func StartRouter() *gin.Engine {
	router := gin.Default()
	
	return router
}
```

如此，一个最简单的入口就写完了，我们在main.go中导入它：

```go
package main

import "GinExample/router"

func main() {
	router := router.StartRouter()
	router.Run(":8001")
}
```

> 其中，Run的参数就是允许的端口号。

如此，我们就完成了一个最最简单的Gin后端，虽然它什么都做不了。因此我们需要在router.go中定义一些路由，通过gin.Engine的Group方法可以定义分组路由，我们常常用于有公共前缀的路由中，请多多使用这种方式创建路由，后期维护会很简单。我们尝试来写一个用于测试的/api/me路由，类型为GET：

```go
package router

import (
	"net/http"
	"github.com/gin-gonic/gin"
)

func StartRouter() *gin.Engine {
	router := gin.Default()
	api := router.Group("/api")
	{
		api.GET("/me", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"code" : 200, "msg" : "hello world!"})
		})
	}
	return router
}
```

最后别忘了把这些加入router.go中，由于这里比较简单就不使用分组路由了：

```go
package router

import (
	"github.com/gin-gonic/gin"
	"GinExample/controller/api1"
)

func StartRouter() *gin.Engine {
	router := gin.Default()

	router.GET("/api/me", api1.Me)
	router.GET("/api/add", api1.Add)

	return router
}
```

我们先测试一下，回到[根目录](https://zhida.zhihu.com/search?content_id=199571231&content_type=Article&match_order=1&q=根目录&zhida_source=entity)，输入：

```bash
go build .
./GinExample
```

运行结果：
<div align=center>
<img src="https://pic1.zhimg.com/v2-a548f864f0649c1c22acfd8d73540188_r.jpg" style="width: 80%;"/>
</div>

打开Postman，我们来给8001端口的api/me发给GET请求：
<div align=center>
<img src="https://pic1.zhimg.com/v2-22d04cdf097d2c86e03c66b57bcc5dc0_r.jpg" style="width: 80%;"/>
</div>
可以看到，数据包返回了，说明我们路由构建成功了。

---

### 第二步：定义[路由处理函数](https://zhida.zhihu.com/search?content_id=199571231&content_type=Article&match_order=1&q=路由处理函数&zhida_source=entity)

有读者可能觉得很奇怪：明明刚才的例子就已经可以完整地完成路由的创建了，为什么还需要controller文件夹？

[不要忘了](https://zhida.zhihu.com/search?content_id=199571231&content_type=Article&match_order=1&q=不要忘了&zhida_source=entity)，我们刚刚只有一个路由，而且处理机制非常简单，就是直接返回JSON包。如果我们项目到后期有一堆的路由，而且好多路由的实现都需要上百行呢？那么router.go就会变得非常臃肿、难以维护。因此，比较优雅的做法是把路由定义在controller文件夹下，而且一个分组路由一个文件夹，让controller的文件树和总路由和各个子路由的关系树完全相同，比如我们下面尝试在api路由下完成两个路由——me和add。

先创建相应的文件夹和文件：

```bash
mkdir controller/api1
touch controller/api1/me.go
touch controller/api1/add.go
```

我们先写me.go，非常简单，和刚才一样：

```go
package api1

import (
	"net/http"
	"github.com/gin-gonic/gin"
)

func Me(c *gin.Context) {
	c.JSON(http.StatusOK, gin.H{
		"code" : 200,
		"msg" : "hello world, I am gin",
	})
}
```

再写一下add，这里我们规定add是通过params传参的，所以我们利用Query进行[参数查询](https://zhida.zhihu.com/search?content_id=199571231&content_type=Article&match_order=1&q=参数查询&zhida_source=entity)：

```go
package api1

import (
	"net/http"
	"strconv"

	"github.com/gin-gonic/gin"
)

func Add(c *gin.Context) {
	a := c.Query("a")
	b := c.Query("b")
	a_value, a_err := strconv.Atoi(a)
	b_value, b_err := strconv.Atoi(b)
	if a_err != nil || b_err != nil {
		c.JSON(http.StatusOK, gin.H{
			"code" : 4001,
			"msg" : "error when parsing a or b!",
		})
		return
	}

	result_value := a_value + b_value
	c.JSON(http.StatusOK, gin.H{
		"code" : 200,
		"data" : result_value,
	})
}
```

回到根目录直接运行main.go：

```bash
go run main.go
```

到Postman中验证一下，先验证api/me：

<div align=center>
<img src="https://pica.zhimg.com/v2-3c1611858088d6e99c5ba6bfb4beb68a_r.jpg" style="width: 80%;"/>
</div>

成功！然后我们看看api/add接口，我们通过/api/add?a=1&b=2 来运行一个1+2：

<div align=center>
<img src="https://pic3.zhimg.com/v2-07e73f5e3adbefcc9cdbb56dd3647702_r.jpg" style="width: 80%;"/>
</div>

返回的data为3，成功了。那么如果我们传入无法解析的字符串呢？我们通过/api/add?a=hello&b=world来验证一下：

<div align=center>
<img src="https://pic2.zhimg.com/v2-aeb926adb10bef893fb3257ad5ce4637_r.jpg" style="width: 80%;"/>
</div>

可以看到，错误也是正常返回了。

------

## 总结

可以看到，使用Gin构建一个最基本的后端服务是非常简单优雅的。当然，这次锦恢并没有涉及数据库，后续的文章中也会涉及到gin如何和数据库交互的内容，gin外链数据库的方式也是非常优雅的。