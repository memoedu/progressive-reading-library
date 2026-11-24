写了快三个月的TypeScript，深感语言的强大，它让我连续写了5000多行代码后可以一次性通过测试，真的太爽了，其中TypeScript强大的类型标注和vscode的TypeScript类型推导器起到了至关重要的作用。为了尽可能使用强大的类型推导器，学习TypeScript的朋友都需要学会如何进行类型体操，这将允许我们用更高的自由度来进行类型标注和约束。

类型体操做得层次越深，代码出bug的概率越小，但是可读性也越差，一般而言，我顶多做到二阶约束。

对于TypeScript的基本语法和标注语法，不再赘述，如果你想要练习类型体操，不妨挑战一下type challenge：[https://link.zhihu.com/?target=https%3A//github.com/type-challenges/type-challenges/](https://link.zhihu.com/?target=https%3A//github.com/type-challenges/type-challenges/)

这篇文章，我将从我个人实践的角度出发，罗列一写我使用类型体操的场景，以供笔者和读者后续参考。

---

## Scene.1 类型组合

对于常规的业务场景，我们经常会碰到部分对象有着相同的属性成员，比如下面这种情况：

```javascript
interface Price {
    name: string
    value: number
    default: number
}

interface Car {
    name: string
    value: string
    default: string
}


interface Login {
    value: boolean
    default: boolean
}
```


这三个类型都有共同的成员name, value, default。如果类型写得非常多，那么代码中就会存在大量冗余的属性描述，维护起来也会很麻烦。我们可以使用&进行类型合并，修改后的代码如下：

```javascript
type Name = { name: string };
type Value<T> = { value: T };
type Default<T> = { default: T };

type Price = Name & Value<number> & Default<number>;
type Car = Name & Value<string> & Default<string>;
type Login = Value<boolean> & Default<boolean>;
```

这样，就能实现和之前完全一样的效果，而且代码量也下去了，对于复杂，且拥有大量共通属性的对象描述，用这种方法可以节省开发时间，并且减小设计空间。

---

## Scene.2 继承与反继承

如果我们在编写类型时，为了复用和建立类型的关联约束来更好地管理项目，通常会使用extends来继承类型。举个例子，我们正在编写一个图形库，图形基类为Shape，一个Circle类型必须继承自Shape，我们就可以这么写：

```javascript
interface Shape {
    name: string
    color: string
    stroke: string
}

interface Circle extends Shape {
    radius: number
}

const circle: Circle = {
    name: 'circle_1',
    color: '#9999ff',
    stroke: '#9999ff',
    radius: 2
};
```


问题来了：实际上用户创建的Circle对象是会缺省的，基类Shape的属性不能随意修改，因为牵一发而动全身，那么如何优雅地打上补丁来规范用户Circle的类型呢？一个最鲁莽的做法就是完全新建一个RawUserCircle类型，定义其中哪些属性是缺省的，但是这样就违背继承的初衷了，如果我们修改Shape或者Circle的定义，这个RawUserCircle也要跟着一起修改，太麻烦了。那么有什么办法从Circle类上反继承得到一个属性更加弱化的RawUserCircle吗？

这里就需要引入TypeScript的泛型了，使用一阶泛型就能做出一些约束：

```javascript
type OptionalPickType<T> = {[P in keyof T]?: T[P]};
```


这个泛型类型传入一个类型，输出一个类型，用于让输入类型的每个属性都变得可选，我们使用这个泛型类型就能实现Circle的反继承了：

```javascript
type OptionalPickType<T> = {[P in keyof T]?: T[P]};
type RawUserCircle = OptionalPickType<Circle>;

// 只需要输入radius而不报错
const rawUserCircle: RawUserCircle = {
    radius: 3
};
```

---


## Scene.3 描述结构自相似的类型

在实现一些存在树状结构的管理模块时，我们经常会遇到存在结构自相似的数据结构，比如下面这个JavaScript对象：

```javascript

const config = {
    Launch: {
        cmd: 'digital-ide.hard.launch',
        icon: 'cmd',
        tip: 'Launch FPGA development assist function'
    },
    Simulate: {
        cmd: 'digital-ide.hard.simulate',
        icon: 'toolBox',
        tip: 'Launch the manufacturer Simulation',
        children: {
            CLI: {
                cmd: 'digital-ide.hard.simulate.cli',
                icon: 'branch',
                tip: 'Launch the manufacturer Simulation in CLI'
            }
        }
    },
    Build: {
        cmd: 'digital-ide.hard.build',
        icon: 'toolBox',
        tip: 'Build the current fpga project',
        children: {
            Synth: {
                cmd: 'digital-ide.hard.build.synth',
                icon: 'branch',
                tip: 'Synth the current project'
            }
        }
    },
};
```

通过观察，不难看出，这个对象的值是一个有成员cmd, icon, tip和children，其中children可缺省，且children的数据结构和外层对象完全一样。要如何描述这种类型呢？

很简单，让type递归就行：

```javascript

type CommandConfig = Record<string, {
    cmd: string,
    icon: string,
    tip: string,
    children?: CommandConfig,
}>;
```


这样vscode就会自动检查我们写的config的每一层是否满足cmd, icon, tip必须存在，children可缺省的结构约束了，而且也支持任意深度的自动补全。

---

## Scene.4 通过泛型让函数的输入参数间存在约束

有大型项目中，我们有时希望某个传入自定义类型的函数可以约束函数的输入参数。

比如我们编写一个函数updateObject，它有三个参数obj，attr和value，负责将对象obj的属性attr赋值为value。很明显，这个函数的三个参数之间是存在约束：

attr必须是obj 类型中的一个属性
value的类型必须和定义attr地方的类型相同
比如传入的obj是我们前面定义的circle，那么attr就只能name，color，stroke和radius中的一个，且value必须是circle[attr]的定义类型。

这个约束在我们写updateObject(circle, )时就应该开始生效，后面两个参数vscode的eslint必须帮助我们自动补全和纠错。如何实现呢？

通过上面的分析，我们知道存在两个约束，那么我们必须引入两个泛型变量A和V，函数如下：

```javascript
type Index = string | number; // 一般情况下js对象的索引只可能是string和number

function updateObject<A extends Index, V>(obj: Record<A, V>, attr: A, value: V) {
    obj[attr] = value;
}
```

测试一下：

<div align=center>
<img src="https://pic4.zhimg.com/v2-77fab8f5d49f5860989e1ec3dd061e03_r.jpg" style="width: 80%;"/>
<p class="img-caption">自动补全工作正常</p>
</div>


<div align=center>
<img src="https://pic1.zhimg.com/v2-8822e1cead4658b784668d7f003c548c_r.jpg" style="width: 80%;"/>
<p class="img-caption">类型纠错工作正常，color属性必须是string（JS中number会自动转成string，所以也能输入number）</p>
</div>


to be continued...