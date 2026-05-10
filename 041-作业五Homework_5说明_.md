# 41：作业五（Homework 5）说明 📚

![](images/9bb6f3ad46eb.png)

![](images/344b4042f710.png)

在本节课中，我们将学习第五次作业（Homework 5）的详细内容。本次作业的核心任务是实现一个**机器翻译模型**，将英文句子翻译成中文。我们将介绍任务目标、数据处理流程、模型构建技巧、评分标准以及新的作业提交平台Judge Board的使用方法。

---

## 任务概述 🎯

本次作业的目标是构建一个**序列到序列（Sequence-to-Sequence）** 的翻译模型。具体而言，我们需要将英文句子翻译成中文句子。

例如，输入英文句子 “Thank you so much chris”，模型应输出中文翻译 “非常谢谢克里斯”。

![](images/bd39a1002ac8.png)

![](images/f659f6d81fa1.png)

我们将使用 **BLEU分数** 作为评估指标，它通过比较模型输出与标准答案的相似度来评分。

---

## 整体工作流程 🔄

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1c0d9646a6905c392c97e5ecdae1f610_9.png)

![](images/15f69576f8de.png)

上一节我们明确了任务目标，本节中我们来看看完成该任务的标准流程。

整个项目流程可以分为以下几个步骤：

1. **数据预处理**：下载原始数据，进行清洗和分词。
2. **模型定义**：构建编码器-解码器架构的神经网络模型。
3. **模型训练**：使用训练数据对模型进行训练。
4. **推理与评估**：使用测试数据进行翻译，并计算BLEU分数。

![](images/b5c60ea352ba.png)

以下是每个步骤的详细说明：

### 数据预处理

![](images/4f6853ca085d.png)

首先，我们需要下载并准备数据。数据集包含平行的中英文句子对（来自TED Talks 2020）以及一部分仅包含中文的语料。

预处理步骤包括：

- 过滤掉过长或过短的句子。
- 进行**分词**，将句子转换为子词单元，这有助于减少词汇表大小并处理未登录词。例如，一个单词可以被拆分为更小的、有意义的单元。

### 模型训练

在定义好模型架构后，我们使用训练数据对模型进行训练。训练过程中会涉及优化器、学习率调整等超参数的设置。

![](images/7dadbaa021a6.png)

![](images/c992a519b26b.png)

![](images/6bba9bec1999.png)

![](images/be1fbcd245dc.png)

### 推理与评估

训练完成后，我们将测试数据输入模型，得到翻译结果，并使用BLEU分数评估模型性能。

![](images/670e9eb6aec7.png)

---

![](images/2456c182654b.png)

## 模型优化技巧 ⚙️

![](images/66ecb25868e9.png)

![](images/4cc3ba865356.png)

为了提升翻译模型的性能，我们可以采用以下几种技术。以下是具体的方法介绍：

![](images/b7019c63d6f4.png)

- **分词**：使用子词分词（如BPE），将单词拆分为更小的单元，以有效管理词汇表大小并提升模型对未知词的泛化能力。
- **标签平滑**：在分类任务中，模型通常会输出一个one-hot向量。标签平滑通过给其他类别分配很小的概率，防止模型对训练数据过度自信，从而缓解过拟合。
  
  公式：`平滑后的标签 = (1 - ε) * one-hot标签 + ε / K`，其中K是类别总数。
- **学习率调度**：采用预热策略，训练初期从一个较小的学习率逐渐升高，在达到指定步数后再按计划衰减，这有助于训练稳定。
- **回译**：利用仅有的中文单语数据。首先训练一个“中文到英文”的翻译模型，然后用这个模型将中文单语数据翻译成英文，从而生成额外的平行语料，用于增强“英文到中文”主模型的训练。
  
  **注意**：回译模型的质量至关重要；增加数据后，可能需要增大主模型的容量。

---

## 基线模型与实现目标 🏁

我们将作业完成度分为四个等级，每个等级对应不同的模型复杂度和性能预期。

![](images/5d11af75a4cf.png)

