## 前言

最近给我的 Lagrange.RagBot，简称 TIP 系统，做了一套 walktalk 系统，用于收集用户的 bug report 数据。我在想，如何保存这些数据，这次，我就不再只是使用 json 了，那样实在太不优雅了，这次，我打算玩儿点新的玩具，比如说文档数据库 elasticsearch（简称 es）。

:::info
es 我也不是第一次玩啦，早在 ShareMiracle 项目初期，我就采用了 es 存储了不同医学数据的元信息，并加以整理。之前使用 python 操作过 es 数据库：[elastic search (es) python 基础使用](https://kirigaya.cn/blog/article?seq=246)。只不过，当初的 es 并不是我亲手起起来了，是师兄帮我起的，现在在我自己的服务器上，我打算自己起一个。
:::

---

## es 安装

老规矩，还是只展示 Linux 上的

```bash
cd ~/package
wget -c https://www.elastic.co/docs/deploy-manage/deploy/self-managed/install-elasticsearch-from-archive-on-linux-macos
tar -xvf elasticsearch-9.0.0-linux-x86_64.tar.gz
```

先通过启动完成初始化：

```bash
cd elasticsearch-9.0.0-linux-x86_64
bin/elasticsearch
```

启动后，会生成一串密码，这是 es 的根用户「elastic」的密码，记录如下两个环境变量：

```bash
export ES_HOME=$HOME/package/elasticsearch-9.0.0
export PATH=$PATH:$ES_HOME/bin
export ES_PASSWORD=<刚刚看到的密码>
```

如果不记得了，可以通过下面的命令重置并获取新的密码：

```bash
bin/elasticsearch-reset-password -u elastic
```

这样，就完成了基本的安装。

---

## 了解你的 es

对于刚刚启动的 es，我们可以测试一下连通性

```bash
curl --cacert $ES_HOME/config/certs/http_ca.crt -u elastic:$ES_PASSWORD https://localhost:9200
```

得到如下输出说明成功：

```json
{
  "name" : "dust-server",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "EwPS00rdStKQoeZgGLFilA",
  "version" : {
    "number" : "9.0.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "112859b85d50de2a7e63f73c8fc70b99eea24291",
    "build_date" : "2025-04-08T15:13:46.049795831Z",
    "build_snapshot" : false,
    "lucene_version" : "10.1.0",
    "minimum_wire_compatibility_version" : "8.18.0",
    "minimum_index_compatibility_version" : "8.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

:::info
es 默认开在 9200 端口上，默认使用 `$ES_HOME/config/elasticsearch.yml` 进行配置。es 初始具有一个名为 `elastic` ，密码为 `$ES_PASSWORD` 的根用户。
:::

不同语言都提供了 es 的 sdk，es 也是目前支持最为广泛的通用型 nosql 数据之一，因此不必担心找不到资料的问题。

---

## 通过 Kibana 进行可视化 & 管理

作为数据库应用，管理UI肯定是少不了的，es 官方提供了 Kibana——一款致力于数据分析的 dashboard 作为 es 的前端界面，进行管理。

先下载它：

```bash
cd ~/package
wget -c https://artifacts.elastic.co/downloads/kibana/kibana-9.0.1-linux-x86_64.tar.gz
tar -xvf kibana-9.0.1-linux-x86_64.tar.gz && cd kibana-9.0.1-linux-x86_64
```

进入后，通过下面的指令启动它：

```bash
bin/kibana
```

这样默认会在 http://localhost:5601/ 打开一个 web 服务进行可视化。

:::info 服务器看不到UI怎么办？
推荐在 vscode 中利用 remote ssh 连接到服务器，在 vscode 的终端中开启 kibana，remote ssh 会自动帮你转发出去，这样本地就能看到界面了。
:::

刚刚进入界面需要等待 kibana 的初始化，等待完成后。需要输入一个访问 token，在服务器上通过下面的命令可以产生访问 token：

```bash
cd $ES_HOME
bin/elasticsearch-create-enrollment-token --scope kibana
```

输入 es 的根用户账号和密码就能进入（es 初始具有一个名为 `elastic` ，密码为 `$ES_PASSWORD` 的根用户）。

进入页面后，也能看到我们创建过的访问 token：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-4e8b698a92f0fd3f72561a3133d746e3_1440w.png" style="width: 100%;"/>
</div>

---

## 后台启动

确定连接无误后，我们把刚刚两个服务关闭，然后放在后台运行吧，这里你可以随意选择你的托管器，我使用 pm2，它的教程如下：[超级好用的进程管理器 pm2 使用教程](https://kirigaya.cn/blog/article?seq=233)。

上述两个服务，分别进入它们的目录后，使用如下命令运行：

```bash
cd ~/package/elasticsearch-9.0.0
pm2 start /usr/bin/bash --name es -- bin/elasticsearch

cd ~/package/kibana-9.0.1
pm2 start /usr/bin/bash --name kibana -- bin/kibana
```

最后重复一下它们的默认端口：

- es：localhost:9200
- kibana：localhost:5601

---

## typescript 操作 es 数据库

使用 typescript 操作上面启动的 es 数据库的一个基本流程如下.

```bash
mkdir typescript-es-test && cd typescript-es-test
npm init -y
npm install typescript @types/node --save-dev
npm install @elastic/elasticsearch
npm install ts-node -g
tsc --init
```

创建 main.ts，内容如下：

```typescript
import { Client } from '@elastic/elasticsearch';

// 创建 Elasticsearch 客户端
const client = new Client({
    node: 'https://localhost:9200', // ES 地址
    auth: {
        username: 'elastic',
        password: process.env['ES_PASSWORD'] || '', 
    },
    tls: {
        rejectUnauthorized: false,
    }
});

// 示例：创建一个索引
async function createIndex(indexName: string) {
    try {
        const response = await client.indices.create({
            index: indexName,
        });
        console.log('Index created:', response);
    } catch (error) {
        console.error('Error creating index:', error);
    }
}

// 示例：插入文档
async function indexDocument(indexName: string, document: any) {
    try {
        const response = await client.index({
            index: indexName,
            body: document,
            // 刷新索引以确保文档立即可用
            // 如果只是异步插入数据，注释这里
            refresh: 'wait_for'
        });
        console.log('Document indexed:', response);
    } catch (error) {
        console.error('Error indexing document:', error);
    }
}

// 示例：搜索文档
async function searchDocuments(indexName: string, query: any) {
    try {
        const response = await client.search({
            index: indexName,
            body: {
                query: query,
            },
        });
        console.log('Search results:', response.hits.hits);
    } catch (error) {
        console.error('Error searching documents:', error);
    }
}

// 示例：删除索引
async function deleteIndex(indexName: string) {
    try {
        const response = await client.indices.delete({
            index: indexName,
        });
        console.log('Index deleted:', response);
    } catch (error) {
        console.error('Error deleting index:', error);
    }
}

// 测试代码
(async () => {
    const indexName = 'my_index';
    const document = { title: 'Test Document', content: 'This is a test document.' };
    const query = { match: { title: 'Test Document' } };

    // 如果设置了 es 的配置
    // action.auto_create_index: true 那么就不需要手动创建 index
    await createIndex(indexName);

    await indexDocument(indexName, document);
    await searchDocuments(indexName, query);
    await deleteIndex(indexName);
})();
```

预期输出：

```js
Index created: { acknowledged: true, shards_acknowledged: true, index: 'my_index' }
Document indexed: {
  _index: 'my_index',
  _id: 'TcnDyZYBOcccBkLBA46U',
  _version: 1,
  result: 'created',
  _shards: { total: 2, successful: 1, failed: 0 },
  _seq_no: 0,
  _primary_term: 1
}
Search results: [
  {
    _index: 'my_index',
    _id: 'TcnDyZYBOcccBkLBA46U',
    _score: 0.5753642,
    _source: { title: 'Test Document', content: 'This is a test document.' }
  }
]
Index deleted: { acknowledged: true }
```