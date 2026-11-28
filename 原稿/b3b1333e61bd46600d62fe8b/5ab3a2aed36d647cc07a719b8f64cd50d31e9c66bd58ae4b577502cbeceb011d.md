## mongodb 的性能问题

最近实习，我们负责一个搭建数据管线的项目，数据我们都通过 mongodb 进行存储和管理，在可视化的时候遇到了一些数据库查询上的小问题。在此记录一下。

具体来说，这个数据库里面存储着上百万的数字资产的元信息，在前端进行分页渲染的时候，比如现在正在第 10000 页，那么翻到第 10001 页的时候就非常非常慢，实测下来需要消耗越 2000ms，在使用体验上这是几乎不可接受的。

那么如何进行优化呢？

---

## skip 的性能问题

首先，我们需要知道，当我们在后端查询 mongodb 的时候，往往是写成这个形式：

```typescript
const query = { /** 你的查询条件  */ };
const items = await this.collection.find({ ...query })
    .limit(pageSize)
    .skip((page - 1) * pageSize);
```

其中 pageSize 就是一页的数量，我们这里一页是 15 个资产； page 就是当前要的页数，是 zero-count 的。

如果 `skip()` 内的参数比较小，还行，如果 skip 的数量比较大的时候（实测大概 > 8000 的时候），上面的查询会有很严重的性能问题。因为 mongodb 这类 nosql 在执行 limit-like 操作的时候，往往会顺序扫描整个文档，比如当我们执行 `skip(1000)` 的时候，游标就会从第一个文档开始往后扫描，直到找到第 1001 个文档。数据量特别大的时候，这个扫描是造成性能瓶颈的主要原因。

因此，后续的优化中，我们尽量不要使用 skip。

---

## mongodb 分页查询调优

通过上面的认知，你应该知道了，我们在数据量特别大时，分页查询不应该使用 skip。那么如何优化呢？一个最常规，也最有用的就是通过 range query 来实现。

认识什么是 range query 之前，你首先得知道 mongodb 内部默认返回数据是如何运作的。

当我们每次在 mongodb 中插入数据时，mongodb 会自动为每个文档添加一个 `_id` 字段，这个 `_id` 字段是一个 12 字节的 BSON 类型，它的组成如下图所示：

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-85c1cbacc438d33c73fcb8620e224c2f_1440w.png" style="max-width: 100%;"/>
<p class="img-caption">source: https://prashant007.medium.com/what-is-objectid-in-mongodb-2eaf7fae0d7</p>
</div>

你只需要知道，因为这个 id 的存在，所以 mongodb 在没有建立额外联合索引的情况下查询返回的顺序和插入顺序是一致的。这说明 id 是递增的。

因此，在分页查询的场景下，id 的递增让我们可以执行 range query。基本做法也非常简单：每次获取到分页查询的时候，前端将上一次查询的最大 id 保存下来成为 lastId，往后翻页的时候（为了简单起见，这里假设只往后翻一页），将这个 lastId 也传递给后端。

后端拿到 lastId 的时候，直接通过 `{ _id: { $gt: lastId } }` 来查询，就能比 skip 更快，直接跳到下一页数据所在的地方。这样就完成了优化。

一个非常简单的实例代码如下所示：

```typescript
async listAllByDirection(options: IListOption = {}): Promise<any[]> {
    const { /** 查询参数 */ } = options;

    let sort = 1;

    if (direction === 'prev') {
        sort = -1;
        matchStage._id = { $lt: new ObjectId(firstId) };
    } else if (direction === 'next') {
        sort = 1;
        matchStage._id = { $gt: new ObjectId(lastId) };
    }

    const pipeline: any[] = [];
    
    pipeline.push(
        { $match: matchStage }
    );

    pipeline.push(
        { $sort: { _id: sort } }
    );

    // 这里的 skip 由前端处理，如果是连续跳页，那么 skip 就是 0
    pipeline.push(
        { $skip: skip }
    );

    pipeline.push(
        { $limit: limit }
    );

    pipeline.push(
        {
            $project: {
                _id: 1,
                /** 返回其他的字段 */
            }
        }
    );
    
    const items = await this.collection.aggregate(pipeline).toArray();

    // prev 的情况下，之前的 sort 是 -1 所以还需要调转一下
    if (direction === 'prev') {
        items.reverse();
    }
    
    return items;
}
```

执行完这段优化后，原本 10000 页到 10001 页的跳转需要消耗 2000ms，但是现在只需要消耗 20ms，性能被我提升了 100倍，可喜可贺，可喜可贺。

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-585f647b081f36f581e74798d90b9cee_1440w.jpeg" style="max-width: 100%;"/>
</div>

---

## 参考

- [Why is MongoDB's skip() so slow and bad to use, while MySQL's LIMIT is so fast?](https://stackoverflow.com/questions/20718929/why-is-mongodbs-skip-so-slow-and-bad-to-use-while-mysqls-limit-is-so-fast)
- [优化查询性能](https://docs.mongoing.com/mongodb-crud-operations/mongodb-crud-concepts/query-optimization/optimize-query-performance)
- [Pagination in MongoDB: The Only Right Way to Implement it (Avoid Common Mistakes)](https://codebeyondlimits.com/articles/pagination-in-mongodb-the-only-right-way-to-implement-it-and-avoid-common-mistakes)