- **Simple Baseline**：运行基础代码即可达到。预计在Kaggle P100 GPU上1小时内可训练完成。
- **Medium Baseline**：需要在Simple Baseline基础上加入**学习率调度**并延长训练时间。预计训练时间增加约40分钟。
- **Strong Baseline**：需要将编码器和解码器从RNN架构替换为**Transformer**架构，并调整相关参数。预计训练时间约3小时。
- **Boss Baseline**：需要实现并应用**回译**技术。由于需要先训练一个反向模型，且数据量增大，预计训练时间将超过12小时。

![](images/2ea3b70d1f9d.png)

![](images/8ede3f85d4a7.png)

---

![](images/b4c8895624a0.png)

![](images/61ac7b239e8b.png)

![](images/64e727ab9361.png)

![](images/0f88e0c32853.png)

![](images/0f296d67e987.png)

## Report 报告要求 📝

![](images/8e5b74d98888.png)

![](images/e6496acd5125.png)

![](images/7810fd5dea18.png)

除了模型性能，还需完成一份报告，包含以下两个问题。

![](images/9fda755d2175.png)

![](images/9c1d8146ad9e.png)

![](images/1370ec3c6600.png)

![](images/059a314cad9b.png)

### 第一题：可视化位置编码

![](images/530396fa7abb.png)

![](images/4bfd2d9bad76.png)

![](images/d06c46df1554.png)

在Transformer模型中，位置编码用于为序列添加顺序信息。本题要求可视化Decoder部分位置编码两两之间的相似度。

![](images/541239395669.png)

![](images/7a410efa9c73.png)

![](images/828f57fb1a46.png)

![](images/13fd7a64fe2e.png)

![](images/fce2ce5e4395.png)

- **任务**：假设位置编码查找表是一个 `N x D` 的矩阵，计算所有位置编码两两之间的相似度，得到一个 `N x N` 的相似度矩阵，并将其可视化。
- **预期**：由于相近的位置应具有较高的相似度，可视化结果应在矩阵对角线附近显示出较强的相似性。
- **计算方法**：推荐使用余弦相似度。PyTorch内置了计算函数：`torch.nn.functional.cosine_similarity`。
- **代码提示**：提取Decoder位置编码的代码如下：`pos_embed = model.decoder.embedding.pos_embedding
  `

![](images/d085be4464c6.png)

![](images/4ce1d0c9e07c.png)

![](images/7c0fdbc2e839.png)

![](images/0f0d672c15d5.png)

![](images/6a6338364b00.png)

### 第二题：梯度裁剪与梯度爆炸

![](images/6f6ebe0f6680.png)

![](images/e35ba1f5f23c.png)

![](images/42eab4aa305d.png)

![](images/5c9a3a046c85.png)

在训练序列模型时，**梯度爆炸**是一个常见问题，它会导致训练不稳定甚至失败。解决方法是使用**梯度裁剪**。

![](images/087cd9ace06c.png)

![](images/165a180e39c6.png)

![](images/ad7ae8c8420f.png)

![](images/47f774857abc.png)

- **梯度爆炸**：指在误差曲面非常陡峭的区域，梯度值变得极大，导致参数更新步长过大。
- **梯度裁剪**：其步骤可概括为：
  
  设置一个最大范数值 `max_norm`。
  计算所有参数梯度的范数 `total_norm`。
  如果 `total_norm > max_norm`，则将所有梯度按比例缩放：`gradient *= max_norm / total_norm`。
- **任务**：
  
  实作梯度裁剪，并在训练过程中记录每个步骤的梯度范数。
  将梯度范数随时间的变化可视化出来。
  在图中圈出两处可能发生梯度爆炸的位置（如果存在）。
- **代码提示**：PyTorch中可使用 `torch.nn.utils.clip_grad_norm_` 函数。关键代码如下：`# 在训练循环中
  loss.backward()
  grad_norm = torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm)
  optimizer.step()
  # 将 grad_norm 存储到列表中以便后续绘图
  `

![](images/b93b5467b5a7.png)

![](images/6fad33e790e0.png)

![](images/e037cb3c64d7.png)

![](images/cf767e08de5f.png)

![](images/30dc216a05cc.png)

---

![](images/8689c8966db8.png)

![](images/6997dce363d4.png)

![](images/d4e46d28c9fb.png)

![](images/e58f098668fe.png)

![](images/70f6864f46b6.png)

## Judge Board 使用指南 💻

![](images/057fba29dfe7.png)

![](images/e4d8b8880cd9.png)

![](images/59022ea27b99.png)

