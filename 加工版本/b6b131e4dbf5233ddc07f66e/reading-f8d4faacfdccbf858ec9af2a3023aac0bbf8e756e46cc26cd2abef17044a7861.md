<!-- reading-guide:start -->

## 阅读前置卡

### 一句话核心
这是一篇由退役ACM选手写给同类人的C++就业学习路线图，核心不是罗列知识点，而是用“编译器知道更多信息→性能更好”这一主线串联起从RAII到模板、虚函数、CRTP的完整认知框架。

### 核心洞见
- 退役ACM选手的C++水平通常是“单文件C with STL”，就业导向的学习需要从编译链接、CMake、RAII等工程基础重新起步。
- C++性能优势的本质是“编译器能知道更多信息”：lambda的独有类型、sort对比qsort、虚函数加final去虚拟化，都是同一原理的不同表现。
- 学习C++存在“0.9999无限循环等于1”的分界线——越过它意味着接受了一套“C++思想”，知道为什么要这么做，而非堆砌语法。
- 就业方向选择上，C++不一定是退役ACM选手的最优解，客户端、Go、Java等差异化竞争同样可行，算法能力是通用杠杆。
- 学习路径建议：先跑通一个完整小项目（如CMU15445的P0），再进入深水区（lambda、模板、虚函数实现），最后用实现工具类、读duckdb源码、关注新特性来收尾。
- 最佳实践比语言律师更重要，遵循rule of three/five/zero等规范可以避开大部分坑。

### 对我的价值判断
**高**。这篇文章不是泛泛的C++教程，而是从“算法竞赛选手”这一特定身份出发，给出了从单文件刷题到工程就业的认知跃迁路径。其“编译器知道更多信息”这一主线极具可迁移性，可以直接转化为卡片和工作流中的核心框架，且作者提供了大量高质量中文资源链接，省去筛选成本。

### 信息密度判断
**高**。密度来自真实经验判断（ACM选手的典型短板、就业市场现实）、方法论框架（学习分界线、深水区划分）、以及具体可操作资源（每个知识点都附有高质量中文文章链接）。不是观点重复，而是有明确学习顺序和优先级判断的经验之谈。

### 能解决我的什么痛点
- 作为知识系统搭建者，需要将“C++学习路径”这类复杂主题拆解为可卡片化的认知单元，本文提供了清晰的主线（编译器信息论）和支线（各知识点）。
- 关注教育科技与课程生产，本文的“从单文件到工程”的过渡设计、以及“先跑通项目再深入原理”的顺序，是很好的课程结构参考。
- 在AI Agent工作流中，本文反复提到“让AI生成cmake项目”“问AI大人汇编”，展示了人机协作学习的具体场景，可沉淀为工作流模板。
- 对创业与产品判断而言，文中关于“C++就业方向其实分散在QT和infra”的观察，有助于理解技术人才市场的真实结构。

### 具体用处
- 将“编译器知道更多信息”作为RemNote中的核心概念卡片，关联lambda、sort/qsort、虚函数final、ranges等子卡片，形成知识图谱。
- 将文中推荐的资源链接（RAII、模板元编程、CRTP、错误处理等）逐一转化为渐进阅读的待读卡片，按文中优先级排序。
- 将“学习分界线”和“深水区划分”提炼为方法论卡片，可用于设计自己的C++学习路径或课程大纲。
- 将“先跑通P0再深入原理”的项目驱动学习模式，沉淀为AI工作流中的学习项目模板。
- 文中“C++新特性要问为什么有它、别的语言怎么做、之前怎么做”的提问框架，可直接转化为AI提示词模板。

### 是否急需阅读
**急需**。这篇文章直接命中读者正在搭建的知识系统核心——它提供了一条高密度、可卡片化的C++进阶路径，且与AI辅助学习、项目驱动、教育产品设计等多个关注点高度重合，值得立即进入渐进阅读流程。

<!-- reading-guide:end -->

---

## 原文

# 退役算法竞赛选手如何学习C++就业

一个算法竞赛选手（acmer），常用语言应该就是C++了，只不过很多acmer的C++知识大概只能说是**单文件的C with STL 。**

