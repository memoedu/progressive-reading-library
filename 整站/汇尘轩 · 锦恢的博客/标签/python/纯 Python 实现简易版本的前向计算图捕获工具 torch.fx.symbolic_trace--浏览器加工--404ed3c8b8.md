## 前言

最近知乎上有一位特殊的朋友发来求助，希望我帮助他实现 `torch.fx.symbolic_trace`，这里对不熟悉深度学习编译器的朋友科普一下什么是 symbolic_trace：

对于很多深度学习模型的部署工作，很多时候我们希望可以获取它的前向计算图（不懂什么是计算图的朋友请看我之前的文章：[计算图的原理，节点类的实现和计算图的可视化](https://zhuanlan.zhihu.com/p/351961835)），这样就能非常方便对计算图进行操作了，比如对算子进行量化，进行异构计算的自动调度（哪些算子在 CPU 上计算，哪些算子在 NPU 上计算。基本上在手机厂干模型部署的，一定会有对应的 ptsd 哈哈哈）

## symbolic_trace 超简用法

pytorch 也提供了 `torch.fx.symbolic_trace` 这个函数用于直接获取一个给定的 `nn.Module` 的前向计算图，非常方便～

假设我们有一个如下的网络：

```python
import torch

class Example(torch.nn.Module):
    def __init__(self):
        super(Example, self).__init__()
        self.conv = torch.nn.Conv2d(3, 3, 3)
        self.relu = torch.nn.ReLU()
    def forward(self, x: torch.Tensor, y: torch.Tensor) -> torch.Tensor:
        x = x / (torch.abs(y) + 1)
        x = self.conv(x)
        x = self.relu(x)
        x = x.reshape(-1)
        x = x / x.norm(p=2, dim=-1, keepdim=True)
        return x
```

symbolic_trace 提取这个网络的前向计算图只需要一行：

```python
from torch.fx import symbolic_trace

model = Example()
traced = symbolic_trace(model)
print(traced.graph)
```

打印结果：

```
graph():
    %x : torch.Tensor [num_users=1] = placeholder[target=x]
    %y : torch.Tensor [num_users=1] = placeholder[target=y]
    %abs_1 : [num_users=1] = call_function[target=torch.abs](args = (%y,), kwargs = {})
    %add : [num_users=1] = call_function[target=operator.add](args = (%abs_1, 1), kwargs = {})
    %truediv : [num_users=1] = call_function[target=operator.truediv](args = (%x, %add), kwargs = {})
    %conv : [num_users=1] = call_module[target=conv](args = (%truediv,), kwargs = {})
    %relu : [num_users=1] = call_module[target=relu](args = (%conv,), kwargs = {})
    %reshape : [num_users=2] = call_method[target=reshape](args = (%relu, -1), kwargs = {})
    %norm : [num_users=1] = call_method[target=norm](args = (%reshape,), kwargs = {p: 2, dim: -1, keepdim: True})
    %truediv_1 : [num_users=1] = call_function[target=operator.truediv](args = (%reshape, %norm), kwargs = {})
    return truediv_1
```

## 造轮子！

那么我们要如何自己实现一个这样的符号跟踪器呢？

### 基本思路 —— 用 settrace 捕获函数调用栈

锦恢此处只给出一个思路，Python 里面有一个函数，`sys.settrace`，用这个函数，我们就可以捕获到所有的函数调用栈。因为所有的算子，它最终都是一个封装好的 torch 函数，所以都会被 settrace 捕获到。

真的吗？

### settrace 捕获不了非 native 的 python 函数

`sys.settrace` 虽然看起来可以获取所有的调用栈，但是不要忘记了，torch 的后端是用 C++ 实现的，然后基于 pybind11 做成了 python 前端，对于 C Side API 实现的 C 函数，`sys.settrace` 是捕获不到的。比如 torch 的加减乘除这些原子操作，都是 C Side 函数，它们本身的实现完全基于 C，只是暴露对外的 API 供 python 使用（对如何使用 C语言 给 python 函数实现后端的，可以看我之前的博客 [学会优雅地使用C\\C++拓展Python模块的正确/官方姿势](https://zhuanlan.zhihu.com/p/479371538)）

不相信的话，可以打印一下源代码，python 中，可以通过 `inspect.getsource` 获取对应函数或者 class 的源代码：

```python
import inspect
print(inspect.getsource(torch.nn.Conv2d))
```

打印：

```

class Conv2d(_ConvNd):
    __doc__ = (
        r"""Applies a 2D convolution over an input signal composed of several input
    planes.
...
```

但是如果打印 `torch.add`，会直接报错，你们可以自己试试。

### 创造代理对象来捕获非 native 的 python 函数

那么有什么方法可以捕获到这些 C API 函数吗？有的兄弟有的，看过 vue.js 源代码的朋友都知道，vue 通过 Proxy 对象的方式代理了所有变量，使得响应属性得到了传播，并对链路上所有的变量注册了对应的渲染管线（不熟悉 vue 源代码的朋友可以查看我之前的博客 [从 vue3 源码开始的响应式数据 debug](https://kirigaya.cn/blog/article?seq=319)），其实在这一点上，我们是可以借鉴的（其实，`torch.fx.symbolic_trace` 就是这么实现的，叮～），我们只需要给所有的 tensor 注册一个代理对象，就能完成所有操作的代理，下面是我简单实现的一个：

```python
class ProxyTensor:
    __array_priority__ = 1000

    def __init__(self, tracer, node, value=None):
        self.tracer = tracer
        self.node = node
        self.value = value

    @classmethod
    def __torch_function__(cls, func, types, args=(), kwargs=None):
        if not any(issubclass(t, ProxyTensor) for t in types):
            return NotImplemented
        kwargs = kwargs or {}

        def unwrap_any(x, tracer):
            if isinstance(x, ProxyTensor):
                return x.node
            if isinstance(x, (list, tuple)):
                return type(x)(unwrap_any(e, tracer) for e in x)
            if isinstance(x, dict):
                return {k: unwrap_any(v, tracer) for k, v in x.items()}
            # 如果是原始 torch.Tensor 或数字/其他，交给 tracer.unwrap 处理（tracer.unwrap 会返回 node 或原样返回）
            try:
                return tracer.unwrap(x)
            except Exception:
                return x

        tracer = None
        for a in args:
            if isinstance(a, ProxyTensor):
                tracer = a.tracer
                break
        if tracer is None:
            for v in kwargs.values():
                if isinstance(v, ProxyTensor):
                    tracer = v.tracer
                    break
        if tracer is None:
            return NotImplemented

        # 统一为所有 op 使用 tracer.unwrap 风格的展开，避免漏掉原始 tensor
        mapped_args = [unwrap_any(a, tracer) for a in args]
        mapped_kwargs = {k: unwrap_any(v, tracer) for k, v in (kwargs or {}).items()}

        # 处理特殊命名：把 torch.div 映射到 operator.truediv，保持和 torch.fx 一致
        if func.__name__ == "div":
            n = tracer.graph.call_function(operator.truediv, mapped_args, mapped_kwargs)
            return ProxyTensor(tracer, n)

        # 常见单函数直接使用 func（例如 torch.abs）——我们也用 mapped_args/mapped_kwargs
        if func is torch.abs or getattr(func, "__name__", None) == "abs":
            n = tracer.graph.call_function(torch.abs, mapped_args, mapped_kwargs)
            return ProxyTensor(tracer, n)

        # 其他未实现的 op 打印并报错（保持你原先的行为）
        print(f"__torch_function__ not implemented for {func}")
        raise NotImplementedError(f"__torch_function__ not implemented for {func}")
    def __add__(self, other):
        other_node = self.tracer.unwrap(other)
        n = self.tracer.graph.call_function(operator.add, [self.node, other_node])
        return ProxyTensor(self.tracer, n)

    def __radd__(self, other):
        return self.__add__(other)
    
    def __truediv__(self, other):
        other_node = self.tracer.unwrap(other)
        n = self.tracer.graph.call_function(operator.truediv, [self.node, other_node])
        return ProxyTensor(self.tracer, n)

    def __rtruediv__(self, other):
        other_node = self.tracer.unwrap(other)
        n = self.tracer.graph.call_function(operator.truediv, [other_node, self.node])
        return ProxyTensor(self.tracer, n)

    def __abs__(self):
        n = self.tracer.graph.call_function(torch.abs, [self.node])
        return ProxyTensor(self.tracer, n)

    def reshape(self, *shape):
        n = self.tracer.graph.call_method("reshape", [self.node] + list(shape))
        return ProxyTensor(self.tracer, n)

    def norm(self, *args, **kwargs):
        n = self.tracer.graph.call_method("norm", [self.node] + list(args), kwargs)
        return ProxyTensor(self.tracer, n)
```

目前是只支持加减乘除，abs，reshape，norm 等操作的代理，tracer proxy 对 OP 的代理是工程量极大的一件事，所以我们此处只当是学习。实际工程中我不建议你们自己造轮子。

## 开始编写 Tracer

了解了上面的基本原理后，我们不难写出如下的简单代码：

```python
import sys
import types
import torch
import torch.nn as nn
import operator
from typing import Any, Dict, List, Optional, Tuple, Union
import inspect

class Node:
    def __init__(self, op, target, args=None, kwargs=None, name=None, users=None):
        self.op = op
        self.target = target
        self.args = args or []
        self.kwargs = kwargs or {}
        self.name = name
        self.users = users or 0  # 跟踪使用次数

    def _fmt(self, obj):
        # 打印时：Node → 只用 name，字面量保留原样
        if isinstance(obj, Node):
            return f'%{obj.name}'
        if isinstance(obj, (list, tuple)):
            return "[" + ", ".join(self._fmt(x) for x in obj) + "]"
        if isinstance(obj, dict):
            return "{" + ", ".join(f"{k}: {self._fmt(v)}" for k, v in obj.items()) + "}"
        if isinstance(obj, str):
            return f"'{obj}'"
        return repr(obj)

    def __repr__(self):
        args_str = ", ".join(self._fmt(a) for a in self.args)
        if self.kwargs:
            kwargs_str = ", " + ", ".join(f"{k}: {self._fmt(v)}" for k, v in self.kwargs.items())
        else:
            kwargs_str = ""
            
        # 格式化输出以匹配 torch.fx
        if self.op == "placeholder":
            return f"    %{self.name} : torch.Tensor [num_users={self.users}] = placeholder[target={self.target}]"
        elif self.op == "output":
            return f"    return {self._fmt(self.args[0]).lstrip('%')}"
        else:
            # 对于 call_function/call_module/call_method
            op_map = {
                "call_function": "call_function",
                "call_module": "call_module",
                "call_method": "call_method"
            }
            target_str = self.target if isinstance(self.target, str) else getattr(self.target, "__name__", str(self.target))
            return f"    %{self.name} : [num_users={self.users}] = {op_map[self.op]}[target={target_str}](args = ({args_str}), kwargs = {{{kwargs_str[2:] if kwargs_str else ''}}})"


class Graph:
    def __init__(self):
        self.nodes = []
        self.param_counters = {}

    def placeholder(self, name):
        node = Node("placeholder", name, name=name)
        self.nodes.append(node)
        return node

    def output(self, val):
        node = Node("output", "output", args=[val], name="output")
        self.nodes.append(node)
        # 增加输出节点的使用计数
        if isinstance(val, Node):
            val.users += 1
        return node

    def _get_name(self, base):
        if base not in self.param_counters:
            self.param_counters[base] = 0
            return base
        else:
            self.param_counters[base] += 1
            return f"{base}_{self.param_counters[base]}"

    def call_function(self, target, args, kwargs=None, name=None):
        name = name or self._get_name(target.__name__ if hasattr(target, '__name__') else str(target))
        node = Node("call_function", target, args, kwargs or {}, name)
        self.nodes.append(node)
        # 增加参数节点的使用计数
        for arg in args:
            if isinstance(arg, Node):
                arg.users += 1
        for kwarg in (kwargs or {}).values():
            if isinstance(kwarg, Node):
                kwarg.users += 1
        return node

    def call_method(self, target, args, kwargs=None, name=None):
        name = name or self._get_name(target)
        node = Node("call_method", target, args, kwargs or {}, name)
        self.nodes.append(node)
        # 增加参数节点的使用计数
        for arg in args:
            if isinstance(arg, Node):
                arg.users += 1
        for kwarg in (kwargs or {}).values():
            if isinstance(kwarg, Node):
                kwarg.users += 1
        return node

    def call_module(self, target, args, kwargs=None, name=None):
        name = name or self._get_name(target)
        node = Node("call_module", target, args, kwargs or {}, name)
        self.nodes.append(node)
        # 增加参数节点的使用计数
        for arg in args:
            if isinstance(arg, Node):
                arg.users += 1
        for kwarg in (kwargs or {}).values():
            if isinstance(kwarg, Node):
                kwarg.users += 1
        return node

    def __repr__(self):
        return "graph():\n" + "\n".join(repr(n) for n in self.nodes)


class ProxyTensor:
    __array_priority__ = 1000

    def __init__(self, tracer, node, value=None):
        self.tracer = tracer
        self.node = node
        self.value = value

    @classmethod
    def __torch_function__(cls, func, types, args=(), kwargs=None):
        if not any(issubclass(t, ProxyTensor) for t in types):
            return NotImplemented
        kwargs = kwargs or {}

        def unwrap_any(x, tracer):
            if isinstance(x, ProxyTensor):
                return x.node
            if isinstance(x, (list, tuple)):
                return type(x)(unwrap_any(e, tracer) for e in x)
            if isinstance(x, dict):
                return {k: unwrap_any(v, tracer) for k, v in x.items()}
            # 如果是原始 torch.Tensor 或数字/其他，交给 tracer.unwrap 处理（tracer.unwrap 会返回 node 或原样返回）
            try:
                return tracer.unwrap(x)
            except Exception:
                return x

        tracer = None
        for a in args:
            if isinstance(a, ProxyTensor):
                tracer = a.tracer
                break
        if tracer is None:
            for v in kwargs.values():
                if isinstance(v, ProxyTensor):
                    tracer = v.tracer
                    break
        if tracer is None:
            return NotImplemented

        # 统一为所有 op 使用 tracer.unwrap 风格的展开，避免漏掉原始 tensor
        mapped_args = [unwrap_any(a, tracer) for a in args]
        mapped_kwargs = {k: unwrap_any(v, tracer) for k, v in (kwargs or {}).items()}

        # 处理特殊命名：把 torch.div 映射到 operator.truediv，保持和 torch.fx 一致
        if func.__name__ == "div":
            n = tracer.graph.call_function(operator.truediv, mapped_args, mapped_kwargs)
            return ProxyTensor(tracer, n)

        # 常见单函数直接使用 func（例如 torch.abs）——我们也用 mapped_args/mapped_kwargs
        if func is torch.abs or getattr(func, "__name__", None) == "abs":
            n = tracer.graph.call_function(torch.abs, mapped_args, mapped_kwargs)
            return ProxyTensor(tracer, n)

        # 其他未实现的 op 打印并报错（保持你原先的行为）
        print(f"__torch_function__ not implemented for {func}")
        raise NotImplementedError(f"__torch_function__ not implemented for {func}")
    def __add__(self, other):
        other_node = self.tracer.unwrap(other)
        n = self.tracer.graph.call_function(operator.add, [self.node, other_node])
        return ProxyTensor(self.tracer, n)

    def __radd__(self, other):
        return self.__add__(other)
    
    def __truediv__(self, other):
        other_node = self.tracer.unwrap(other)
        n = self.tracer.graph.call_function(operator.truediv, [self.node, other_node])
        return ProxyTensor(self.tracer, n)

    def __rtruediv__(self, other):
        other_node = self.tracer.unwrap(other)
        n = self.tracer.graph.call_function(operator.truediv, [other_node, self.node])
        return ProxyTensor(self.tracer, n)

    def __abs__(self):
        n = self.tracer.graph.call_function(torch.abs, [self.node])
        return ProxyTensor(self.tracer, n)

    def reshape(self, *shape):
        n = self.tracer.graph.call_method("reshape", [self.node] + list(shape))
        return ProxyTensor(self.tracer, n)

    def norm(self, *args, **kwargs):
        n = self.tracer.graph.call_method("norm", [self.node] + list(args), kwargs)
        return ProxyTensor(self.tracer, n)


class Tracer:
    def __init__(self, module: nn.Module):
        self.module = module
        self.graph = Graph()
        self.env = {}
        self.old_trace = None
        self.name_counter = 0
        self._patched_forwards = {}
        self._module_name_by_obj = {}

    def new_name(self, base="n"):
        self.name_counter += 1
        return f"{base}_{self.name_counter}"

    def unwrap(self, val):
        if isinstance(val, ProxyTensor):
            return val.node
        # 如果是被替换前的原始 tensor（我们在 trace 时把原始 tensor id 也写进了 self.env）
        if id(val) in self.env:
            return self.env[id(val)]
        # 对于简单数值（int/float/None/str），保持原样
        return val

    def _wrap_modules(self):
        for name, mod in self.module.named_modules():
            if mod is self.module:
                continue
            if isinstance(mod, (nn.Conv2d, nn.ReLU)):
                self._module_name_by_obj[id(mod)] = name.rsplit('.', 1)[-1]
                if mod not in self._patched_forwards:
                    orig_forward = mod.forward

                    def make_forward(m, tself=self):
                        def wrapped_forward(_self, x, *args, **kwargs):
                            target = tself._module_name_by_obj[id(_self)]
                            x_node = tself.unwrap(x)
                            node = tself.graph.call_module(target, [x_node])
                            return ProxyTensor(tself, node)
                        return types.MethodType(wrapped_forward, m)

                    self._patched_forwards[mod] = orig_forward
                    mod.forward = make_forward(mod)

    def _restore_modules(self):
        for mod, orig_fwd in self._patched_forwards.items():
            mod.forward = orig_fwd
        self._patched_forwards.clear()
        self._module_name_by_obj.clear()

    def __enter__(self):
        self._wrap_modules()
        self.old_trace = sys.gettrace()
        sys.settrace(self.tracefunc)
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        sys.settrace(self.old_trace)
        self._restore_modules()
        return self.graph

    def tracefunc(self, frame, event, arg):
        code = frame.f_code
        func_name = code.co_name

        if event == "call" and func_name == "forward" and frame.f_locals.get("self") is self.module:
            for k, v in list(frame.f_locals.items()):
                if isinstance(v, torch.Tensor):
                    n = self.graph.placeholder(k)
                    p = ProxyTensor(self, n, v)
                    frame.f_locals[k] = p
                    # 关键：同时把原始 tensor 的 id 和 proxy 的 id 都映射到 node
                    self.env[id(p)] = n
                    self.env[id(v)] = n

        if event == "return" and func_name == "forward" and frame.f_locals.get("self") is self.module:
            out_val = arg
            out_node = out_val.node if isinstance(out_val, ProxyTensor) else self.unwrap(out_val)
            self.graph.output(out_node)

        return self.tracefunc


def my_symbolic_trace(module: nn.Module, *args, **kwargs):
    """
    自动符号跟踪模块的计算图
    
    Args:
        module: 要跟踪的神经网络模块
        *args: 可选的位置参数，用于指定输入数据的形状
        **kwargs: 可选的关键字参数，用于指定输入数据的形状
    
    Returns:
        Graph: 计算图对象
    """
    # 如果未提供输入参数，则自动生成
    if not args and not kwargs:
        # 获取forward方法的参数签名
        sig = inspect.signature(module.forward)
        parameters = sig.parameters
        
        # 生成默认输入数据
        args = []
        for param_name, param in parameters.items():
            if param.default == inspect.Parameter.empty and param_name != 'self':
                # 为每个参数生成一个随机tensor，使用常见的默认形状
                if param_name in ['x', 'input', 'inputs']:
                    shape = (1, 3, 224, 224)  # 常见的图像输入形状
                elif param_name in ['hidden', 'state', 'hidden_state']:
                    shape = (1, 64)  # 常见的隐藏状态形状
                else:
                    shape = (1, 3, 32, 32)  # 默认形状
                
                args.append(torch.randn(shape))
    
    # 使用Tracer进行跟踪
    with Tracer(module) as tracer:
        output = module(*args, **kwargs)
        # except Exception as e:
        #     print(f"跟踪过程中出现错误: {e}")
        #     # 尝试使用更简单的输入
        #     simple_args = [torch.randn(1, 3, 32, 32) for _ in range(len(args))]
        #     output = module(*simple_args, **kwargs)
    
    return tracer.graph

if __name__ == "__main__":
    from example import Example

    model = Example()

    graph = my_symbolic_trace(model)    
    print("我的代码生成的计算图:")
    print(graph)

    from torch.fx import symbolic_trace
    real_traced = symbolic_trace(model)
    print("\ntorch.fx生成的计算图:")
    print(real_traced.graph)
```

运行它，就能得到如下的结果：

```
我的代码生成的计算图:
graph():
    %x : torch.Tensor [num_users=1] = placeholder[target=x]
    %y : torch.Tensor [num_users=1] = placeholder[target=y]
    %abs : [num_users=1] = call_function[target=abs](args = (%y), kwargs = {})
    %add : [num_users=1] = call_function[target=add](args = (%abs, 1), kwargs = {})
    %truediv : [num_users=1] = call_function[target=truediv](args = (%x, %add), kwargs = {})
    %conv : [num_users=1] = call_module[target=conv](args = (%truediv), kwargs = {})
    %relu : [num_users=1] = call_module[target=relu](args = (%conv), kwargs = {})
    %reshape : [num_users=2] = call_method[target=reshape](args = (%relu, -1), kwargs = {})
    %norm : [num_users=1] = call_method[target=norm](args = (%reshape), kwargs = {p: 2, dim: -1, keepdim: True})
    %truediv_1 : [num_users=1] = call_function[target=truediv](args = (%reshape, %norm), kwargs = {})
    return truediv_1
```

嗯，看起来有点那个意思了。

## 结论

改进的方向非常非常多，用户自定义函数的递归解析，更多的 OP 代理等等，都需要更多的工作量，这篇文章更多是带大家了解一下符号跟踪器的基本实现，是不是非常简单呢？