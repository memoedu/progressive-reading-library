最近在使用实验室服务器时，发现有朋友可能会让几个任务堆在一张卡上，导致负载不均衡，于是简单写了一个脚本用来自动挑选当前利用率最低的GPU优先执行。

脚本如下：

```bash
best_cudas() {
    cuda_usage_sorts=$(nvidia-smi --query-gpu=utilization.gpu --format=csv,noheader | nl | sort -n -k 2)
    
    OLD_IFS=$IFS
    IFS=$' \t\n'

    cuda_array=($cuda_usage_sorts)

    device_string=""

    while read element; do
        nl=${element:0:1}
        trim_nl=$[nl - 1]
        device_string+="$trim_nl,"
    done <<< "${cuda_array[*]}"

    IFS=$OLD_IFS

    export CUDA_VISIBLE_DEVICES="${device_string%,}"
    echo "set cuda devices to ${device_string%,}"

    echo $CUDA_VISIBLE_DEVICES
}
```

> 使用该脚本会让后续的python程序优先选取利用率最低的显卡进行cuda装载。

---

## 安装方法

### bash

打开`~/.bashrc`，将上面的函数复制到文件末尾，然后`source ~/.bashrc`。

### zsh

打开`~/.zshrc`，将上面的函数复制到文件末尾，然后`zsh`。

---

## 使用方法

```bash
best_cudas
```

out:

```
set cuda devices to 0,1,2,3,4,5,6,7
```

