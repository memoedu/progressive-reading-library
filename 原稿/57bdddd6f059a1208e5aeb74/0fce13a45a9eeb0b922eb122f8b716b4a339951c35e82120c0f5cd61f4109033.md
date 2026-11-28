## 前言

Web 上面主流的持久化存储方式分为如下三种：

- cookie：用于记录用户状态，最多只能存储 4kb 的数据，而且只能存储 string。
- web storage：包括 `localStorage` 和 `sessionStorage`，最多只能存储 4.98MB 的数据（不同浏览器情况不同，safari 2.49M），而且只能存储 string。遇到普通对象需要使用 `JSON.stringify` 进行转化；如果遇到复杂对象，需要转换成 base64 进行存储，效率低下。
- indexDB：存储容量可达到 200 MB 以上，最大存储容量为用户本地磁盘的 50% 左右（有的浏览器甚至允许 100% 的用于磁盘空间）。存储的方式不再是键值对，而是和 SQL 表完全一致，使用方法更加繁琐（以 transection 的方式进行，而且需要初始化），但是支持任意类型的数据存储和更加精致和高效的查询方式。

有关这三者更加详细的不同，可以参考这篇文章：[[TypeScript]前端存储方案介绍及IndexedDB的使用封装](https://juejin.cn/post/7163839445375025189)

这篇文章记录一个简单的适用于 ts 的 indexDB 封装。顺带说一句，我的网站上的这只可爱的小猫娘就是通过 indexDB 缓存在了你们的电脑上。

---

## 封装代码

```typescript
import LAppDefine from "./lappdefine";

export function initialiseIndexDB(dbName: string, version: number, storeName?: string): Promise<IDBDatabase | undefined> {
    return new Promise((resolve, reject) => {
        if (window.indexedDB === undefined) {
            resolve(undefined);
        }
        const dbRequest = indexedDB.open(dbName, version);
        dbRequest.onsuccess = (event) => {
            const db = (event.currentTarget as IDBOpenDBRequest).result;
            resolve(db);
        }
        dbRequest.onerror = (event) => {
            const error = (event.currentTarget as IDBOpenDBRequest).error;
            console.log('[live2d] 打开 indexDB 错误. ' + error.message);
            resolve(undefined);
        }
        dbRequest.onupgradeneeded = (event) => {
            const db = (event.currentTarget as IDBOpenDBRequest).result;
            if (storeName) {
                if (!db.objectStoreNames.contains(storeName)) {
                    const store = db.createObjectStore(storeName, { autoIncrement: true });
                    store.createIndex('url', 'url', { unique: true });
                    store.createIndex('arraybuffer', 'arraybuffer');
                }
            }
        }
    });
}

export function selectItemIndexDB<T>(key: string, value: string): Promise<T | undefined> {
    return new Promise((resolve, reject) => {
        const tx = LAppDefine.Live2dDB.transaction('live2d', 'readonly');
        const store = tx.objectStore('live2d');
        const queryRequest = store.index(key).get(value);
        queryRequest.onsuccess = (event) => {
            const result = (event.currentTarget as IDBOpenDBRequest).result;
            if (result) {
                resolve(result as T);
            } else {
                resolve(undefined);
            }
        }

        queryRequest.onerror = (event) => {
            resolve(undefined);
        }
    });
}

export function createItemIndexDB<T>(value: T): Promise<boolean> {
    return new Promise((resolve, reject) => {
        const tx = LAppDefine.Live2dDB.transaction('live2d', 'readwrite');
        const store = tx.objectStore('live2d');
        const queryRequest = store.add(value);

        queryRequest.onsuccess = () => resolve(true);
        queryRequest.onerror = () => resolve(false);
    });
}
```

其中下面代码的地方定义了主键和 head，请根据具体项目自行改写：

```typescript
const store = db.createObjectStore(storeName, { autoIncrement: true });
store.createIndex('url', 'url', { unique: true });
store.createIndex('arraybuffer', 'arraybuffer');
```

两行 `createIndex` 定义了如下的表格：

| url（主键） | arraybuffer |
| --- | --- |
| https://xxx | ArrayBuffer | 
| https://yyy | ArrayBuffer | 

---

## 使用方法

首先，你需要在项目的入口处进行数据库的初始化，并将数据库对象挂载到全局对象上以便后续访问：

```typescript
// 初始化名为 db 的数据库，如果这个数据库中没有名为 live2d 的表，则创建
const db = await initialiseIndexDB('db', 1, 'live2d');
globalSetting.Live2dDB = db;
```

插入数据的例子：

```typescript
interface UrlDBItem {
    url: string
    arraybuffer: ArrayBuffer
}

const url = 'https://kirigaya.cn/blog/article?seq=170';
const arraybuffer  = new ArrayBuffer();

createItemIndexDB<UrlDBItem>({ url, arraybuffer });
```

查询数据的例子：

```typescript
const url = 'https://kirigaya.cn/blog/article?seq=170';
const item = await selectItemIndexDB<UrlDBItem>('url', url);
```
