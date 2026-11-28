刚好最近的「数据库原理」要进入期末了，正好把比较麻烦的东西整理一下。

先简单复习一下，在关系代数中，能够让运算封闭的基本运算如下表所示：

|   符号   |   含义   |
| :------: | :------: |
|  $\cup$  |    并    |
|   $-$    |    差    |
|  $\cap$  |    交    |
| $\times$ | 笛卡尔积 |

后续常用的几个基本操作：$\sigma,\Pi,\bowtie,\div$都是基于上述基本操作定义出的。

---

## 关系数据库除操作

### 数学定义

简单复习一下**除操作(Division)**的定义。给定两个关系$R(X,Y)$和$S(Y,Z)$，则$R\div S$定义为：
$$
R\div S=\{ t_r[X]|t_r\in R\land \Pi_Y(S)\subseteq Y_{t_r[X]} \}
$$
其中$Y_{t_r[X]}$代表$R$中所有属性$X$值为$t_r[X]$的元组的属性$Y$组成的集合。

用自然语言理解上述的操作过程可以是：

1. 初始化返回集合$M$为空
2. 遍历关系$R$中属性$X$的域中的每一个值$x$
3. 记$R$中所有$X=x$的元组的集合为$T$
4. 如果是$\Pi_Y(S)\subseteq \Pi_Y(T)$，则将$\Pi_X(T)$加入$M$
5. 循环结束后返回$M$

### 简单的例子

来简单看个例子，我们令$R$为：

| A    | B    | C    |
| ---- | ---- | ---- |
| a1   | b1   | c2   |
| a2   | b3   | c7   |
| a3   | b4   | c6   |
| a1   | b2   | c3   |
| a4   | b6   | c6   |
| a2   | b2   | c3   |
| a1   | b2   | c1   |

令$S$为：

| B    | C    | D    |
| ---- | ---- | ---- |
| b1   | c2   | d1   |
| b2   | c1   | d1   |
| b2   | c3   | d2   |

如何计算$R\div S$呢？首先找出$X,Y,Z$对应的属性组是什么，很显然，$X=[A]$, $Y=[B,C]$, $Z=[D]$。$R$中属性$X$的域为a1,a2,a3,a4，下面开始遍历。

首先是a1，选出对应的元组为(a1, b1, c2), (a1, b2, c3), (a1, b2, c1)，选取对应的$Y$分量组成的集合：(b1, c2), (b2, c3), (b2, c1)，发现$\Pi_Y(S)$是它的子集，所以a1满足要求。

然后是a2，选出对应的元组为(a2, b3, c7), (a2, b2, c3)，选取对应的$Y$分量组成的集合：(b3, c7), (b2, c3)，发现$\Pi_Y(S)$不是它的子集，所以a2不满足要求。

同理可得a3和a4都不满足要求，所以$R\div S$的结果为：

| A    |
| ---- |
| a1   |

### 实际应用

除法在含有“至少选了”，“选择了全部”这类场景中用得很多。比如我们要查“选修了所有选修课的同学的学号”。其中代表学生选课情况的SC表为SC(Sno, Cno, Grade), 课程C表为C(Cno, Cname)，其中各个项的含义如下：

- Sno：学生学号
- Cno：课程编号
- Grade：成绩
- Cname：课程名称

那么“选修了所有选修课的同学的学号”这个查询对应的关系代数为：
$$
\Pi_{\text{Sno,Cno}}(\text{SC})\div \Pi_{\text{Cno}}(\text{C})
$$
这句话得到的结果就是查询结果。

---

## Oracle的SQL语句实现除操作

首先强调，Oracle原生提供的SQL语句**没有除操作**。我们需要自己实现，好在Oralce提供了量词exists来表达复杂语义。

直接给出$\Pi_{\text{Sno,Cno}}(\text{SC})\div \Pi_{\text{Cno}}(\text{C})$对应的SQL语句：

```sql
SELECT DISTINCT SCX.SNO FROM SC SCX
WHERE NOT EXISTS (
    SELECT * FROM C
    WHERE NOT EXISTS (
        SELECT * FROM SC SCY
        WHERE SCY.CNO = C.CNO AND SCY.SNO = SCX.SNO
    )
);
```

上面的双重not exists想表达的意思就是“对于满足要求的学号A，不存在一个课程，它存在于课表C中，但是A没有选A”，理解起来可能会比较绕，但是写多了还是很好理解的。上述代码就是一个很好的模板，而且使用这种写法很灵活，比如我现在的要求变成“选修了所有选修课的同学的学号和姓名”，其中姓名存在表S(Sno, Sname, Ssex)中，用关系代数还需要加一个自然连接，但是使用量词的SQL只需要改一个地方：

```sql
SELECT SNAME FROM S
WHERE NOT EXISTS (
    SELECT * FROM C
    WHERE NOT EXISTS (
        SELECT * FROM SC
        WHERE SC.CNO = C.CNO AND SC.SNO = S.SNO
    )
);
```



---

# 参考

[1] [关系运算中的除操作怎么用SQL语句表示？ 有套路的！（双 NOT EXISTS）_一个不想写代码的程序员的博客-CSDN博客](https://blog.csdn.net/weixin_36979214/article/details/105993194?spm=1001.2014.3001.5506)