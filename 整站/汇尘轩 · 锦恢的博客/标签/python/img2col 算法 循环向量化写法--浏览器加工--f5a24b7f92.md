mat2col 算法，版本 0.0.1


```python
def mat2col(mat: np.ndarray, ksize: int, stride:int=1) -> np.ndarray:
    """
    简单说明一下 向量化版本的 mat2col 的内部变量标记
        alpha_0 = (beta_0, beta_1, ..., beta_{out_h * out_w}) ^ T
        alpha = (alpha_0, alpha_1, ..., alpha_{k - 1})
    """
    assert len(mat.shape) == 2, 'mat2col only process 2d matrix!'
    h, w = mat.shape
    assert ksize <= min(h, w), 'ksize must be smaller than both height and width of input image!'
    
    out_h = (h - ksize) // stride + 1
    out_w = (w - ksize) // stride + 1
    
    beta_0 = np.arange(ksize)
    beta_i = np.arange(out_h * out_w)
    
    alpha_0_0 = (beta_i // out_w) * w + (beta_i % out_w) * stride
    alpha_0 = beta_0 + alpha_0_0[..., None].repeat(ksize, axis=1)
    
    alpha_i = np.arange(ksize) * w
    alpha_0 = np.tile(alpha_0, ksize)
    alpha_i = np.repeat(alpha_i, ksize)
    alpha = alpha_0 + alpha_i
    col = np.take(mat.flatten(), alpha)
    return col
```