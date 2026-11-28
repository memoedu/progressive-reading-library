# How to debug a learning algorithm?

如果你训练完了一个神经网络但是发现效果并不理想，那应该怎么改进我们的神经网络呢？

我们有如下的手段：

- Get more training examples.
- Try smaller sets of features.
- Try getting additional features.
- Try adding polynomial features(x1^2,x1x2,etc.).
- Try decreasing lambda.
- Try increasing lambda.

当然，一个一个地去尝试这些方法会耗费大量的时间，那是我们不愿意的。我们有一些方法能使我们诊断问题事半功倍。

## 测试集

将数据分为训练集（80%）和测试集（20%）

## 验证集

在一个问题不确定那个选择什么模型的情况下，可以将数据分为训练集（60%），验证集（20%）和训练集（20%）。将所有待选择的模型用训练集跑出来，再在验证集上测试，选取表现最好的那个模型放入测试集上测试。

验证集的选取也可以使用**交叉验证集（Cross validation）**。

通过图像要学会判断当前的异常状况是属于高偏差（欠拟合）还是高方差（过拟合）

## 正则化系数选择

之前已经讨论过了，正则化系数λ不能太大也不能太小，那么应该取多少呢？我们可以以两倍的步长来列举λ：

>1. Try λ = 0
>2. Try λ = 0.01
>3. Try λ = 0.02
>4. Try λ = 0.04
>5. Try λ = 0.08
>
>......
>
>12. Try λ = 10(10.24)

每一个λ产生的模型进行交叉验证，选取误差最小的那个模型对应的λ