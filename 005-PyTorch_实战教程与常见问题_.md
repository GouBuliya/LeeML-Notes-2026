# 5：PyTorch 实战教程与常见问题 🚀

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/5b458ef48d4d386d8d41139b42da0d32_0.png)

![](images/f7a2b5328374.png)

![](images/fee95c070deb.png)

![](images/7c71b0a65d5e.png)

![](images/a5fe08ee5ecc.png)

![](images/77c02d51693f.png)

![](images/61b12f964c7f.png)

在本节课中，我们将通过一个回归问题的实战案例，学习使用 PyTorch 构建和训练机器学习模型的完整流程。我们将从数据加载开始，逐步完成模型定义、训练循环，并了解如何查阅官方文档以及解决常见的编程错误。

![](images/6405f87fb406.png)

![](images/e3c6fa8cef61.png)

![](images/affa1bc7e3a6.png)

![](images/7c279bb1df64.png)

---

![](images/010bd2a743ee.png)

![](images/2fae5a3fbc9c.png)

![](images/070fe6d94e89.png)

![](images/4dd48ecb9d6e.png)

![](images/d73ddd0a405a.png)

## 数据检查与加载 📊

![](images/a0019ab1d143.png)

![](images/ae1836157ff0.png)

![](images/542db931a12a.png)

![](images/254c13df9496.png)

上一节我们介绍了 PyTorch 的基本组件，本节中我们来看看如何在实际项目中处理数据。

![](images/f262b76628e1.png)

![](images/cd5a1512f6ba.png)

![](images/9d0ea2ad783e.png)

![](images/015154aee0ef.png)

首先，在开始编写机器学习代码时，一个不错的起点是检查数据的样子。在这个案例中，数据存储在一个 `.csv` 文件中。该文件的每一行代表一条数据，总共有 118 列，其中前 117 列是特征，最后一列是标签。

![](images/c6831eef4519.png)

![](images/8fd87a66d3c4.png)

![](images/20e37035cdcb.png)

![](images/a57c83fac512.png)

了解数据的大致结构后，就可以开始将数据读入并进行简单的预处理。由于数据存储在 `.csv` 文件中，这里提供一个简单的方法，使用 `pandas.read_csv` 将其读入，读入后是一个 `DataFrame` 对象。

![](images/7fcd9c442e89.png)

![](images/5abeac8d7c94.png)

![](images/50f500ccdde5.png)

![](images/d3ad37171602.png)

以下是数据加载与预处理的步骤：

![](images/99e9e2e9c4a6.png)

![](images/ed09cc3dea26.png)

![](images/7020985e60e2.png)

![](images/c5b9f4557f68.png)

1. **读取数据**：使用 `pandas.read_csv` 函数。
2. **分离特征与标签**：将读入的数据分为模型输入 `x` 和标签 `y`。
3. **数据形状**：处理后，`x` 的形状是 `(2699, 117)`，代表有 2699 条训练数据，每条数据有 117 个特征。`y` 的形状是 `(2699,)`，代表对应的 2699 个标签值。

![](images/554740a592ec.png)

![](images/39b6ee0a835c.png)

![](images/af1ea1dda16f.png)

![](images/547015458dee.png)

> 注意：如果作业中没有提供验证集，通常需要自己从训练数据中划分出来。本教程为简化说明，省略了此步骤。实际操作可参考作业一的示例代码。

![](images/17614d95aca1.png)

![](images/0b54b2eb2305.png)

![](images/106db855808b.png)

![](images/7914511754db.png)

![](images/5b88ad54a3ca.png)

---

![](images/078d2a63e21b.png)

![](images/7c3cd45239cf.png)

![](images/b738bca80afc.png)

![](images/1cc7753a5921.png)

## 构建 Dataset 与 DataLoader 🔧

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/5b458ef48d4d386d8d41139b42da0d32_98.png)

![](images/8c4f581d3318.png)

![](images/8c8ec9f67d17.png)

![](images/50826b07660f.png)

处理完原始数据后，接下来可以声明一个 `Dataset` 类。和之前介绍的一样，我们需要编写三个方法：`__init__`、`__getitem__` 和 `__len__`。

![](images/a83b32c2351d.png)

![](images/523eef18a35f.png)

![](images/98ebca1b90f2.png)

![](images/c29d254f2e45.png)

- **`__init__`**：在此方法中读取数据或进行预处理。
- **`__getitem__`**：给定一个索引，此方法负责返回对应索引的数据和标签。在本例中，返回一个 117 维的特征向量及其对应的标签。
- **`__len__`**：返回整个数据集的总长度，本例中是 2699。

