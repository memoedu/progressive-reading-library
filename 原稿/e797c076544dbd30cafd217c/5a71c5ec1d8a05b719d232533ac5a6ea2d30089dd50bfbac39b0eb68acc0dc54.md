## 前言

今天起床看到群友分享的一个链接，感觉是一个非常棒的工作。于是想着分享一下它。
![](https://picx.zhimg.com/80/v2-de148dbef301709d2d230b25a5874621_1440w.png)

OpenMED 本身有点小小的标题党，但是瑕不掩瑜，我先在正式分享之前简单省流一下：OpenMED 目前公开了一组 NER 模型，一共 384 个，大小从 0.1B 到 0.6B 不等，几乎涵盖了医学领域大部分名词和领域。基于这些 NER 模型组，有望从海量的医学数据中制作出高质量的医学数据，从而训练出更加专业的医学大模型。OpenMED 希望这项工作的外围由开源社区来完成，并也提供了对应的工具，NER 模型组可能只是第一个。

他们的 huggingface 主页：https://huggingface.co/OpenMed

下面是在 OpenMED 主页上的原文，我只是简单爬取内容并做了一下翻译。

## 认识OpenMed：380+免费医疗实体识别模型

医疗AI长期被昂贵的付费墙和封闭系统所阻碍。研究人员、医生和开发者不得不应对高昂费用和神秘的"黑箱"工具，严重拖慢进展速度。现在，[**OpenMed**](https://huggingface.co/OpenMed)以**超过380个顶尖医疗实体识别(NER)模型**彻底改变这一现状，所有模型均在**Apache 2.0许可证**下免费开放。

这些模型不仅媲美昂贵的商业方案，更超越它们，提供卓越性能并让全球医疗突破加速实现。

## 问题：被锁住的医疗AI

医疗AI面临几大障碍：
* **昂贵授权**：小型团队和大学难以负担
* **技术黑箱**：商业工具不公开工作原理
* **技术滞后**：许多付费模型跟不上最新技术
* **使用局限**：只有大公司能获得优质资源

这严重拖慢了研究进度、临床发展以及医疗公平。

## 解决方案：OpenMed NER模型

OpenMed带来**380+免费NER模型**，可识别各类医学术语——从药物名称到疾病等。其优势包括：
* ✅ **完全免费**：Apache 2.0开源许可
* ✅ **开箱即用**：专为实际场景设计
* ✅ **灵活尺寸**：1.09亿至5.68亿参数可选
* ✅ **严格测试**：基于13+标准数据集验证
* ✅ **兼容性强**：完美支持Hugging Face和PyTorch

这些模型打破了旧有壁垒，让医疗AI真正为所有人所用。

## OpenMed工具箱里有什么？

OpenMed的**380+模型**基于**13个关键医疗数据集**精细调校，取得顶尖成果——F1分数最高达**0.998**。提供多种尺寸选择，满足从轻量级到超强性能的不同需求。

### 🔬 全面覆盖领域

这些模型在多个领域表现卓越：
* **药物化学**：识别化合物助力药物研发或安全追踪
* **疾病临床**：提取病症信息优化诊断工具
* **基因分子**：深入基因组学与精准医疗
* **解剖术语**：提升医疗记录与编码效率
* **癌症研究**：推动肿瘤学研究

从学术论文到医院工作流都能完美适用。

### 🎯 OpenMed(开源) vs 商业闭源方案对比

数据集 | OpenMed最佳F1(%) | 闭源最佳F1(%)† | 差距(OpenMed-SOTA) | 当前闭源领先者  
---|---|---|---|---  
**BC4CHEMD** | **95.40** | 94.39 | **+1.01** | Spark NLP  
**BC5CDR-Chem** | **96.10** | 94.88 | **+1.22** | Spark NLP  
**BC5CDR-Disease** | **91.20** | 88.5 | **+2.70** | BioMegatron  
**NCBI-Disease** | **91.10** | 89.71 | **+1.39** | BioBERT  
**JNLPBA** | 81.90 | **82.00** | -0.10 | KeBioLM  
**Linnaeus** | **96.50** | 92.70 | **+3.80** | BERN2  
**Species-800** | **86.40** | 82.59 | **+3.81** | Spark NLP  
**BC2GM** | **90.10** | 88.75 | **+1.35** | Spark NLP  
**AnatEM** | 90.60 | **91.65** | -1.05 | Spark NLP  
**BioNLP2013CG** | **89.90** | 87.83 | **+2.07** | Spark NLP  
**Gellus** | **99.80** | 63.40 | **+36.40** | ConNER  
**CLL** | **95.70** | 85.98 | — | (无公开最佳)  
**FSU** | **96.10** | — | — | (无公开最佳)  

†闭源分数为文献中最高同行评审/排行榜结果(通常来自Spark NLP、NEEDLE、BERN2等商业模型)

[![开源与闭源性能对比图](https://huggingface.co/spaces/OpenMed/README/resolve/main/openmed_vs_sota_performance.png)](https://huggingface.co/spaces/OpenMed/README/resolve/main/openmed_vs_sota_performance.png)

### 🔬 按领域分类

领域 | 包含数据集 | 可用模型数 | 参数范围 | 推荐模型  
---|---|---|---|---  
**药理学** | `bc5cdr_chem`等 | 90个 | 1.09亿-5.68亿 | `OpenMed-NER-PharmaDetect-SuperClinical-434M`  
**疾病病理** | `bc5cdr_disease`等 | 60个 | 1.09亿-4.34亿 | `OpenMed-NER-PathologyDetect-PubMed-v2-109M`  
**基因组学** | `jnlpba`等 | 150个 | 3.35亿-5.68亿 | `OpenMed-NER-GenomicDetect-SnowMed-568M`  
**解剖学** | `anatomy` | 30个 | 5.60亿 | `OpenMed-NER-AnatomyDetect-ElectraMed-560M`  
**肿瘤学** | `bionlp2013_cg` | 30个 | 3.55亿 | `OpenMed-NER-OncologyDetect-SuperMedical-355M`  
**临床笔记** | `cll` | 30个 | 5.60亿 | `OpenMed-NER-BloodCancerDetect-ElectraMed-560M`  

### ⚡ 选择合适尺寸

规格 | 参数量 | 适用场景  
---|---|---  
**紧凑型** | 1.09亿 | 快速部署  
**大型** | 3.35-3.55亿 | 平衡精度  
**超大型** | 4.34亿 | 全能选手  
**特大型** | 5.60-5.68亿 | 极致性能  

[![模型尺寸对比图](https://huggingface.co/spaces/OpenMed/README/resolve/main/model-size-comparison.png)](https://huggingface.co/spaces/OpenMed/README/resolve/main/model-size-comparison.png)

### 📊 各数据集最佳模型

数据集 | 最佳模型 | F1分数 | 模型尺寸  
---|---|---|---  
`bc5cdr_chem` | [`OpenMed-NER-PharmaDetect-SuperClinical-434M`](https://huggingface.co/OpenMed/OpenMed-NER-PharmaDetect-SuperClinical-434M) | 0.961 | 4.34亿  
`bionlp2013_cg` | [`OpenMed-NER-OncologyDetect-SuperMedical-355M`](https://huggingface.co/OpenMed/OpenMed-NER-OncologyDetect-SuperMedical-355M) | 0.899 | 3.55亿  
`bc4chemd` | [`OpenMed-NER-ChemicalDetect-PubMed-335M`](https://huggingface.co/OpenMed/OpenMed-NER-ChemicalDetect-PubMed-335M) | 0.954 | 3.35亿  
`linnaeus` | [`OpenMed-NER-SpeciesDetect-PubMed-335M`](https://huggingface.co/OpenMed/OpenMed-NER-SpeciesDetect-PubMed-335M) | 0.965 | 3.35亿  
`jnlpba` | [`OpenMed-NER-DNADetect-SuperClinical-434M`](https://huggingface.co/OpenMed/OpenMed-NER-DNADetect-SuperClinical-434M) | 0.819 | 4.34亿  
`bc5cdr_disease` | [`OpenMed-NER-DiseaseDetect-SuperClinical-434M`](https://huggingface.co/OpenMed/OpenMed-NER-DiseaseDetect-SuperClinical-434M) | 0.912 | 4.34亿  
`fsu` | [`OpenMed-NER-ProteinDetect-SnowMed-568M`](https://huggingface.co/OpenMed/OpenMed-NER-ProteinDetect-SnowMed-568M) | 0.961 | 5.68亿  
`ncbi_disease` | [`OpenMed-NER-PathologyDetect-PubMed-v2-109M`](https://huggingface.co/OpenMed/OpenMed-NER-PathologyDetect-PubMed-v2-109M) | 0.911 | 1.09亿  
`bc2gm` | [`OpenMed-NER-GenomeDetect-SuperClinical-434M`](https://huggingface.co/OpenMed/OpenMed-NER-GenomeDetect-SuperClinical-434M) | 0.901 | 4.34亿  
`cll` | [`OpenMed-NER-BloodCancerDetect-ElectraMed-560M`](https://huggingface.co/OpenMed/OpenMed-NER-BloodCancerDetect-ElectraMed-560M) | 0.957 | 5.60亿  
`gellus` | [`OpenMed-NER-GenomicDetect-SnowMed-568M`](https://huggingface.co/OpenMed/OpenMed-NER-GenomicDetect-SnowMed-568M) | 0.998 | 5.68亿  
`anatomy` | [`OpenMed-NER-AnatomyDetect-ElectraMed-560M`](https://huggingface.co/OpenMed/OpenMed-NER-AnatomyDetect-ElectraMed-560M) | 0.906 | 5.60亿  
`species800` | [`OpenMed-NER-OrganismDetect-BioMed-335M`](https://huggingface.co/OpenMed/OpenMed-NER-OrganismDetect-BioMed-335M) | 0.864 | 3.35亿  

## 三行代码快速体验

使用Hugging Face Transformers轻松集成OpenMed NER模型：
```python
from transformers import pipeline

ner_pipeline = pipeline("token-classification", 
                       model="OpenMed/OpenMed-NER-PharmaDetect-SuperClinical-434M",
                       aggregation_strategy="simple")
text = "患者处方10mg阿司匹林治疗高血压"
entities = ner_pipeline(text)
print(entities)
```
输出将自动识别"阿司匹林"作为化学药物。

### 大数据集处理方案

高效处理CPU/GPU上的大规模数据集：
```python
from transformers.pipelines.pt_utils import KeyDataset
from datasets import Dataset
import pandas as pd

# 加载医疗数据集
medical_dataset = load_dataset("BI55/MedText", split="train[:100]")
data = pd.DataFrame({"text": medical_dataset["Completion"]})
dataset = Dataset.from_pandas(data)

# 根据硬件配置批量处理
batch_size = 16  # 根据GPU内存调整
results = []

for out in ner_pipeline(KeyDataset(dataset, "text"), batch_size=batch_size):
    results.extend(out)

print(f"已处理{len(results)}条文本")
```

## 真实应用场景

医疗实体识别(NER)技术从非结构化文本中提取关键信息。在医疗领域，它能实现三大核心功能：

🔒 **匿名化处理**：自动识别并脱敏医疗记录中的个人信息(PHI)，既保护患者隐私(符合HIPAA等法规要求)，又保持数据研究价值。比人工处理更高效准确。

🔗 **实体关系提取**：建立医疗实体间的关联网络(如药物→副作用，疾病→症状)，支撑临床决策系统、药物研发和个性化治疗。

💡 **HCC编码**：从病历中自动提取诊断信息生成分级条件类别(HCC)编码，优化医保报销流程，识别高风险患者提前干预。

## 加入OpenMed社区

欢迎访问[Hugging Face](https://huggingface.co/OpenMed)加入OpenMed社区交流想法。

## 公平开放原则
* **许可证**：Apache 2.0——自由使用、修改和分享
* **透明信息**：每个模型都有详细说明文档

## 总结

OpenMed的380+NER模型集合将顶尖性能与零成本完美结合，彻底开放医疗AI资源。无论您从事研究、临床还是开发工作，这些模型都能提供强大支持：
* 🥇 **性能卓越**：最高超越商业方案36%
* 🆓 **完全免费**：永久开源
* 🚀 **简单易用**：兼容现有工具链
* 🌍 **社区共建**：加入开发者生态

立即访问 [huggingface.co/OpenMed](https://huggingface.co/OpenMed) 开始探索。让我们携手推动智慧医疗发展！