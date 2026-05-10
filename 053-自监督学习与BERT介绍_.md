# 53：自监督学习与BERT介绍 🧠

![](images/5012be18fce6.png)

![](images/3d3a1c3a4e9b.png)

![](images/4ac6e50dd5fc.png)

![](images/3b43aa7c4584.png)

![](images/7280982c20ba.png)

在本节课中，我们将要学习自监督学习（Self-supervised Learning）的核心概念，并深入探讨一个著名的自监督学习模型——BERT。我们将了解BERT如何在没有标注数据的情况下进行预训练，以及如何将其应用于各种下游任务。

![](images/acf3427ec1ec.png)

![](images/3fe4b3244fbf.png)

![](images/e8e3a36355b9.png)

---

![](images/1cc554c477e0.png)

![](images/b640fc08dc65.png)

## 什么是自监督学习？ 🤔

![](images/e12974dcef1d.png)

![](images/142ac85cff47.png)

![](images/3e52a1266a7b.png)

![](images/ffdc20c6dd39.png)

![](images/96ec2f1a83c3.png)

上一节我们介绍了监督学习的基本概念。在监督学习中，模型需要输入X和对应的标签Y来进行训练。例如，进行情感分析时，我们需要大量标注了正面或负面标签的文章。

![](images/7042c5244d35.png)

![](images/56cace6e60b5.png)

![](images/b58aaf71592e.png)

![](images/e78cc727d838.png)

![](images/cd9253479878.png)

![](images/a21e7d60281b.png)

![](images/1aeff1952702.png)

![](images/3e09070825b0.png)

那么，什么是自监督学习呢？自监督学习是指在没有人工标注标签的情况下，模型自己想办法创造监督信号进行学习。具体来说，我们将数据X分成两部分：一部分作为模型的输入（X‘），另一部分作为模型的学习目标或“伪标签”（X“）。模型的目标是让输出Y尽可能接近X“。

![](images/63d1266e4992.png)

![](images/4c9f3ce8cea8.png)

![](images/5d85e5fb9c87.png)

![](images/7aa77b201901.png)

![](images/a410d43ba5ef.png)

![](images/2287dd6670cf.png)

![](images/7316bfdb9fc2.png)

自监督学习可以被看作是无监督学习（Unsupervised Learning）的一种方法，因为整个过程没有使用人工标注的标签。但为了更明确地描述这种“自己创造标签”的学习方式，我们特别称之为“自监督学习”。

![](images/ee8fa774495b.png)

![](images/c759425d5489.png)

---

![](images/c61143a717eb.png)

![](images/ce747295f2d2.png)

## BERT模型详解 🏗️

![](images/f415086dd79c.png)

![](images/2f4320b2951d.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c2cfb9e086c9a08908bdb11df43dc50c_73.png)

![](images/b073d68837db.png)

![](images/a1e917ee366d.png)

现在，我们来看看自监督学习的一个具体实例——BERT模型。BERT是一个基于Transformer架构的模型，更具体地说，它使用了Transformer的编码器（Encoder）部分。

![](images/c95b1b3c4974.png)

![](images/6fa7b81ef061.png)

![](images/3fb1461503fe.png)

![](images/b9cc2008178e.png)

![](images/cdc656ef5246.png)

![](images/7b5d6ede0e04.png)

![](images/276cae2b522c.png)

![](images/a29e95d16100.png)

BERT的输入通常是一段文字序列。在预训练阶段，我们会随机遮盖（Mask）输入序列中的一些词汇。遮盖有两种具体做法：

- 用一个特殊的 `[MASK]` 符号替换原词汇。
- 用字典中随机挑选的另一个词汇替换原词汇。

![](images/83cf25d067ac.png)

![](images/09ebadfeb742.png)

![](images/27481c89c417.png)

![](images/5f409737a96f.png)

![](images/61b4049b1cf7.png)

![](images/f31942b0390b.png)

![](images/2599c3191c18.png)

![](images/7b392f64e000.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c2cfb9e086c9a08908bdb11df43dc50c_111.png)

模型的目标是预测被遮盖住的原始词汇是什么。这本质上是一个填空任务。

![](images/9b51f55e7d6b.png)

![](images/4e1fe7b997e0.png)

![](images/6c56ac0094c4.png)

![](images/8eab46b4d9e8.png)

![](images/b38da0e8ea85.png)

![](images/caae23adcb59.png)

![](images/23f9789237dd.png)

![](images/79cfad3a1499.png)

![](images/bf022d00f5f1.png)

以下是BERT预训练（做填空题）的核心流程描述：

1. 输入一个句子，随机遮盖其中部分词汇。
2. 将处理后的序列输入BERT模型。
3. BERT为序列中每个位置（包括被遮盖的位置）输出一个向量。
4. 将被遮盖位置对应的输出向量，通过一个线性变换层和Softmax函数，转换成一个概率分布。
5. 训练目标是让这个概率分布预测出被遮盖的原始词汇。这相当于一个多类别的分类问题，类别数等于词汇表大小。

![](images/803a6c52211a.png)

![](images/4327a81df87b.png)

除了遮盖预测，BERT在原始论文中还使用了“下一句预测”（Next Sentence Prediction）任务，即判断两个句子是否连续。但后续研究发现这个任务对模型能力的提升帮助有限，甚至有一些改进模型采用了“句子顺序预测”（Sentence Order Prediction）等替代任务。

![](images/9e32e614277f.png)

---

![](images/9a16e6cfcd9e.png)

## BERT的应用：微调（Fine-tuning） 🔧

