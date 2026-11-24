## 常用图像分割指标
常用图像分割指标有如下几个：

- Dice
- IoU
- Sensitivity
- PPV
- HD95


这些指标除了HD95，都是越高越好，HD95是越低越好，因为它衡量了两个图像的拓扑距离。

## 代码
为了方便后期使用，我将代码整理如下。

先安装基本库：

```bash
pip install numpy hausdorff opencv-python
```

基本代码如下：

```python
import hausdorff
def transform_image_data(predict: np.ndarray, label: np.ndarray):
    predict = predict.astype(np.bool_).astype(np.int_)
    label = label.astype(np.bool_).astype(np.int_)
    return predict, label

def dice_coef(predict: np.ndarray, label: np.ndarray, epsilon: float = 1e-5) -> float:
    predict, label = transform_image_data(predict, label)
    intersection = (predict * label).sum()
    return (2. * intersection + epsilon) / (predict.sum() + label.sum() + epsilon)

def iou_score(predict: np.ndarray, label: np.ndarray, epsilon: float = 1e-5) -> float:
    predict, label = transform_image_data(predict, label)
    intersection = (predict & label).sum()
    union = (predict | label).sum()
    return (intersection + epsilon) / (union + epsilon)

def sensitivity(predict: np.ndarray, label: np.ndarray, epsilon: float = 1e-5) -> float:
    predict, label = transform_image_data(predict, label)
    intersection = (predict * label).sum()
    return (intersection + epsilon) / (label.sum() + epsilon)

def ppv(predict: np.ndarray, label: np.ndarray, epsilon: float = 1e-5) -> float:
    predict, label = transform_image_data(predict, label)
    intersection = (predict * label).sum()
    return (intersection + epsilon) / (predict.sum() + epsilon)

def hd95(predict: np.ndarray, label: np.ndarray, distance="euclidean"):
    predict, label = transform_image_data(predict, label)
    predict = predict.flatten()[..., None]
    label = label.flatten()[..., None]
    distance = hausdorff.hausdorff_distance(predict, label, distance=distance)
    return distance * 0.95
```

使用方法：
```python
gt_image = cv2.imread('./gt.png', 0)
kmeans_image = cv2.imread('./kmeans.mask.png', 0)

dice_coef(kmeans_image, gt_image)
```
输出：
```
0.9
```

因为它们的函数接口几乎一样，所以不妨写一个字典，以便后续批量调用：

```python
metrics = {
    'Dice': dice_coef,
    'IoU': iou_score,
    'Sensitivity': sensitivity,
    'PPV': ppv,
    'HD95': hd95
}

for metric_name in metrics:
    score = round(metrics[metric_name](predict, label), 3)
    print('{}:{}'.format(metric_name, score))
```