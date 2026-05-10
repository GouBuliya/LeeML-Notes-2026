# 35：作业四（Speaker Identification）说明 🎤🤖

在本节课中，我们将学习如何完成第四次作业，其核心主题是**注意力机制（Attention）**。本次作业的目标是构建一个模型，能够识别一段语音属于哪位说话者，这是一个典型的分类任务。我们将使用Transformer架构，并探索其变体。

![](images/fbca8155ef28.png)

---

![](images/dc6878ba87c8.png)

## 任务概述与数据集 📊

![](images/9023af42dc2a.png)

本次作业的任务是**说话人识别（Speaker Identification）**。模型的输入是一段语音，输出是这段语音对应的说话者身份。

我们使用的数据集是 **VoxCeleb2**。这是一个在真实世界环境下录制的语音数据集。

- **训练集**：包含约56,000条语音。
- **测试集**：包含约4,000条语音。
- 我们从所有说话者中随机选取了 **600** 位，作为本次作业使用的数据子集。

---

![](images/322e8fad5dae.png)

## 数据预处理 🔧

![](images/3adabc9e849d.png)

数据预处理流程与作业二类似，但省略了最后一步的语音增强。我们使用 **MFCC（梅尔频率倒谱系数）** 作为语音特征，而没有使用语谱图。如果你希望深入了解MFCC，可以参考课程提供的链接。

下载数据集后，目录下主要包含以下文件：

- `metadata.json`：包含数据集的元信息。
- `testdata.json`：测试数据的相关信息。
- `mapping.json`：将说话者ID（字符串）映射为数字标签。
- 每条语音以 `utterance-{id}.pt` 格式的文件存储。

![](images/f3d6125c50f5.png)

在 `metadata.json` 中，`speakers` 字段下列出了每位说话者的所有语音片段。每条语音信息包含 `feature_path`（文件路径）和 `mel_len`（该语音的MFCC特征长度）。由于不同语音的长度不一致，这会给模型训练带来困难。

为了解决长度不一致的问题，我们采用 **分段采样（Segmentation）** 技术。我们将每条长语音切分成多个较短的片段，在训练时随机选取一个或多个连续的片段作为该条语音的代表。

---

## 模型架构 🏗️

上一节我们介绍了任务和数据，本节中我们来看看模型的基本架构。

模型的处理流程如下：

1. **输入**：一段预处理好的语音特征序列。
2. **编码器（Encoder）**：对输入进行特征抽取，输出高维特征序列。
3. **池化层（Pooling Layer）**：由于编码器输出的特征数量很多，我们使用池化层进行下采样，得到一个固定维度的向量，作为整段语音的**表征（Representation）**。
4. **分类器（Classifier）**：将表征向量输入一个全连接层（Linear Layer）进行分类。
5. **输出**：一个600维的向量，代表模型预测该语音属于600位说话者中每一位的概率分布。

![](images/c0d956293683.png)

模型的核心公式可以简化为：  

`输出 = 分类器(池化(编码器(输入)))`

---

## 作业要求与评分基准 🎯

![](images/4f7c7bc33df1.png)

![](images/2f65a2980044.png)

本次作业分为四个部分，每个部分都有对应的评分基准（Baseline）。

以下是各个基准的简要介绍：

- **Simple Baseline**
  
  **目标**：熟悉代码框架，运行提供的示例代码。
  **操作**：直接运行代码即可。
  **预期分数**：约0.60+。
  **训练时间**：约30-40分钟（在Colab免费版上）。
- **Medium Baseline**
  
  **目标**：通过调整Transformer模型的超参数来提升性能。
  **可调参数**：
  
  减少或调整分类器中全连接层的复杂度。
  调整Transformer中注意力头（Head）的数量。
  调整 `d_model` 参数（即输入特征被投影到的维度）。
  调整编码器层（Encoder Layer）的堆叠数量（`n_layers`）。
  
  
  **预期分数**：0.70375。
  **训练时间**：约1-1.5小时。

![](images/36acd690a1b8.png)

- **Strong Baseline**
  
  **目标**：实现并集成 **Conformer** 模型。
  **说明**：Conformer是Transformer的一种变体，它在架构中融合了卷积神经网络（CNN），能更好地捕捉局部特征。你需要将原始代码中的编码器替换为Conformer编码器。
  **预期分数**：0.77750。
  **训练时间**：约3-4小时。

![](images/edb0d8dcc7fa.png)

- **Boss Baseline**
  
  **目标**：实现两个进阶模块。
  **任务**：
  
  **Self-Attention Pooling**：将平均池化（Mean Pooling）层替换为自注意力池化层，让模型学习如何加权聚合所有时间步的特征。
  **Additive Margin Softmax**：使用带加性间隔的Softmax损失函数，让分类边界更清晰，以提升模型判别能力。
  
  
  **预期分数**：0.8600。
  **训练时间**：约2小时以上。

---

## 提交格式与评分标准 📝

