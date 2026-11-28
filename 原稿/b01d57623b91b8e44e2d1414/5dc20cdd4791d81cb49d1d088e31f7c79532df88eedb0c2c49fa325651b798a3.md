## 前言

在 [这篇文章](https://kirigaya.cn/blog/article?seq=246) 中，我们简单讲解了一下如何通过 python 来使用 es 进行简单的数据库操控。作为 es 原生态的主力军，真的想要大规模开发喝部署肯定是需要使用 java 的。介于网上关于 java springboot 下使用 es 的文章，大部分都过时或者完全错误，本文我将花点时间简单讲解 8.x 的新版本下要如何在 java 中优雅地使用 elastic 来进行 es 数据库的操作。

---

## 新版 API 和老版 API

es 给 Java 的 API 一共分为两种：Java REST Client 和 Java API Client 。

在 es 6.x 和 es 7.x 中，都是采用 Java REST Client 提供 API，它们的画风是这样的：

```java
SearchSourceBuilder sourceBuilder = new SearchSourceBuilder(); 
sourceBuilder.query(QueryBuilders.termQuery("user", "kimchy")); 
sourceBuilder.from(0); 
sourceBuilder.size(5); 
sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS)); 
```

新版本的 es 8.x 中，采用了新式的 Java API Client，使用起来更加简约，它们的画风是这样的：

```java
GetResponse<MdataMetaStatus> mdataRes = esClient.get(i -> i
	.index("user")
	.id("53")
	.from(0)
	.size(5),
	MdataMetaStatus.class);
```

 推荐使用后者，原因有如下几点：

- Java API Client 的写法更加简单，把之前构造请求的过程放进了 lambda 函数中
- Java API Client 广泛应用于其他可以操作 es 的编程语言中中，比如 python 的 elasticsearch 库使用起来就和 Java API Client 几乎完全一样。

新版本 Java API Client 的官方文档： https://www.elastic.co/guide/en/elasticsearch/client/java-api-client/current/installation.html

---

## 建表

es 中的index相当于sql中的表。要建立一个名为 products 的索引操作如下：

```java
esClient.indices().create(c -> c
    .index("products")
);
```

---

## 插入/更新文档

es 中的每一个数据项被我们称为文档，它相当于一个自由结构的 json 或者说 js 对象。

es 中插入文档和更新文档都被我们称为 index。当我们插入文档时，可以手动指定当前文档的全 index 唯一 id，这样方便我们后面去修改和搜索。

比如我们现在需要插入新的用户，文档如下：

```json
{
    "id": 0,
    "name": "kirigaya"
}
```

代码如下：

```java
// 构造数据，真实场景下，序列化层会帮我们把前端的数据构
// 造成一个 java 对象，不需要我们自己创建
Account account = new Account();
account.setId(0);
account.setName("kirigaya");

IndexResponse response = esClient.index(i -> i
    .index("users")
    .id(product.getId())
    .document(account)
);
```

假设当我们需要修改 User 数据结构，增加一个密码项：

```json
{
    "id": 0,
    "name": "kirigaya",
    "password": "123"
}
```

当我们插入文档时，需要指定 id，es 会根据我们输入的文档自动更新已有的那个。对于下面这种更新，es 是支持：

```java
Account account = new Account();
account.setId(0);
account.setName("kirigaya");
// 真实情况下我们不会将用户的密码存入数据库
// 存入的都是hash过的值，只做验证，此处只是演示
account.setPassword("123");

IndexResponse response = esClient.index(i -> i
    .index("users")
    .id(product.getId())
    .document(account)
);
```



除了使用 `index`，官方还提供了 `update` 方法来更新文档，下面是 [官方例程](https://www.elastic.co/guide/en/elasticsearch/client/java-api-client/current/getting-started-java.html#_searching_documents)

```java
// This is how you can update a document, for example to add a new field:
Product product = new Product("bk-1", "City bike", 123.0);

esClient.update(u -> u
        .index("products")
        .id("bk-1")
        .upsert(product),
    Product.class
);
```



---

## 根据 id 检索文档

通过 `get` 可以根据 id 获取到对应的文档对象。

用法如下，下面的代码会返回一个检索到的数据响应。其中 `ElasticSearchConstant.MdataMetaManageDB` 是我用常量层管理的 index 名。

```java
GetResponse<MdataMetaStatus> mdataRes = esClient.get(i -> i
	.index(ElasticSearchConstant.MdataMetaManageDB)
	.id(String.valueOf(elasticSearchItemDTO.getId()))
	, MdataMetaStatus.class);

log.info("create mdata {}", elasticSearchItemDTO.toJson());
```

得到检索响应后，需要判断它是否找到了指定 id 的数据：

```java
if (!mdataRes.found()) {
    // 通过 source 方法，可以将文档从响应中序列化成 java 对象
    MdataMetaStatus status = mdataRes.source();
    log.info("name: {}, id: {}", status.getName(), status.getId());
}
```

---

## 根据字段进行检索

下面的例程说明了在 `products` 这个 index 中搜索 name 为 bike 的文档。 

```java
String searchText = "bike";

SearchResponse<Product> response = esClient.search(s -> s
        .index("products")
        .query(q -> q
            .match(t -> t
                .field("name")
                .query(searchText)
            )
        ),
    Product.class
);
```



---

## 删除

### 删除文档

```java
// Deleting documents
esClient.delete(d -> d.index("products").id("bk-1"));
```



### 删除 index

简称删库跑路，最好还是不要在项目中放入这个。

```java
// Deleting an index
esClient.indices().delete(d -> d
    .index("products")
);
```

---

## 数据聚合

聚合是数据库中的一个常用概念，意为获取数据的指标，度量或者其他的统计量。

比如最常用的聚合就是获取满足某一个要求的数据的数据量，类似于 sql 语句中的 count 聚合，这在构建网页内容，视频呈现中的分页器时必不可少。

比如前端用户现在想要搜索满足某个标签的文章，我们记当前的搜索可抽象为 `query`，那么为了正确显示搜索结果一共有多少页，需要后端在初次搜素 `query` 时，给出对应的搜索总数。es 8.x 中可以这么写：

```java
// 自己构造一个 es 的 query，或者直接写在 .query 里面也行
// 此处是因为 query 比较复杂，为了复用和代码可读性，单独写成了一个函数
Query query = this.getESSearchQuery(searchDTO);

CountRequest countRequest = CountRequest.of(c -> c
	.index(ElasticSearchConstant.MdataMetaDB)
	.query(query)
);
CountResponse countResponse = esClient.count(countRequest);

long count = countResponse.count();
```









