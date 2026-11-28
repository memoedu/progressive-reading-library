## MongoDB 和 Java 驱动的版本关系

参考 https://www.mongodb.com/docs/drivers/java/sync/v4.6/compatibility/ 中所说，一共有两张表格：

- Java Driver Version 和 MongoDB 版本号对应
- Java Driver Version 和 Java 的版本对应

目前新版本的 Java21 顶多支持到 4.5 + ，推荐至少使用 4.x 的版本进行开发。



## 安装 MongoDB

- [Ubuntu](https://www.mongodb.com/zh-cn/docs/manual/tutorial/install-mongodb-on-ubuntu/#std-label-install-mdb-community-ubuntu)
- [Docker](https://www.mongodb.com/zh-cn/docs/manual/tutorial/install-mongodb-community-with-docker/)

### 使用 apt 安装

https://www.mongodb.com/zh-cn/docs/manual/tutorial/install-mongodb-on-ubuntu/#std-label-install-mdb-community-ubuntu

### Docker

```bash
docker run --name mongodb -p 27017:27017 -d mongodb/mongodb-community-server:latest
```

安装特定版本的：

```bash
docker run --name mongodb -p 27017:27017 -d mongodb/mongodb-community-server:7.0-ubuntu2204
```





## 安装 MongoDB Shell（mongosh）

mongosh 是一个用于基于命令行的 mongodb 客户端，可以非常方便地连接数据库进行简单的操作和检查。

在这里下载安装：https://www.mongodb.com/try/download/shell

安装后，可以进行验证：

```bash
mongosh --port 27017
```

进入交互界面后(默认为test)，输入：

```bash
test> db.runCommand( { hello: 1 } )
```

返回结果：

```json
{
  isWritablePrimary: true,
  topologyVersion: {
    processId: ObjectId('66262ff35ba3d82b7c58042f'),
    counter: Long('0')
  },
  maxBsonObjectSize: 16777216,
  maxMessageSizeBytes: 48000000,
  maxWriteBatchSize: 100000,
  localTime: ISODate('2024-06-29T07:48:59.894Z'),
  logicalSessionTimeoutMinutes: 30,
  connectionId: 147,
  minWireVersion: 0,
  maxWireVersion: 21,
  readOnly: false,
  ok: 1
}
```

说明数据库工作正常。