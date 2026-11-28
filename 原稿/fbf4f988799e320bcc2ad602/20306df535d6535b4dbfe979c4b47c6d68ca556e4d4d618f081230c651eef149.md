## 代码

最近写论文需要可视化一下 Dirichlet分布。为了美观，我们一般只可视化三维的情况，因为三维的情况下，各系数为标准二阶单纯形的开集，也就是一个平面上的三角形，更美观。代码如下：

> 摘自 https://blog.bogatron.net/blog/2014/02/02/visualizing-dirichlet-distributions/

```python
from math import gamma
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.tri as tri

class Dirichlet(object):
    def __init__(self, alpha):
        
        self._alpha = np.array(alpha)
        self._coef = gamma(np.sum(self._alpha)) / \
                           np.multiply.reduce([gamma(a) for a in self._alpha])
    def pdf(self, x):
        '''Returns pdf value for `x`.'''
        from operator import mul
        return self._coef * np.multiply.reduce([xx ** (aa - 1) for (xx, aa)in zip(x, self._alpha)])

corners = np.array([[0, 0], [1, 0], [0.5, 0.75**0.5]])
AREA = 0.5 * 1 * 0.75**0.5
triangle = tri.Triangulation(corners[:, 0], corners[:, 1])

# For each corner of the triangle, the pair of other corners
pairs = [corners[np.roll(range(3), -i)[1:]] for i in range(3)]
# The area of the triangle formed by point xy and another pair or points
tri_area = lambda xy, pair: 0.5 * np.linalg.norm(np.cross(*(pair - xy)))

def xy2bc(xy, tol=1.e-4):
    '''Converts 2D Cartesian coordinates to barycentric.'''
    coords = np.array([tri_area(xy, p) for p in pairs]) / AREA
    return np.clip(coords, tol, 1.0 - tol)

def draw_pdf_contours(dist, nlevels=200, subdiv=8, **kwargs):
    refiner = tri.UniformTriRefiner(triangle)
    trimesh = refiner.refine_triangulation(subdiv=subdiv)
    pvals = [dist.pdf(xy2bc(xy)) for xy in zip(trimesh.x, trimesh.y)]
    plt.tricontourf(trimesh, pvals, nlevels, cmap='jet', **kwargs)
    plt.axis('equal')
    plt.xlim(0, 1)
    plt.ylim(0, 0.75**0.5)
    plt.axis('off')
```

## 使用方法

使用方法如下：

```python
# 绘制 α = (0.99, 0.99, 0.99) 的三维 Dirichlet 分布
draw_pdf_contours(Dirichlet([0.99, 0.99, 0.99]))
plt.show()
```

输出效果

<div align=center>
<img src="https://pic1.zhimg.com/80/v2-970510bd72dfeb8ce85f13fe5e5d1858_1440w.png" style="width: 40%;"/>
</div>


