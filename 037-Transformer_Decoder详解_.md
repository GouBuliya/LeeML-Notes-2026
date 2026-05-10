# 37：Transformer Decoder详解 🧠

## 概述

在本节课中，我们将学习Transformer模型中的Decoder部分。我们将详细讲解Decoder的两种主要类型：自回归Decoder和非自回归Decoder，并深入探讨它们的工作原理、内部结构以及训练方法。通过本课程，你将能够理解Decoder如何生成输出序列，并掌握相关的训练技巧。

---

## Decoder的两种类型

上一节我们介绍了Encoder，本节中我们来看看Decoder。Decoder主要有两种类型：自回归Decoder和非自回归Decoder。我们将重点介绍常见的自回归Decoder，并以语音辨识为例进行说明。

![](images/d340bb62f0b7.png)

![](images/8fe836bf6c8c.png)

### 自回归Decoder的工作原理

自回归Decoder的运作方式如下：

1. 输入一段声音信号给Encoder，Encoder输出一排向量。
2. Decoder读取Encoder的输出，并生成输出序列。
3. Decoder的输入包括一个特殊的开始符号（BOS），输出为一系列文字。

![](images/a9cb3770e9b4.png)

以下是自回归Decoder的生成过程：

- Decoder首先输入BOS符号，生成第一个输出（例如“机”）。
- 将“机”作为新的输入，生成第二个输出（例如“器”）。
- 重复此过程，直到生成完整的输出序列。

![](images/476cde53d97b.png)

自回归Decoder的核心特点是：**Decoder会将自己的输出作为后续的输入**。这种机制可能导致错误传播问题，即一步错步步错。我们将在后续讨论如何处理这个问题。

![](images/ff0284fe4f3c.png)

![](images/70d39bcce0a9.png)

![](images/21112d507b25.png)

![](images/d1c4eca5fee4.png)

![](images/304efa4bb14f.png)

![](images/29c4de7d20ae.png)

![](images/1f25bcf92254.png)

![](images/3510dc1a3be6.png)

---

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/3c01bac6b64da49f6a69c0c7d9a2a5ae_27.png)

![](images/6d62dbb3efbb.png)

![](images/ee3de2340d39.png)

## Decoder的内部结构

接下来，我们来看看Decoder的内部结构。Decoder的结构比Encoder稍复杂，但整体框架相似。

![](images/65b9ff3c7d91.png)

### Masked Self-Attention

Decoder中的Self-Attention与Encoder不同，它采用了Masked Self-Attention。Masked Self-Attention的作用是限制Decoder只能关注当前位置之前的输入，而不能看到未来的信息。

具体来说：

- 生成第一个输出时，只能使用第一个位置的输入信息。
- 生成第二个输出时，只能使用前两个位置的输入信息。
- 以此类推，直到生成完整的输出序列。

这种设计符合Decoder的生成过程，因为输出是一个一个产生的，无法提前看到未来的信息。

![](images/8a129316a81e.png)

![](images/cd70e10a7208.png)

![](images/bfd444eb2411.png)

![](images/de608b808541.png)

### Cross-Attention

Cross-Attention是连接Encoder和Decoder的桥梁。它的工作原理如下：

1. Decoder生成一个Query（Q）。
2. Encoder提供Key（K）和Value（V）。
3. Decoder通过计算Q与K的注意力分数，从Encoder的输出中抽取信息，生成最终的输出。

Cross-Attention使得Decoder能够利用Encoder的输入信息，生成更准确的输出序列。

![](images/7d1e571ccfab.png)

---

## 非自回归Decoder

除了自回归Decoder，还有一种非自回归Decoder（NAT）。NAT的运作方式如下：

1. 输入一排BOS符号给Decoder。
2. Decoder一次性生成完整的输出序列。

![](images/a206f11a442d.png)

NAT的优势在于：

- **平行化处理**：一次性生成整个序列，速度更快。
- **可控输出长度**：可以通过额外的分类器预测输出序列的长度。

![](images/a3bb79af940b.png)

然而，NAT的性能通常不如自回归Decoder，需要更多的技巧才能达到相近的效果。

---

![](images/7a6a1deb755b.png)

## Decoder的训练

在训练Decoder时，我们需要使用训练数据，例如语音辨识中的声音信号和对应的文字。训练过程如下：

1. 将声音信号输入Encoder，得到输出向量。
2. 将Encoder的输出和正确答案输入Decoder，计算每个输出的交叉熵损失。
3. 通过最小化总损失，优化模型参数。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/3c01bac6b64da49f6a69c0c7d9a2a5ae_51.png)

### Teacher Forcing

在训练过程中，Decoder的输入是正确答案，而不是自己的输出。这种方法称为Teacher Forcing。然而，在测试时，Decoder只能看到自己的输出，这可能导致训练与测试的不一致问题。

![](images/34cc3cdf56f6.png)

![](images/3095e73db541.png)

为了解决这个问题，可以采用以下方法：

- **Scheduled Sampling**：在训练时，偶尔将Decoder自己的输出作为输入，而不是总是使用正确答案。
- **Exposure Bias**：通过给Decoder输入一些错误的东西，提高其鲁棒性。

---

![](images/eb5e6f1b7996.png)

## 训练技巧

以下是训练Decoder时的一些实用技巧：

### Copy Mechanism

在某些任务中，Decoder需要从输入中复制一些内容到输出中。例如，在聊天机器人或摘要生成任务中，复制机制可以显著提高性能。

### Guided Attention

对于语音辨识或语音合成任务，注意力机制应该由左向右移动。通过引导注意力机制，可以强迫模型按照预期的方向生成输出。

![](images/4bf5b3d690d0.png)

![](images/8a9122b4425f.png)

### Beam Search

Beam Search是一种搜索算法，用于在生成输出序列时找到近似最优解。它通过保留多个候选路径，避免贪心解码可能带来的局部最优问题。

### 随机性

在某些任务中，如语音合成或句子补全，加入随机性可以提高输出的自然度和多样性。

---

![](images/f43b4246d6ca.png)

## 总结

本节课中，我们一起学习了Transformer模型中的Decoder部分。我们详细介绍了自回归Decoder和非自回归Decoder的工作原理、内部结构以及训练方法。通过本课程，你应该能够理解Decoder如何生成输出序列，并掌握相关的训练技巧，如Teacher Forcing、Copy Mechanism和Beam Search等。希望这些知识能够帮助你在实际应用中更好地使用Transformer模型。

![](images/588eae386d2f.png)

![](images/ac6cd1aa7632.png)

![](images/a476fb269ec0.png)

---
