简单记录一下如何使用 python 或者 JavaScript 读取 CT 文件 NIFTI。

NIFTI，简称 nii 文件，是用于存储 CT 三维图像的一种数据存储格式，它一共存在两种版本：

- 未压缩版本：NIFTI-1，后缀为 `.nii`
- 压缩版本：NIFTI-2，后缀为 `.nii.gz`

如果需要使用深度学习模型对 CT 图像进行处理，就需要获取 python 进行 nii IO 的 API；如果需要构建数据平台，在网页对于 CT 图像进行可视化，则需要 JavaScript 的 nii IO API。

---

## Python

```bash
pip install nibabel
```

```python
import nibabel as nib
image_object = nib.loadsave.load('./Train1/Data/2.nii')
images = image_object.get_fdata()
images = np.array(images)
# (256, 256, 4)
```
这代表该 CT 只有 四层，每层的分辨率为 (256, 256) 。通过 `images[..., i]` 就能获取第 `i` 层的图像。需要注意的是，CT 图像的像素点值是基于传感器参数变换（Radon 变换）得到的，并不是 0 到 1 或者 0 到 255 的数字，所以需要归一化 。我们可以简单分离一下，并看一下我们感兴趣的图像属性：

```python
images = np.array(images)

image_number = images.shape[-1]
print(image_number)

plt.figure(dpi=120, figsize=[20, max(20 // image_number, 1)])
for image_id in range(image_number):
    image = images[..., image_id]
    print(image.shape)
    print(image.min(), image.max())
    normalise_image = (image - image.min()) / (image.max() - image.min())
    
    plt.subplot(1, image_number, image_id + 1)
    plt.title('layer ID: ' + str(image_id))
    plt.imshow(normalise_image, cmap='gray')

plt.tight_layout()
plt.show()
```
输出：
```
4
(256, 256)
-23.0 2248.0
(256, 256)
-23.0 2246.0
(256, 256)
-23.0 2265.0
(256, 256)
-23.0 2271.0
```
从中你能看到 CT 中像素值的分布。下面是可视化结果：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-ed6cf3af5bd108e1a202489c3b5c20e3_1440w.png" style="width: 80%;"/>
</div>

如果是 `.nii.gz` 文件，使用 标准库 `gzip` 解压后再使用 `nibabel` 进行 IO 即可。

---


## JavaScript

根据我的搜索，我找到了一个不错的项目来在前端实现 nii 的可视化：[NIFTI-Reader-JS](https://github.com/rii-mango/NIFTI-Reader-JS)




