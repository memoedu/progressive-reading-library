## 前言

最近需要使用 es 完成某个项目，使用过程中发现 es 是真的好用。

虽然我们主要是通过 Java 来操控 es 的，但是有时候，我们需要进行一些简单的批量操作，比如导入数据，新建表格什么的，还是需要一个比较好用的脚本的，幸好 python 是有 es 的库的，先安装一下：

```bash
pip install elasticsearch
```

---

## 数据库连接

第一步当然是连接我们的数据库了，基于 http basic auth 的 es 数据库连接如下。

- 如果你的是无密码连接，只需要把 `basic_auth` 字段去除即可。
- 如果你是基于 https 的，只需要把 `schema` 改成 `https `即可

```python
from elasticsearch import Elasticsearch
import yaml

config = yaml.load(open('./config/ElasticSearch.yml', 'r'), Loader=yaml.Loader)

ES_HOST = config['host']
ES_PORT = config['port']
USERNAME = config['username']
PASSWORD = config['password']

es = Elasticsearch(  
    [{'host': ES_HOST, 'port': ES_PORT, 'scheme': 'http'}],
    basic_auth=(str(USERNAME), str(PASSWORD))
)  

if es.ping():  
    print('Yes, I can connect to Elasticsearch!')  
else:  
    print('No, I cannot connect to Elasticsearch!')
```

输出：

```
Yes, I can connect to Elasticsearch!
```

这样，就可以顺利连接 es 数据库了。

---

## 创建 index （数据表）

index 是 es 中的基础容器，类似于 sql 中的表，创建一个 index 需要指定 index 的名称和数据项的 schema。假设我们现在一个数据项的示例长这样：

```json
{
    "id": 53,
    "name": "ADSD",
    "status": 0,
    "createTS": 12312231312,
    "modifyTS": 12312231312,
}
```

如果想要创建存储这样的数据项的 index， python 代码如下：

```python
index_name = "mdata-background-management"

mappings = {
    "properties": {  
        "id": {  
            "type": "long"  
        },  
        "name": {  
            "type": "text",  
            "fields": {  
                "keyword": {  
                    "type": "keyword",  
                    "ignore_above": 256
                }  
            }  
        },  
        "status": {  
            "type": "integer"  
        },  
        "createTS": {  
            "type": "long"  
        },  
        "modifyTS": {  
            "type": "long"  
        }  
    }
}

if not es.indices.exists(index=index_name):
    res = es.indices.create(
        index=index_name,
        body={
            'mappings': mappings
        }
    )
    print(f'create {index_name}, result: {res}')
else:
    print(f'{index_name} already exists!')
```

输出：

```
Yes, I can connect to Elasticsearch!
create mdata-background-management, result: {'acknowledged': True, 'shards_acknowledged': True, 'index': 'mdata-background-management'}
```

---

## 往 index 中加入/修改数据项

使用 python 操作 es 时，批量往 es 中导入数据项应该是最常用的一个步骤了。

基础使用和 java 中的 es 使用差不多，我们先把需要导入的数据载入内存再说：

```python
import json
storage = json.load(open('./data/storage.json', 'r')).get('storage', [])
print(len(storage))
```

out

```
77
```

一共 77 项数据。我们需要把它们全部导入 es，基础使用如下：

```python
index_name = "mdata-background-management"

for data in storage:
    now_ms = int(time.time() * 1000)
    body = {
        'id': int(data['id']),
        'name': data['name'],
        'status': 0,
        'createTS': now_ms,
        'modifyTS': now_ms
    }
    
    es.index(
        index=index_name,
        id=str(id), # es 中不存在主键的概念，但是有着和图数据库一样的 数据项 id（字符串），可以用这个做主键
        body=body
    )
```

可以看到，es 使用起来还是比较简单的。

---

## 查询

## 根据 id 查询

此处的 id 指数据项自己的内建的 id，而不是用户创建的，用户创建的 id 除了标识唯一性，并没有加速查找的作用。

```python
document_id = 53
response = es.get(index=index_name, id=document_id)  
  
if response['found']:  
    source: dict = response['_source']  
    print(source)
else:  
    print(f"Document with ID {document_id} not found.")
```



### 根据某个字段进行搜索（精准搜索）

es 允许和一般的数据库一样，对某些字段匹配进行搜索

```python
index_name = "mdata-background-management"

query = {  
    "query": {  
        "match": {  
            "name": "data"  
        }  
    }  
}

search_response = es.search(index=index_name, body=query)
for hit in search_response['hits']['hits']:
    source: dict = hit['_source']
```





### 根据某个字段进行搜索（模糊搜索）

字段模糊查询是 es 最大的特色

```python
index_name = "mdata-background-management"

query = {  
    "query": {  
        "wildcard": {  
            "name": {  
                "value": "*medical*",  # 使用*作为通配符，例如 "*john*" 会匹配到包含john的任何词  
                "boost": 1.0  		   # 可以设置boost值来调整查询的权重，但通常是可选的  
            }  
        }  
    }  
}

search_response = es.search(index=index_name, body=query)
for hit in search_response['hits']['hits']:
    source: dict = hit['_source']
```





### 查询所有

```python
index_name = "mdata-background-management"

query = {
    'query': {
        'match_all': {}
    }
}

search_response = es.search(index=index_name, body=query)
for hit in search_response['hits']['hits']:
    source: dict = hit['_source']
```