![](images/9a7e1e7975be.png)

![](images/cc24ff0058f9.png)

![](images/abc28c2427b3.png)

![](images/5ed97a286345.png)

![](images/180a4aff0b60.png)

声明完 `Dataset` 后，就可以声明 `DataLoader`。其中比较重要的参数是 `batch_size`（每个批次的数据量）和 `shuffle`（是否打乱数据顺序）。在训练时，通常将 `shuffle` 设为 `True`。

![](images/a84768da2bb5.png)

![](images/654416b10c68.png)

![](images/dd4305324de6.png)

![](images/46e9d95f0174.png)

---

![](images/f57952af5754.png)

![](images/85abe5873538.png)

![](images/3cd326593018.png)

![](images/823c812fec22.png)

## 定义模型与训练组件 🧠

![](images/7fa7788c5b18.png)

![](images/21adbe93a4b7.png)

![](images/7af0d0ab3f75.png)

![](images/3637e6d2a19d.png)

![](images/48c3eeddc099.png)

处理完数据部分，接下来看看模型的定义。

![](images/9f9536927f8c.png)

![](images/1b03b3fe782b.png)

![](images/ed27623f2569.png)

![](images/25a6b979ec5c.png)

这里举一个简单的例子：一个三层的线性模型。因为在这个任务中，每条数据的维度是 117，所以线性模型的输入维度也应是 117。最后的输出维度是 1，因为我们要预测一个数值。

![](images/6a662f1d8b10.png)

![](images/938d230b27c8.png)

![](images/49e0c5491b48.png)

![](images/a74f1c79465e.png)

定义好模型后，需要选择合适的损失函数。考虑到这是一个回归任务，选择均方误差损失是一个不错的选择。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/5b458ef48d4d386d8d41139b42da0d32_166.png)

![](images/62bd96e2691a.png)

![](images/88f615869813.png)

![](images/dd997acc6fc0.png)

![](images/705458cc8f0f.png)

```python
criterion = nn.MSELoss()
```

![](images/0469de953859.png)

![](images/ef54e35e0dbf.png)

![](images/10f1f507524a.png)

![](images/78c5dabfb582.png)

![](images/1f3a44561bb3.png)

接着是优化器的部分。这里简单举例，选用随机梯度下降作为优化器。

![](images/7cac39d24318.png)

![](images/e5c4da358935.png)

![](images/82423d9fee5b.png)

![](images/5abecd702675.png)

```python
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)
```

![](images/88833155ac7f.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/5b458ef48d4d386d8d41139b42da0d32_196.png)

![](images/eea4e481a79c.png)

![](images/061b725a6bed.png)

---

![](images/65852c65f51c.png)

![](images/95b075bd0857.png)

![](images/9ade65b1ea59.png)

![](images/aaf9ffe1c999.png)

## 编写训练循环 🔄

![](images/32dde98451f8.png)

![](images/b98c41981e2d.png)

![](images/59ee4ce881c6.png)

![](images/715d87dcf08c.png)

完成上述准备后，就进入了训练循环。最外层的循环代表训练的总轮数，这里设为 3000 轮。

![](images/6b7c0b0f68ad.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/5b458ef48d4d386d8d41139b42da0d32_220.png)

![](images/e89a4004ff20.png)

![](images/1c0455f21d5a.png)

循环内部的操作流程如下：

1. 将输入数据传入模型，得到预测值。
2. 使用损失函数计算预测值与真实标签之间的损失。
3. 调用 `loss.backward()` 计算梯度。
4. 调用 `optimizer.step()` 更新模型参数。
5. 每次更新后，记得使用 `optimizer.zero_grad()` 将梯度重置为零。

![](images/435057051c73.png)

![](images/8e865fdf6457.png)

![](images/171c3957d43b.png)

![](images/9af7634907ed.png)

> 重要提示：本教程的代码是从作业一的示例代码简化修改而来。若要完成作业，请务必以作业一的原始示例代码为准。

![](images/b8b6592aa0a3.png)

![](images/204b1f9b9b7a.png)

![](images/e82c29ea50fb.png)

![](images/fce938b7882f.png)

---

![](images/d6f8248608d2.png)

![](images/86212cb56453.png)

![](images/4aa898202cfa.png)

![](images/56966fd28b04.png)

![](images/a484f6547264.png)

## 如何查阅 PyTorch 官方文档 📖

![](images/a78f8c269425.png)

![](images/17c972c3134b.png)

