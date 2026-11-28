在跑分布式深度学习时，作为玩具级的工具， `torchrun` 虽然性能很垃圾，但是很多时候我们又不得不用。

## torchrun 基本使用

torchrun 有两种使用方法

```bash
# 兼容性比较好，但是比较古老
python -m torch.distributed.run [torchrun 参数] train.py [train.py 参数]

# 比较新，推荐
torchrun [torchrun 参数] train.py [train.py 参数]
```

需要注意，在使用参数时，`torchrun` 的参数写在前面， 脚本自己的参数写在后面。

---

## torchrun 基本参数

`--nproc_per_node`：卡的数量

`--master_port`：主节点通信进程端口，默认为 29500

`--master_addr`：主节点通信进程占用地址，默认为 localhost

`--node_rank`：进程编号，默认/只有一个服务器的情况下为 0

`--nnodes`：机器数量，默认/只有一个服务器的情况下为 1