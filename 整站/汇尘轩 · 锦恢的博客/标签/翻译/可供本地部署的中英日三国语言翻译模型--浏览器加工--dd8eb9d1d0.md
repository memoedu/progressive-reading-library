对于文档开发和本地化工作流而言，一个能够快速使用的本地推理的AI翻译模型是很有必要的，因为大工作流请求Chatgpt进行翻译受限于网速和API Token。而本地推理模型不仅成本为零，而且允许你对原文本进行多次修改，重新翻译。

这篇文章推荐的模型是`Huggingface`上的`K024/mt5-zh-ja-en-trimmed`。在亚洲，最常用的语言就是中文、英文和日文了，这个模型刚好可以一次解决这三个语言的相互翻译的问题。

## 代码

原本的代码不支持cuda加速，我改写了一版如下：

```python
from transformers import (
  T5Tokenizer,
  MT5ForConditionalGeneration,
)
path = "K024/mt5-zh-ja-en-trimmed"
model = MT5ForConditionalGeneration.from_pretrained(path).cuda(device=0)
tokenizer = T5Tokenizer.from_pretrained(path)

def translate_common(zh: str, mode: str) -> str:
    sentence = f"{mode}:{zh}"
    input_tensor = tokenizer(sentence, return_tensors='pt')
    input_ids = input_tensor['input_ids'].cuda(device=0)
    attention_mask = input_tensor['attention_mask'].cuda(device=0)
    outputs = model.generate(input_ids=input_ids, attention_mask=attention_mask, max_new_tokens=512)

    return tokenizer.decode(outputs[0], skip_special_tokens=True)

def zh2ja(zh: str) -> str:
    return translate_common(zh, 'zh2ja')

def zh2en(zh: str) -> str:
    return translate_common(zh, 'zh2en')

def en2ja(en: str) -> str:
    return translate_common(en, 'en2ja')

def en2zh(en: str) -> str:
    return translate_common(en, 'en2zh')

def ja2zh(ja: str) -> str:
    return translate_common(ja, 'ja2zh')

def ja2en(ja: str) -> str:
    return translate_common(ja, 'ja2en')
```

## 使用与测试

```python
test_zh = '你知道如何学习人工智能吗？'
test_en = 'Do you know how to learn artificial intelligence?'
test_ja = '人工知能の学び方を知っていますか？'

print('测试中文翻译成英文和日文：')
print('{} -> {}'.format(test_zh, zh2en(test_zh)))
print('{} -> {}'.format(test_zh, zh2ja(test_zh)))

print("\nTest Translation from English to Chinese and Japanese:")
print('{} -> {}'.format(test_en, en2zh(test_en)))
print('{} -> {}'.format(test_en, en2ja(test_en)))

print('\nテスト日本語から中国語、英語への翻訳：')
print('{} -> {}'.format(test_ja, ja2zh(test_ja)))
print('{} -> {}'.format(test_ja, ja2en(test_ja)))
```

输出：
```
测试中文翻译成英文和日文：
你知道如何学习人工智能吗？ -> Do you know how to learn AI?
你知道如何学习人工智能吗？ -> AIを学ぶ方法をご存知ですか?

Test Translation from English to Chinese and Japanese:
Do you know how to learn artificial intelligence? -> 你知道如何学习人工智能吗?
Do you know how to learn artificial intelligence? -> 人工知能を学ぶ方法を ご存知ですか?

テスト日本語から中国語、英語への翻訳：
人工知能の学び方を知っていますか？ -> 你知道人工智能的学习方式吗?
人工知能の学び方を知っていますか？ -> Do you know how to learn about artificial intelligence?
```