![](images/13778247a93c.png)

从本次作业开始，我们将使用新的作业提交与评分平台 **Judge Board**。其使用流程与Kaggle类似，但有几点需要特别注意。

![](images/287ae037019c.png)

![](images/2a02dfa41f1d.png)

![](images/e282591d29a7.png)

![](images/4efc470fb3a9.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1c0d9646a6905c392c97e5ecdae1f610_138.png)

以下是关键步骤和注意事项：

![](images/a17999aef39a.png)

![](images/4e8962e6d38b.png)

![](images/18de196feeb4.png)

![](images/8d3890988157.png)

- **前置条件**：必须提前填写助教发布的GitHub和CoLab表单，否则将无法上传作业（会被扣1分）。
- **登录**：使用GitHub账号登录Judge Board网站并授权。
- **上传**：在 “Submission” 页面选择生成的 `prediction.csv` 文件进行上传。
- **选择评分提交**：在 “Leaderboard” 页面，可以勾选至多两个提交记录作为最终评分依据。截止时间后，选择功能将失效。
- **使用规则**：
  
  每日有5次上传额度，午夜重置。
  只能上传 `.csv` 文件，且大小有限制。
  请勿在截止前最后时刻上传，以免因服务器拥堵导致失败。
  平台每周五早上6-9点（UTC+8）进行维护，期间可能无法访问。
- **问题反馈**：任何关于Judge Board的问题，请到课程NTU COOL讨论区的指定板块留言，并附上你的学号和GitHub ID。

![](images/4583e9ebc1d4.png)

![](images/c1e63738de4d.png)

![](images/7f698953490f.png)

![](images/018c05de5fcb.png)

---

![](images/e681571c1056.png)

![](images/cc4f9a762936.png)

![](images/0ce8865a7c56.png)

![](images/a9bf751db1ec.png)

![](images/674b1a340e4c.png)

## 代码结构导读 📂

![](images/f980013d6bcf.png)

![](images/e4af48888837.png)

![](images/20d70e115028.png)

![](images/e2ac6a8d723b.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1c0d9646a6905c392c97e5ecdae1f610_172.png)

最后，我们来简要浏览一下提供的代码框架，了解各个部分的功能。

![](images/e0c5531f2cd7.png)

![](images/4999f14451c0.png)

![](images/95e6b8669941.png)

代码主要包含以下模块：

![](images/65c1e5d0f74f.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1c0d9646a6905c392c97e5ecdae1f610_182.png)

![](images/b33cd36ff99f.png)

![](images/2870e776d3d0.png)

1. **环境设置与数据下载**：安装依赖包、下载原始数据集。
2. **数据预处理**：包含过滤句子、分词、构建词汇表、创建数据加载器等函数。
3. **配置与模型定义**：
  
  `config`：设置模型超参数（如隐藏层维度、学习率、训练轮数等）。
  `model.py`：定义了RNN Encoder, Decoder, Attention以及整个Seq2Seq模型。进阶目标需将其替换为Transformer。
4. **训练与优化**：
  
  `label_smoothing_loss`：实现标签平滑损失函数。
  `get_lr_scheduler`：实现学习率调度器（需在Medium Baseline中完成）。
  训练循环中集成了梯度裁剪和记录逻辑（用于Report第二题）。
5. **推理与提交**：
  
  `translate`：使用训练好的模型进行翻译。
  `submit`：生成符合提交格式的 `prediction.csv` 文件。
6. **回译实现**：`create_augmented_dataset` 函数展示了如何利用单语数据通过回译技术增强数据集（用于Boss Baseline）。

![](images/18675184fb95.png)

![](images/d5f0ccdc5f17.png)

![](images/02f6ebe10bdd.png)

![](images/f3964fe1cd4d.png)

---

![](images/f348919b956f.png)

![](images/e4c352afbdc3.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1c0d9646a6905c392c97e5ecdae1f610_198.png)

![](images/e2db8629aeff.png)

本节课中，我们一起学习了Homework 5的完整内容：从机器翻译的任务定义、数据处理流程、模型优化技巧，到具体的四个基线目标、两份报告问题的详解，以及新平台Judge Board的使用方法。请大家按照步骤，从Simple Baseline开始，逐步完成更高要求的模型，并按时提交作业和报告。
