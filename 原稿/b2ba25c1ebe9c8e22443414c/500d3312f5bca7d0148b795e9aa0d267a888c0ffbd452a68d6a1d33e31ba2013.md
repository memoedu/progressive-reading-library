## outline

### chapter1 前置知识 (optional)

1. 矩阵微分
2. 反向传播、梯度下降、理论+case
3. python, numpy, matplotlib
4. 深度学习训练Pipe四板斧（dataloader，model，optimizer，loss）
5. 数据集的重要性
6. 深度学习框架实现原理简介*
7. 显卡核心与cuda驱动在深度学习中的作用。

### chapter2 深度学习框架与平台简介(optional)

1. 深度学习框架
    1. pytorch [3]
    2. tensorflow [4]
    3. paddlepaddle [5]
2. 训练平台
    1. AI Studio (使用免费V100)
    2. Colab (使用免费P100)
3. 开源平台
    1. Github (case: 快速克隆实现一个人脸检测)
    2. Huggingface (case: 基于开源权重快速实现一个情感检测网络)
    3. ModelScope (根据需求寻找多模态模型)

### chapter3 线性回归与全连接层神经网络

> 类似于花书，每个章节，四种语言实现一个case

1. 线性回归
    1. 认识线性回归
    2. 线性回归——boston房价预测
2. 全连接层神经网络
    1. 重新实现boston房价预测
    2. 更好的优化器
    3. 过拟合与欠拟合
    4. 模型保存与数据持久化

### chapter4 卷积神经网络

1. 图像数据与卷积
2. 为什么要用卷积？
    1. 全连接到卷积（CNN可以看成连接稀疏化的全连接层）
    2. 传统滤波器CV算法到卷积（CNN可以看成自动特征工程）
3. 实验：手写数字识别
    1. 使用CNN实现手写数字识别
    2. 使用AlexNet实现手写数字识别
        1. 为什么要加入池化（因为CNN下采样效率低）
        2. 激活函数（为什么选择Relu，Sigmoid有什么缺点，梯度爆炸和梯度弥散）
4. 批量规范化（BN）
5. 经典的网络架构（Resnet，Densenet，选择一种完成动物识别）
6. 实验：卷积自编码器去噪

### chapter5 循环神经网络

1. 序列数据与序列模型
2. 词嵌入word2vec
3. 从RNN到LSTM
4. 实验：新闻类别预测
5. 编码器-解码器模型
    1. 文本生成 seq2seq
    2. 权重衰减与注意力机制
    3. 实验：基于IMDB数据集的聊天机器人

### chapter6 Transformers

> 本章重点在于big picture和huggingface

1. 介绍
    1. 循环神经网络的缺点
    2. 多头注意力
    3. 位置编码
2. bert与gpt系列 (play with huggingface)
3. Vit (play with huggingface)
4. CLIP (play with huggingface)
5. chatgpt & new bing
    1. 数据量的重要性
    2. new bing 分布式爬虫+LLM

### chapter7 强化学习
1. 基本介绍
2. Q-learning & Sarsa(实验：寻路小乌龟)
3. DQN(实验：自动杆)
4. DDPG(实验：无人机智能悬停)

### chapter8 计算机视觉

目标识别，实例分割......

### chapter9 实用工具

1. 可视化工具：torchvision, tensorboard, visdom
2. 数据装载器

### chapter10 DIY实现深度学习框架 (附录)

1. 计算图的原理，节点类的实现
2. 前向运算和反向传播的实现
3. 算子函数与算子梯度函数

---

## 参考文献

1. https://zh-v2.d2l.ai
2. https://aistudio.baidu.com/aistudio/projectdetail/235437
3. https://pytorch.org/
4. https://www.tensorflow.org/?hl=zh-cn
5. https://www.paddlepaddle.org.cn/