![](images/7fd8aa11eec9.png)

模型的性能以分类准确率（Accuracy）评估。

- **Simple/Medium/Strong/Boss Baseline**：每达到一个基准的公开（Public）和私有（Private）测试集分数，各获得 **0.5分**，共计最高 **2分**。
- **代码提交**：成功提交代码可获得 **2分**。
- **报告（Report）**：共 **4分**。

![](images/a5a732ac94e6.png)

提交的预测结果文件应为CSV格式：

- 第一行（表头）为：`Id,Category`。
- 后续每一行对应一条测试语音的预测结果，例如：`utterance-0001,5`。
- 文件总行数应为4001行（1行表头 + 4000行数据）。

![](images/ec57be865ec1.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/33791df696a3000667834298814d2ed1_29.png)

代码需提交至NTU COOL系统，命名格式为：`{学号}_{姓名}_HW4.zip`。

![](images/89cde18b6561.png)

---

![](images/b94eb6a1ce31.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/33791df696a3000667834298814d2ed1_35.png)

![](images/e606b0af06fd.png)

## 报告问题与作业规定 📄

![](images/1cd3f88c4ee5.png)

![](images/912263dd4a32.png)

![](images/bc2ba419a4ff.png)

报告部分需要回答两个问题，共占4分。

![](images/6075053663ef.png)

![](images/25daec7888b1.png)

以下是两个报告问题：

1. 请简要介绍一种Transformer的变体模型（例如Conformer，或其他你在网络上找到的变体）。**(2分)**
2. 请简要解释为什么在Transformer中加入卷积层（Convolutional Layer）能够提升模型性能。**(2分)**

![](images/5135dbe4e2fd.png)

**作业规定**：

- **截止日期**：代码、报告提交截止日期为三周后。
- **学术诚信**：严禁抄袭、人工修改预测结果、分享代码或模型给他人、使用外部数据或预训练模型（如Wav2Vec2）。
- **违规处罚**：违反规定者，本次作业成绩将乘以0.9（即打九折）。
- **问题咨询**：如有问题，请在NTU COOL讨论区提问，或邮件联系助教（邮件主题请以`[HW4]`开头）。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/33791df696a3000667834298814d2ed1_50.png)

![](images/ec45a46d741b.png)

---

![](images/9fa4d26004e5.png)

![](images/81aced852aed.png)

![](images/c739bbf7a28f.png)

## 代码框架讲解 💻

![](images/42f2e057b475.png)

![](images/825b6ec16524.png)

现在，让我们深入了解一下提供的代码框架。代码主要包含以下几个部分：

![](images/ab7c2348676a.png)

![](images/8d2e78be1f30.png)

1. **数据下载与解压**：代码会自动从GitHub下载数据集并解压。
2. **固定随机种子**：设置随机种子以保证实验结果可复现。
3. **数据集与数据加载器**：`Dataset` 和 `DataLoader` 相关设置，通常无需修改。
4. **模型定义**：这是需要重点修改的部分。模型结构如之前所述，包含：
  
  `PreNet`：将输入投影到 `d_model` 维度。
  `Encoder`：特征抽取模块。
  `Decoder`（实际为分类器）：将池化后的特征映射到说话者类别。
  `Pooling`：目前为平均池化，需修改为自注意力池化（Boss Baseline）。
5. **学习率调度**：包含热身（Warm-up）阶段和训练阶段，你可以尝试不同的调度策略。
6. **训练与验证循环**：包含计算损失、准确率的核心函数。
7. **超参数设置**：如批量大小（`batch_size`）、验证步数间隔（`validation_step`）、总训练步数（`total_steps`）等。
8. **推理脚本**：用于加载训练好的模型并对测试集进行预测，生成提交文件。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/33791df696a3000667834298814d2ed1_68.png)

![](images/a355c8ceba20.png)

对于使用Conformer，你可以选择自己实现，也可以使用现有的开源套件进行集成。

![](images/895fa9cf7f4b.png)

---

![](images/ee2164b7a545.png)

## 总结 🎉

![](images/b5573aa469ab.png)

![](images/066870644ccf.png)

![](images/84d1a75709ad.png)

本节课中，我们一起学习了第四次作业的完整内容：

1. **任务核心**：使用基于**注意力机制**的模型（Transformer及其变体）完成说话人识别任务。
2. **数据处理**：了解了VoxCeleb2数据集以及如何处理长度不一的语音数据（分段采样）。
3. **模型架构**：掌握了从编码器、池化层到分类器的基本模型流程。
4. **进阶挑战**：认识了四个逐步提升的基准任务，包括调整参数、集成Conformer、实现自注意力池化和加性间隔Softmax。
5. **实践指南**：熟悉了代码框架的结构和需要修改的关键部分，以及最终的提交格式和评分标准。

![](images/d5a4e06915da.png)

希望大家通过本次作业，能够深入理解Attention机制及其在语音任务中的应用。祝你顺利完成作业！
