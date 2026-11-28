时域卷积网络（Temporal Convolutional Network，TCN）由Shaojie Bai et al.在2018年提出，顾名思义，可知是使用卷积来进行时序信息操作的一项工作。

简单说，TCN就是在算子结构上对CNN进行了改进，使得其在时序问题上也能适用。TCN的卷积操作如下所示：

<div align=center>
<img src="http://nooverfit.com/wp/wp-content/uploads/2018/06/Screenshot-from-2018-06-09-155731.png" style="width: 80%;"/>
<p class="img-caption">TCN 卷积中数据结构变化</p>
</div>

可以看到，TCN的卷积有如下的两个特点：

- 因果卷积：第T步的值只能由前T-1步的值运算得来。
- 空洞卷积：卷积窗口随着网络的层数逐步变大。

因果卷积的引入很好理解，因为要处理时序信息嘛，那么只用前T-1的信息预测第T步的信息合情合理。那么空洞卷积（**dilated convolutions**，也被称为膨胀卷积）作用是什么呢？从现在的角度来看，空洞卷积无非就是为了增大卷积的感受野，使得全局信息可以更好得在高层结构中被学到。这正好也是RNN这种串行结构的缺点。

总而言之，TCN卷积用公式表示就是：
$$
F(s)=(x*_df)(s)=\sum_{i=0}^{k-1}f(i)\cdot x_{s-d\cdot i}
$$


当然，除了基本的结构上的变化，原论文中还加入了残差连接的结构。

<div align=center>
<img src="http://nooverfit.com/wp/wp-content/uploads/2018/06/Screenshot-from-2018-06-09-162900.png" style="width: 90%;"/>
<p class="img-caption">残差连接</p>
</div>

---

# 参考

- [时间卷积网络(TCN) 总结：时序模型不再是递归网络(RNN) 的天下，但作为信息粗暴提取的一种方法，请不要神话CNN ! | David 9的博客 --- 不怕"过拟合" (nooverfit.com)](http://nooverfit.com/wp/时间卷积网络tcn-总结：时序模型不再是递归网络rnn/)