用于需要将手头的几本旧书（扫描件）转换成pdf，正好趁这次机会，将基于python的pdf操作总结一下。

## 需要安装的库

安装常规的用于操作pdf的python库：
```bash
pip install PyMuPDF
pip install PyPDF2
```


---


## 读取pdf文本内容

```python
import PyPDF2

reader = PyPDF2.PdfReader('file.pdf')

# 选择第0页
page_0 = reader.pages[0]

text = page_0.extract_text()
print(text)
```

---

## pdf转化成图像

```python
import os
# pip install PyMuPDF
import fitz
from tqdm import tqdm

def pdf2images(pdfPath: str, save_image_dir: str):
    if not os.path.exists(save_image_dir):
        os.makedirs(save_image_dir)

    pdfDoc: fitz.Document = fitz.open(pdfPath)
    for page_index in tqdm(range(len(pdfDoc))):
        page = pdfDoc[page_index]
        mat = fitz.Matrix(1.3, 1.3).prerotate(0)
        pix: fitz.Pixmap = page.get_pixmap(matrix=mat, alpha=False)

        save_name = f'images_{page_index}.png'
        save_path = os.path.join(save_image_dir, save_name)
        pix.save(save_path)
```

测试，将`test.pdf`每一页转化为图像，并存在`./images`文件夹下：
```python
pdf2images('test.pdf', './images')
```

---

## 合并两个pdf

```python
import PyPDF2

# 打开要合并的两个PDF文件
pdf1 = open('file1.pdf', 'rb')
pdf2 = open('file2.pdf', 'rb')

# 创建一个PdfFileMerger对象
merger = PyPDF2.PdfMerger()

# 将两个PDF文件添加到合并器中
merger.append(pdf1)
merger.append(pdf2)

# 合并两个PDF文件
merged_pdf = open('merged_file.pdf', 'wb')
merger.write(merged_pdf)

# 关闭所有打开的文件
pdf1.close()
pdf2.close()
merged_pdf.close()
```

---

## 读取 pdf 每页的图像

通过 `page.images`获取这个page的所有图像，通过`image.data`得到图像的二进制数据，然后直接写入本地即可。

```python
def load_pair_from_pdf(pdf_path: str) -> Generator[Tuple[Image.Image], str, str]:
    temp_image_path = 'temp.png'
    pdf = PyPDF2.PdfReader(pdf_path)
    for page in pdf.pages:
        img = page.images[0]
        
        with open(temp_image_path, 'wb') as fp:
            fp.write(img.data)
        image = Image.open(temp_image_path)
        yield image

    os.remove(temp_image_path)
```