![](images/6f8cb0c311eb.png)

BERT在预训练阶段只学会了“填空”，那么它如何解决我们实际关心的任务呢？关键在于**微调**。

![](images/91645d49acbc.png)

![](images/e78fac0b17a6.png)

![](images/a0a0e2135817.png)

我们可以把预训练好的BERT模型看作一个具有强大理解能力的“基础模型”。当我们需要解决一个具体的下游任务（如情感分类、问答）时，就在BERT模型后面添加一个简单的任务特定层（例如一个线性分类器），然后使用少量该任务的标注数据，对整个模型（BERT+任务层）进行微调。

![](images/e40693c9d82e.png)

![](images/b9bc7e253bb8.png)

![](images/2d44824998e3.png)

在这个过程中，BERT模型本身的参数不是随机初始化的，而是加载了预训练好的权重。这通常比完全随机初始化能带来更好的性能和更快的收敛速度。

![](images/a494a8676ae9.png)

![](images/99851f1cd8b1.png)

![](images/af4c9bac9378.png)

以下是BERT应用于不同下游任务的几个典型案例：

![](images/9faad8aaf6da.png)

![](images/77c0207d6e98.png)

![](images/174a2a7c731d.png)

![](images/fb453b316b00.png)

**案例一：句子分类（如情感分析）**

- **任务**：输入一个句子，输出一个类别（如正面/负面）。
- **做法**：在句子前添加 `[CLS]` 符号，输入BERT。取 `[CLS]` 位置对应的输出向量，通过一个线性分类器得到类别。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c2cfb9e086c9a08908bdb11df43dc50c_167.png)

![](images/97dbeafa9bc4.png)

![](images/8f682db9e116.png)

**案例二：序列标注（如词性标注）**

- **任务**：输入一个句子，为句中每个词输出一个标签（如名词、动词）。
- **做法**：将句子输入BERT，取每个词对应位置的输出向量，分别通过线性分类器预测其标签。

![](images/cd219db658ee.png)

![](images/20966c91e43e.png)

![](images/8b14cffc2e1d.png)

![](images/92aefd44889a.png)

**案例三：句子对分类（如自然语言推理）**

- **任务**：输入两个句子，输出一个关系类别（如蕴含、矛盾）。
- **做法**：将两个句子用 `[SEP]` 符号分隔，并在最前面添加 `[CLS]`，一起输入BERT。取 `[CLS]` 位置的输出向量进行分类。

![](images/36dbd0e7445d.png)

![](images/173c97fb272b.png)

![](images/8f4e7eb14d6a.png)

![](images/16253eedcb50.png)

**案例四：抽取式问答（如作业七任务）**

- **任务**：输入一篇文章和一个问题，从文章中截取一个片段作为答案。
- **做法**：将问题和文章用 `[SEP]` 连接，前面加上 `[CLS]`，输入BERT。引入两个可学习的向量，分别与文章部分每个位置的输出向量计算点积并通过Softmax，得到答案起始位置和结束位置的概率。

![](images/bd527cd84c3c.png)

---

![](images/9ed68606d3de.png)

![](images/85ae74ba4554.png)

![](images/10bfde1c828e.png)

![](images/d8b2c9a666ce.png)

## 关于BERT的补充说明 💡

![](images/e0613080d7cb.png)

![](images/e3709a16cab4.png)

BERT的预训练需要海量的无标注文本数据和巨大的计算资源（例如使用TPU训练数天），因此个人通常难以从头训练一个BERT模型。但幸运的是，我们可以直接使用公开的预训练模型，并在此基础上进行微调，这只需要相对少量的计算资源。

![](images/d84e4d1e4470.png)

![](images/0b6abced7697.png)

![](images/3eab3e0d9d42.png)

![](images/2115a170e113.png)

![](images/7aa16be71a6a.png)

此外，BERT主要预训练了编码器部分。对于序列到序列（Seq2Seq）的任务，也有相应的方法对完整的Transformer（编码器-解码器）模型进行预训练，例如通过破坏输入文本再让模型还原等方式。

![](images/7d97f7607111.png)

![](images/c2b9fb09c50f.png)

![](images/49fb587db86d.png)

---

![](images/3b0bbfda6f21.png)

![](images/0ee8b4784fc6.png)

![](images/eb82f2ac4268.png)

![](images/c5a305844772.png)

![](images/f5191f1b2dfe.png)

![](images/d44fcb5f5dc1.png)

## 总结 📚

![](images/aa16d0d4e2b8.png)

![](images/7757dd515d14.png)

![](images/811c55ad4bbc.png)

![](images/9e40ca1b5a81.png)

![](images/6c7220d05796.png)

![](images/6c1b86ec5411.png)

![](images/0c28bcab8687.png)

![](images/76f91dcaa95e.png)

![](images/942e0ddf1f98.png)

本节课中我们一起学习了自监督学习和BERT模型：

1. 自监督学习让模型在没有标注的数据上，通过设计 pretext task（如填空）来自己生成监督信号进行学习。
2. BERT是一个基于Transformer编码器的著名自监督学习模型，其核心预训练任务是“掩码语言模型”（Masked Language Model），即预测被随机遮盖的词汇。
3. 预训练后的BERT模型可以通过**微调**，适配到多种下游自然语言处理任务中，如图分类、序列标注、句子对理解和问答等，通常只需在BERT后添加简单的任务层并使用少量标注数据。
4. BERT的强大能力建立在海量数据和算力之上，但其“预训练+微调”的模式使得我们能够利用其强大的语言理解能力来解决实际问题。