![](images/aeae1b669b0d.png)

![](images/a5fd2289f17d.png)

如果对 PyTorch 的使用有疑问，最直接的方法是查阅官方文档。

![](images/5f14df85b860.png)

![](images/cb262e2a2503.png)

![](images/3a29da1be2f7.png)

![](images/7fa00556d79d.png)

- `torch.nn`：包含神经网络层相关的说明。
- `torch.optim`：包含优化算法的说明。
- `torch.utils.data`：包含 `Dataset` 和 `DataLoader` 相关的说明。

![](images/a0a937c7c77c.png)

![](images/13d7d873d662.png)

![](images/f4b3ff1dda6a.png)

![](images/3587f3c979bf.png)

![](images/19ba61ebd8e3.png)

在阅读文档时，每个函数通常包含以下要素：

1. 函数的输入和输出说明。
2. 对每个输入参数的详细解释，包括其作用和数据类型。

![](images/8493e83279f0.png)

![](images/fd123373725f.png)

![](images/21abedf91bc7.png)

![](images/44256e5fbd68.png)

PyTorch 中，有些函数有不同的变体。文档中会用 `*` 星号分隔两种参数：

- `*` 之前的参数是位置参数，传递时无需指定参数名。
- `*` 之后的参数是关键字参数，传递时必须指定参数名。  
  
  有些参数有默认值，这类参数是可选的。

![](images/3d632b4ba165.png)

![](images/a5875ccdbe3f.png)

![](images/9487203af9ed.png)

![](images/dc0a73099355.png)

---

![](images/8cc557fc422d.png)

![](images/9e9da1d0bb20.png)

![](images/35b301104854.png)

![](images/a25a2f6c7060.png)

![](images/d54fbe9dc437.png)

## 常见 PyTorch 错误及解决方法 ⚠️

![](images/dce449fcbdc1.png)

![](images/31d27a548203.png)

![](images/eb418c5dbb8c.png)

![](images/cef43ef5625e.png)

以下是一些常见的 PyTorch 错误及其解决方法：

![](images/c9a07a7b8a70.png)

![](images/d7a0548bb239.png)

![](images/1eec003cbfd3.png)

![](images/81de4d575334.png)

![](images/c198391e0aaa.png)

1. **模型与张量设备不匹配**
  
  **问题**：模型在一个设备上，而输入张量在另一个设备上。
  **解决**：将张量移动到模型所在的设备。例如：`x = x.to(‘cuda’)`。

![](images/033fdd7f5326.png)

![](images/f9ba283f24ae.png)

![](images/9cb07de7aeed.png)

![](images/a0dec49b44e6.png)

1. **张量形状不匹配**
  
  **问题**：进行运算的张量形状不一致。
  **解决**：使用 `view()`、`reshape()` 或 `transpose()` 等函数调整张量形状。

![](images/6f11a726f9d4.png)

![](images/7b32fda22cc1.png)

![](images/5f50ce7529e6.png)

1. **CUDA 内存不足**
  
  **问题**：显卡显存耗尽。
  **解决**：减小 `DataLoader` 中的 `batch_size` 参数。

![](images/e45db74e72be.png)

![](images/374313d81e67.png)

![](images/b711a7aba985.png)

![](images/d977c4de2e75.png)

1. **张量数据类型不匹配**
  
  **问题**：运算或函数要求特定数据类型的张量。
  **解决**：转换张量的数据类型。例如，对于需要 `long` 类型标签的交叉熵损失，使用 `y = y.long()` 进行转换。

![](images/ba7f1a24e8ec.png)

![](images/3810c58bc882.png)

![](images/0d51545b03b7.png)

![](images/747c32043bc2.png)

![](images/06099d5aa0f3.png)

---

![](images/48e58557648b.png)

![](images/e07aa4a54499.png)

![](images/ae5029be576d.png)

## 总结 📝

![](images/d62c4e6a85ec.png)

![](images/d6de604d2004.png)

![](images/668dd13ec9a9.png)

![](images/b552a0d46e17.png)

![](images/f17772a425e2.png)

本节课我们一起学习了使用 PyTorch 解决一个回归问题的完整流程。我们从数据加载与预处理开始，构建了自定义的 `Dataset` 和 `DataLoader`，定义了一个简单的神经网络模型，并设置了损失函数与优化器。最后，我们编写了训练循环来迭代优化模型。此外，我们还介绍了如何有效查阅 PyTorch 官方文档以及诊断解决几种常见的编程错误。希望这些内容能帮助你更顺利地进行 PyTorch 实践。