作为一个曾经的acmer（我的文章大部分都是各种题解的），工作了俩三年了（主要都是写C++），平常也经常水群，和各种小登们交流。学弟也向我咨询过如何学习C++（以就业为目的）。我感觉我还是可以以一个过来人的角度给出一些意见的。

![](https://pic2.zhimg.com/v2-3dfcabf1039b2b344546d59fc8913c39_r.jpg)

本文章适合人群为想找工作的退役acmer，当然广义算法竞赛蓝桥杯什么的可能也算吧。

* * *

## 前言

### 你真的要选择就业吗？

为什么不去读研/考公/出国呢？

当然大部分人的可选项就只有读研 or 就业，所以下面就只讨论这两个了。

因为环境问题，很多人担心“我读研三年，出来找的工作可能还不如本科”，但是**如果你的学历提升的影响能超过三年时间的不可确定性**，那么我其实是建议读研的，因为很多双非，二本，三本，xx学院，在目前的就业市场是很艰难的。

是是是，某个二本的xx神有多么多么的牛，直接进大厂，但那毕竟是少数，而且不是你！

如果你发现你的学长学姐，基本上只能去一些小公司，除了一些传说中的存在基本无缘大厂，那还是考虑读个研提升下学历，除非自己的技术水平真的很高。

如果多方面考虑后还是决定就业的，那就有另一个问题了。

![](https://pic2.zhimg.com/v2-8891dc64c56b6e888e7be1c599978535_r.jpg)

### **真的要选择C++作为就业语言吗？**

根据我的观察，相当一部分退役acmer选择C++就业，主要是因为路径依赖，因为打比赛就用的这个，觉得自己比较熟悉。而且网上，各种群里面，经常也能见到嘲讽Java的消息（典型的Java思维这种）。

但是其实，我认识相当一部分acmer，在退役后，光速学习java/go/前端/客户端等等的知识，最后也能进大厂。而且acmer和其他人比起来有一个巨大的优势，就是你不需要在算法题上费太多的心思，刷刷力扣的hot50这种，熟悉一下格式。所以C++可能不一定是最好的选择，你大可以去选客户端(无贬义)什么的搞一搞差异化竞争。

![](https://pic3.zhimg.com/v2-2945f012d3de90dc33104d12c08ac1ba_1440w.jpg)

目前我接触的C++相关的工作，基本上除了QT就是各种基架(编译器，hpc，数据库，存储这种)。

就，infra，非常的interesting啊。

### 学习C++难度很高

不过我不是劝退C++的，虽然网上很多言论都在说C++学习多么多么难，一个经典的说法是

**真羡慕你们这些用C++的，可以考虑很多我们不需要考虑的东西**。

但是，C++的重点就是这些需要多考虑的东西。

而且C++学习也不见得有传闻中那么难，很多“写C++文章”/“做C++视频”的答主/博主/UP主，可能都不是计算机专业的，而且还有中专生。

我有一个不恰当的类比，学习C++存在一个“0.9999无限循环等于1”的分界线，当你接受了这点，并且越过了分界线，虽然后续还需要持续的学习，但是最起码知道要如何学习了。

也就是要接受一套“C++思想”，知道为什么要这么做。

![](https://pic4.zhimg.com/v2-ab8a1bf901849a247f1206938ed5fe2f_r.jpg)

### 叠甲

我知道，就业没有C++方向这个说法。都是xxx行业用了C++。

但是考虑到大部分的ap（acmer批的简称，某种自嘲，下文可能多次提到），他就没有接触过什么行业，当然也不是ap的问题，大部分大三的学生能知道什么呢？

而且，考虑到正常的就业流程，大学生应该先去实习，实习本身就没有指望实习生会多少东西。

> **实习**，是[学生](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%25E5%25AD%25A6%25E7%2594%259F)到[企业](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%25E4%25BC%2581%25E4%25B8%259A)、[政府部门](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%25E6%2594%25BF%25E5%25BA%259C%25E9%2583%25A8%25E9%2596%2580)或其他[组织](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%25E7%25BB%2584%25E7%25BB%2587_%28%25E7%25A4%25BE%25E4%25BC%259A%25E7%25A7%2591%25E5%25AD%25A6%29)等进行实践的一个过程，目的是为以后的工作做好准备

**先保证能写出，语义明确，可读性高，性能不错的C++代码**，至于xxx行业的知识，大可以遇到了再学，现在的AI极大减少了入门的成本。

## 正文

下面我会按照顺序提一些我觉得比较重要的点，如果我之前写过相关的文章，我会优先贴上我的（因为至少我和大部分ap的上下文差不多）。

### 编译与链接

考虑到大部分ap们都只写过单问题，所以先推荐学习一下cmake（这一步可以直接让ai生成一个cmake项目）。

可以看一下这个

[C++学习——声明定义与链接](https://zhuanlan.zhihu.com/p/625815492)

### RAII

> 现代C++如果非要说是C with xxx的话，那么就是 C with RAII

可以先去把构造函数，拷贝构造函数，析构函数这些都看看（这个找找八股，或者xx教程看一下）。

[现代C++学习—— 什么是RAII](https://zhuanlan.zhihu.com/p/600337719)

[现代C++学习 —— 为什么需要std::move](https://zhuanlan.zhihu.com/p/598564624)

### 多态

也是先找个八股，xx教程学习一下虚函数的用法什么的。

然后可以研究一下他的实现。

不过我建议可以先不研究多态的实现，等后面再回来研究。

[C++中虚函数、虚继承内存模型](https://zhuanlan.zhihu.com/p/41309205)

[C++虚函数表的位置——从内存的角度](https://zhuanlan.zhihu.com/p/563418849)

[C语言实现虚函数/继承/封装](https://zhuanlan.zhihu.com/p/566782733)

### **模板**

C++的模板是不得不品尝的一环，这里推荐

[雾里看花：真正意义上的理解 C++ 模板](https://zhuanlan.zhihu.com/p/655902377)

但是可能难度对初学者有点大，可以等后面慢慢再回来看，目前可以看

[现代C++学习 模板元编程入门](https://zhuanlan.zhihu.com/p/594241569)

当然不需要看模板元编程相关的内容，只需要知道。

1.  模板可以暂时理解成，**让编译器帮我生成代码**
2.  模板生成的代码，**会保留类型信息**，我们可以利用这个信息进行一些计算与优化。

![](https://pic4.zhimg.com/v2-882746d1e8edf8b667f6d0f43b490729_r.jpg)

### 智能指针

unique\_ptr 比较好实现，shared\_ptr的话

[C++ 实现 shared\_ptr / weak\_ptr /enable\_shared\_from\_this](https://zhuanlan.zhihu.com/p/680068428)

不过不推荐上来就学实现，可以先学习一下用法。

### 尝试跑一个完整的项目

这里比较推荐cum15445的p0，把一个小项目编译，运行起来。

**只需要做p0就可以了。**

学习一下git，cmake，跑ut什么的。

* * *

以上是我认为很基础的内容了，可能也有很多ap已经掌握相关的内容了。

接下来是我认为的深水区了。

我们先回顾一下，为什么C++这门语言的性能要比其他语言好？

其中最重要的是，**编译器能知道更多的信息。**

### lambda

举个例子，用lambda来写一个dfs。

```cpp
    auto dfs = [](auto &&self, int n) -> int {
        if (n <= 1) return 1;
        return n * self(self, n - 1);
    };

    return dfs(dfs, 5);
```

放到[https://cppinsights.io/](https://link.zhihu.com/?target=https%3A//cppinsights.io/)里面看一下。

![](https://pica.zhimg.com/v2-c54dcad2370e3c9204466a0ee89182aa_r.jpg)

你会发现生成了一堆东西，因为

> 每个 lambda，**它都是一个独一无二的匿名的类型**

[为什么C++的lambda表达式要设计得这么麻烦？](https://www.zhihu.com/question/541321687/answer/2557177963)

注意这里

![](https://pic1.zhimg.com/v2-5c6f4ea055fd4dcb74cf67fc858a15e0_r.jpg)

因为我知道这个self的具体类型，所以在实际编译中，编译器可以进行优化。

比如如果我们用function来实现这个dfs编译器就**不太愿意优化**。

（如果不知道function可以看[c++函数进化史 （函数，函数指针，function，仿函数，lambda）](https://zhuanlan.zhihu.com/p/561916691)

[https://godbolt.org/z/ME6sx68de](https://link.zhihu.com/?target=https%3A//godbolt.org/z/ME6sx68de) （请擅用这个网站）

![](https://pic1.zhimg.com/v2-40c1ffa10fca112cf367a3f3f50d4334_r.jpg)

如果看不懂汇编（其实我也不太能看懂），可以直接问ai大人。

![](https://pic4.zhimg.com/v2-2635911dd82c3aba76e759d5808144bd_r.jpg)

![](https://pic3.zhimg.com/v2-7d6eac0040c8e3cf4eea7e4771302f7e_r.jpg)

### sort与qsort

一个非常老生常谈的话题是，C语言和C++的速度。

但是实际上，C++在语言层面上就比C要快（不要提手写汇编这种

例如sort（[c++函数进化史 （函数，函数指针，function，仿函数，lambda）](https://zhuanlan.zhihu.com/p/561916691)

qsort是用的函数指针

```cpp
int compare(const void* a, const void* b) {
    return (*(int*)a - *(int*)b);
}
qsort(arr, n, sizeof(int), compare);
```

而sort一般是lambda这种

```cpp
// 比较器通常是内联的函数对象
std::sort(arr.begin(), arr.end(), [](int a, int b) {
    return a < b;  // 直接比较，可以内联
});
```

那模板生成的代码，**会保留类型信息，**然后**编译器可以看到cmp的完整实现**，那就能更好的做出优化。

![](https://pic3.zhimg.com/v2-a11bc23bf22ac4e649a7c7fb28619dc6_r.jpg)

### 虚函数性能

经典问题，为什么虚函数会慢啊？

从上面的几个例子应该可以反应过来，当我使用多态的时候。

```cpp
struct Base {
    virtual void foo() = 0;
};

struct A : Base {
    void foo() override {
    }
};

struct B : Base {
    void foo() override {
    }
};

void foo(Base *p) {
    p->foo();
}
```

**编译器不知道这个p到底是指向谁的，他不太乐意进行优化。**

不过有一个操作叫去虚拟化。

[CppCon 2015 学习:C++ devirtualization in clang](https://link.zhihu.com/?target=https%3A//blog.csdn.net/TM1695648164/article/details/148558386)

[\[C++\] GCC对C++虚函数调用的一个优化](https://zhuanlan.zhihu.com/p/23233548)

省流

下面这样还是有虚函数调用

![](https://pic1.zhimg.com/v2-bf3478ad684e641c93303534f4391d10_r.jpg)

加个final，编译器知道这个foo不会有其他人重写了，就知道具体的实现了。

![](https://pica.zhimg.com/v2-f5363334d934ed01c55cbd3dd7c922c0_r.jpg)

（ps：这里可以和前面的编译链接的内容联系起来。final是属于签名的一部分，所以**别的翻译单元也需要遵守**。如果没有final，要是别的翻译单元继承一下A，重写一下foo怎么办？所以编译器只能老老实实的调用虚函数了。

回顾一下上面三个点。

1.  每个lambda的类型都不一样，编译器可以通过类型知道lambda的实现。
2.  sort和qsort相比，编译器能知道cmp的实现。
3.  虚函数可以通过final来告诉编译器指向的是哪个对象。

**这些优化都是尽可能的告诉编译器更多的信息。**

这里我们再扩展一下，C++20的ranges。

```cpp
    std::vector<int> vec = {1, 2, 3, 4, 5};
    auto squared_view = vec | std::views::transform([](int x) { return x * x; }) | std::views::filter([](int x) { return x % 2 == 0; });
```

如果看一下这个auto的类型，可以发现很复杂，他把很多东西都记录在类型里面了。

![](https://pica.zhimg.com/v2-a8de49b4c450885fae866a722916ce56_r.jpg)

这个感兴趣可以去搜索关键词

> 模板 惰性求值 表达式模版

* * *

### 了解一下其他语言的多态实现

看到这里，应该能对C++为什么跑的快有一个初步的印象了。这个时候推荐看一下别的语言的多态的实现。比如go/java这种。

这是为了加深对C++的理解。

[为什么C++实现多态必须要虚函数表？](https://www.zhihu.com/question/629627999/answer/3283280576)

这个可以后面看（因为感觉很多C++提到多态的地方都会有人说这个proxy

[C++ proxy库的多态是怎么实现的](https://zhuanlan.zhihu.com/p/22307747744) （但是现阶段可以不看

这里可以顺带学习一下值语义和引用语义，在C++中，虚函数是只有引用语义下有多态的。

[引用语义多态-vs-值语义多态](https://link.zhihu.com/?target=https%3A//www.mindthink.me/2024/01/17/%25E5%25BC%2595%25E7%2594%25A8%25E8%25AF%25AD%25E4%25B9%2589%25E5%25A4%259A%25E6%2580%2581-vs-%25E5%2580%25BC%25E8%25AF%25AD%25E4%25B9%2589%25E5%25A4%259A%25E6%2580%2581/)

也有一些提案 [https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p0201r5.html](https://link.zhihu.com/?target=https%3A//www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p0201r5.html)

> std::optional<T> 是一种值语义的指针

我面试的时候经常会说上面这个，感觉挺有效的（比较能彰显对C++的理解说是

* * *

### 编译器会假设你的代码没有问题

除了编译器能知道更多的信息外，编译器还会假设你的代码没有问题，从而进行优化。

比如这种

[C++神秘学习——memcpy的一个ub](https://zhuanlan.zhihu.com/p/699259091)

编译器加上src一定是not null，所以直接返回没有判断了。

![](https://pic2.zhimg.com/v2-36fb9d8fde8b4d3104b519e68881901b_r.jpg)

还有一个严格别名

> 编译器会假设void bar(int \*p1, float \*p2)_p1，p2不会指向同一内存区域_

[严格别名（Strict Aliasing）规则是什么，编译器为什么不做我想做的事？](https://zhuanlan.zhihu.com/p/595286568)

* * *

剩下的是我认为也比较重要，但是没有先后顺序，彼此比较独立的内容。

### CRTP

一个很好像经常能在C++八股里面见到的东西。说什么这是静态多态的。

不过我还是比较推崇下面这种

**把继承看成函数注入**

**把CRTP看成带类型的函数注入**

[C++学习——CRTP与函数注入](https://zhuanlan.zhihu.com/p/695962055)

### 各种奇怪的C++知识

比如std::list::size的复杂度过去是O(n).

![](https://picx.zhimg.com/v2-2486db40c6b6efd3e527d944e0ade7b5_r.jpg)

这种东西太多了，不过好在有好兄弟给专门整理了一下

[https://makeinu-daily.pages.dev/](https://link.zhihu.com/?target=https%3A//makeinu-daily.pages.dev/)

### 错误处理

下面这个文章讲的挺好的了

[Cpp错误处理杂谈](https://zhuanlan.zhihu.com/p/643809540)

### 语言律师

[cppreference.cn - C++参考手册](https://link.zhihu.com/?target=https%3A//cppreference.cn/w/)已经不够用了，语言律师都是直接发ISO的（bushi

[https://cppquiz.org/quiz/question/231](https://link.zhihu.com/?target=https%3A//cppquiz.org/quiz/question/231) 可以闲的没事做做这个玩。

![](https://picx.zhimg.com/v2-93670a4c5ca6ddcc92f98c1fe5496317_r.jpg)

偶尔做做就行。

### 最佳实践

比起做语言律师，掌握最佳实践可能更重要（只要遵循最佳实践，大部分奇奇怪怪的问题都不会遇到的。

比如很经典的The rule of three/five/zero

[三/五/零法则 - cppreference.cn - C++参考手册](https://link.zhihu.com/?target=https%3A//cppreference.cn/w/cpp/language/rule_of_three)

![](https://pic1.zhimg.com/v2-d7a5fabe81bb097819937b07e74b9c34_r.jpg)

这些东西可以再网上找找

Effective Modern C++这种

或者直接搜索最佳实践相关的

[C++ Best Practices (C++最佳实践)翻译与阅读笔记](https://zhuanlan.zhihu.com/p/427778091)

### 模板元编程

不需要真的搞一个什么，编译期模板堆排序。

能做到简单的类型萃取就可以了，比如传入一个函数类型，获得返回值和参数。

```cpp
template<typename F>
struct function_args;

template<typename R, typename ...Args>
struct function_args<R(Args...)> {};
```

这种可以专门找一些模板元编程的文章看看，或者看stl源代码来学习。

比如下面这个（不过这个我觉得属于小技巧不算模板元编程，不过就放在一起吧

[C++学习——从unique\_ptr的deleter到\[\[no\_unique\_address\]\]](https://zhuanlan.zhihu.com/p/717500446)

### mq白

[mq白的b站空间](https://link.zhihu.com/?target=https%3A//space.bilibili.com/1292761396)

虽然好像mq白风评不是很好，知乎上也有“如何评价”这种。但是我确实看过mq白的视频学过C++，讲得还可以，至少讲错了你真的可以骂他。

> 而且就风评而言，各位acmer和别人相比也不遑多让吧

而且mq白本身也贡献了不少优质的**中文**C++资源。

比如这个 [https://github.com/Mq-b/Loser-HomeWork](https://link.zhihu.com/?target=https%3A//github.com/Mq-b/Loser-HomeWork)

> rocksdb的defer 里面是放了一个 std::function （[https://github.com/facebook/rocksdb/blob/main/util/defer.h](https://link.zhihu.com/?target=https%3A//github.com/facebook/rocksdb/blob/main/util/defer.h)），如果你看了上面的练习题后就知道如何优化了。

### **实现几个工具类**

例如tuple，function，any，variant这种。其实我觉得知道背后实现的原理就很好了，没必要真的手搓一个

可以看我之前写过的[C++模板学习实践](https://www.zhihu.com/column/c_1604135403384201217)

![](https://pic3.zhimg.com/v2-f4d4b247d137238dd76097d27fae5874_r.jpg)

不过其实写的比较烂，很多地方实现的不是很好，还有错误。。。

如果想最求比较高的质量可以去看msvc的stl

[https://github.com/microsoft/STL](https://link.zhihu.com/?target=https%3A//github.com/microsoft/STL)

### C++新特性

关注为什么要有这个？别的语言是怎么做的？之前是怎么做的？现在这么做有什么好处？

例如 [C++23特性总结 - 上](https://zhuanlan.zhihu.com/p/562383157)中提到的多元`operator[]`，我们可以直接问ai

![](https://pic2.zhimg.com/v2-397f008bf462cc2956b7bc0340c60721_r.jpg)

一个东西能进到C++的标准里面，说明他肯定是解决了什么问题的。

### 看一些成熟的代码

有很多优质的开源代码，看一些实际生产中的代码也是很好的学习。

就数据库学习而言，我比较推荐duckdb

[https://github.com/duckdb/duckdb](https://link.zhihu.com/?target=https%3A//github.com/duckdb/duckdb) （duckdb用C++11标准，考虑到很多人未来工作的时候可能标准被锁死在11上了，duckdb还是很有意义（用C++11也能写出好看的代码。

当然学习并使用一些库也是不错的，比如simdjson这种[https://github.com/simdjson/simdjson](https://link.zhihu.com/?target=https%3A//github.com/simdjson/simdjson)

### 加一些C++的学习群

这里不做引流，但是有一个比较个人的标准。

要找那种，经常说话的人里面，一半以上都是二次元头像的群。

![](https://pic2.zhimg.com/v2-f5d7ba06509cd7417e67c6459d4ad245_r.jpg)

## 结尾与补充

我上面写的内容，有相当一部分来自我当初如何学习C++的。不过C++内容太多了，所以很多只是提了一点，更多的是“如何学习”的方法论吧。

* * *

将来一些补充的东西会记在下面。