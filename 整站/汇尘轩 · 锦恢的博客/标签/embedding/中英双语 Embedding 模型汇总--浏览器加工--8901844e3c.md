## 前言

在 LLM 时代， RAG 的策略使得向量数据库变得非常重要，加持了 vecdb 的 LLM 回答流如下图所示

<div align=center>
<img src="https://pic2.zhimg.com/v2-467a563e9693a1f69feae6b0a67d9ea9_r.jpg" style="width: 60%;"/>
</div>

其中 Embedding Model 也非常重要，这篇文章就简单讲解一下常用的开源可用的 Embedding Model 都有哪些。

## Summary

下面的模型都是支持中文英文双语言的嵌入。

- [text2vec-base-chinese](https://huggingface.co/shibing624/text2vec-base-chinese) 2022 年提出的通用文本 embedding
- [BCEmbedding](https://huggingface.co/maidalun1020/bce-embedding-base_v1) `BCEmbedding` 是由网易有道开发的中英双语和跨语种语义表征算法模型库，其中包含 `EmbeddingModel` 和 `RerankerModel` 两类基础模型。

如果你是第一次使用 hf 的模型，使用如下的命令登录：

```bash
pip install transformers
huggingface-cli login
```

根据指示一步步走。

---

## text2vec-base-chinese

安装

```bash
pip install -U text2vec
```

使用

```python
from text2vec import SentenceModel
sentences = ['python 是什么', '请介绍一下 python']
model = SentenceModel('shibing624/text2vec-base-chinese')
embeddings = model.encode(sentences)
print(embeddings.shape)
```

输出

```
(2, 768)
```

在128核CPU上跑embedding，速度非常快，几乎都是一瞬间完成嵌入。




## BCEmbedding

进入 [https://huggingface.co/maidalun1020/bce-embedding-base_v1](https://huggingface.co/maidalun1020/bce-embedding-base_v1) 勾选同意该模型的协议。

安装

```bash
pip install BCEmbedding
```

使用

```python
from BCEmbedding import EmbeddingModel
sentences = ['python 是什么', '请介绍一下 python']
model = EmbeddingModel(model_name_or_path="maidalun1020/bce-embedding-base_v1")
embeddings = model.encode(sentences)
print(embeddings.shape)
```

输出

```
(2, 768)
```

在128核CPU上跑embedding，速度非常快，几乎感觉不到延迟。