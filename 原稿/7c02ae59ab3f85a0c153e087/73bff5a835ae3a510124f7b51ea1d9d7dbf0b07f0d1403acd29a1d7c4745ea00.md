## 前言

OOD 检测算法汇总，是一个对我开发的 `torchood` 库的简易说明，具体使用方法请参考：[torchood 使用文档](https://document.kirigaya.cn/blogs/torchood/main.html).

---

## Prototype Network

工作来源：[CVPR 2018: Rbust classification with convolutional prototype learning](https://arxiv.org/pdf/1805.03438)

计算出每一个类别的原型向量（`centers`），这些原型向量代表了该类别的中心点。训练过程中，该方法会不断去更新 centers，最终收敛。

推理阶段，计算出输入样本距离每一个 centers 的距离，选择最近的 center 对应的类别作为分类结果；如果最近的距离都非常远，那么判为 OOD。

Prototype Learning 的本质思想为简化 KNN，不需要存储每一个样本点，而是存储代表每一个类别的中心点。

```python
from torchood import SimpleCNN, PrototypeNetwork

base_model = SimpleCNN(in_dim=3, out_dim=num_class).to('cuda')

model = PrototypeNetwork(
    base_model,
    model_output_dim=1000,
    num_hidden_units=2,
    num_classes=num_class,
    scale=2
).to('cuda')
```



其中 `base_model` 的类的 forward 函数必须满足如下的函数签名：

```python
class SimpleCNN(nn.Module):
    
    def forward(self, x: torch.Tensor) -> tuple[torch.Tensor, torch.Tensor, torch.Tensor]:
		// process
		// ...
        return feature, logits, prob
```

其中

- `feature`：backbone 处理后的特征图
- `logits`：分类层的输出结果
- `probs`：分类层的输出结果 softmax 后的结果



### 训练

```python
for data, label in train_loader:
	features, centers, distance, outputs = model(data)
	# label.shape = [batch_size]
    loss = model.criterion(features, centers, outputs, label)
```



### 推理

```python
for data, label in test_loader:
    features, centers, distance, outputs = model(data)
    pred = distance.argmax(dim=1)
```



### OOD

```python
# 设置最小距离阈值
threshold = ...

for data, label in test_loader:
    features, centers, distance, outputs = model(data)
    pred = distance.softmax(dim=1)
    min_distance = torch.min(- distance, dim=1).values
    is_ood = min_distance > threshold
```

---

## Prior Network

工作来源：[NeurIPS 2018: Predictive Uncertainty Estimation via Prior Networks](https://papers.nips.cc/paper_files/paper/2018/hash/3ea2db50e62ceefceaf70a9d9a56a6f4-Abstract.html)

训练阶段就输入 OOD 和 ID，在训练阶段就学习到 OOD 的先验分布，利用 OOD 的先验分布在推理阶段去判断一个样本是否为 OOD。

在 [PriorNetworks](https://github.com/gtegner/PriorNetworks) 这个项目中，训练阶段的 OOD 样本通过构造层次化的加入高斯噪音的训练样本组成。通过下面的代码可以构造一个和训练数据同形的 tensor

```python
def generate_ood(x_train: torch.Tensor, dist: float) -> torch.Tensor:
    r"""Generate OOD samples from training set
    
    Args:
        N (int): sample number of generated ood data
        x_train (torch.Tensor): training dataset
        dist (float): disturbing scale to add noise
    
    """
    x_range = (x_train[:, 0].min() - dist, x_train[:, 0].max() + dist)
    v = np.random.uniform(x_range[0], x_range[1], size=x_train.shape)
    return torch.FloatTensor(v)
```

### 训练

### 推理

### OOD

---

## Post Network

工作来源：[NeurIPS 2020: Posterior Network: Uncertainty Estimation without OOD Samples via Density-Based Pseudo-Counts](https://proceedings.neurips.cc/paper/2020/hash/0eac690d7059a8de4b48e90f14510391-Abstract.html)

Post Network 通过对于分类器输出值建模后验分布，来实现 OOD 检测。具体做法为，使用 Dirichlet 这样的二阶分布函数来建模分类器输出的 logits，通过 UCE loss 来进行训练：
$$
\text{UCE} = \sum_{i=1}^K y_i(\psi(\alpha_0)-\psi(\alpha_i)) - \lambda \cdot \text{Entropy}(\text{Dirichlet}(\alpha))
$$
其中：

- $y_i$ 是目标类别的 one-hot 编码。
- $\alpha_0=\sum_{i=1}^K \alpha_i$ 是 Dirichlet 分布的精度参数。
- $\psi$ 是 digamma 函数。
- $\lambda$ 是正则化系数。





### 训练

### 推理

### OOD

---

## HOD Loss

工作来源：[MIA 2022: Does Your Dermatology Classifier Know What It Doesn't Know? Detecting the Long-Tail of Unseen Conditions](https://arxiv.org/abs/2104.03829v1?_hsenc=p2ANqtz-8g4edUHChpOYtecmHQ5BQyfEE8FtzPSe_cJRHy1k8R7cmRVuF8pSRpajGRW-eMezmU2aM-ohNGWabNKag6LX4x0zXjEQ&_hsmi=167784826&utm_source=pocket_mylist)

这项工作提出了 HOD Loss 用于训练适用于 OOD 检测的分类器，但是这个方法需要事先知道 OOD 的类别数量。在 `torchood` 中，初始化 `HODDetector` 时，你需要告诉模型 ID 和 OOD 类别分别有多少。





### 训练

### 推理

### OOD

---

## ENN



### 训练

### 推理

### OOD

---

## ERNN



### 训练

### 推理

### OOD