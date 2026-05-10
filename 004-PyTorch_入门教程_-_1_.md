# 4：PyTorch 入门教程 - 1 🚀

![](images/4fc0591bfcd9.png)

![](images/325467477bb0.png)

![](images/6961e15156ba.png)

![](images/5a8918a48f47.png)

![](images/dec2c5e0c40e.png)

![](images/6aa306f8c743.png)

![](images/f805c866d08d.png)

![](images/c2b1bd16774a.png)

![](images/02dac325dbf9.png)

在本节课中，我们将学习如何使用 PyTorch 这一强大的机器学习框架。课程将涵盖 PyTorch 的基本概念、如何读取数据、定义神经网络、选择损失函数和优化器，以及完整的训练、验证和测试流程。

![](images/4aaf9895b1af.png)

![](images/c73b600b1d7f.png)

![](images/0ed63f727857.png)

![](images/4b8c90a9d6c4.png)

---

![](images/d7347634dfc8.png)

![](images/435015e6fdc4.png)

![](images/d279e96a4d59.png)

![](images/d4afd0f6de78.png)

![](images/1e8349a2266a.png)

## 什么是 PyTorch？🤔

![](images/e25dd13931bb.png)

![](images/522283dcf8e0.png)

![](images/3857a45e8515.png)

![](images/fa7968abea04.png)

PyTorch 是一个基于 Python 的机器学习框架。它的主要优势在于能够利用 GPU 加速高维矩阵（称为 **张量**）的运算，并内置了自动梯度计算功能，使得神经网络的训练变得非常简便。

![](images/a1a4452d214d.png)

![](images/23f30795aa92.png)

![](images/3da89257bc0a.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_49.png)

![](images/5f66a04b0ec9.png)

## 训练神经网络的三个步骤 📝

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_53.png)

![](images/66757a6b9751.png)

![](images/085907caae87.png)

![](images/7d2fdf3bc565.png)

![](images/8de0baba93ec.png)

上一节我们介绍了 PyTorch 的基本概念，本节中我们来看看训练一个神经网络的通用流程。根据李宏毅老师的课程，训练神经网络通常包含三个核心步骤：

![](images/329582059ab3.png)

![](images/f78ff4b840fb.png)

![](images/e305556d001b.png)

![](images/8f2c9f8059c8.png)

1. **定义神经网络结构**：确定网络包含哪些层以及它们如何连接。
2. **选择损失函数**：定义一个用于衡量模型预测好坏的函数，我们的目标是使其最小化。
3. **选择优化算法**：挑选一个梯度下降算法来优化模型的参数。

![](images/a53d1cad2d07.png)

![](images/7e2c26b9725b.png)

![](images/5995ebdff62a.png)

![](images/2245d44b240c.png)

![](images/4b0677e37630.png)

除了训练，我们还需要进行验证和测试。通常，我们会反复进行训练和验证，最后再进行测试。

![](images/bfd05baf85b1.png)

![](images/fa760aadbb6a.png)

![](images/5b5072713b0b.png)

![](images/3a7503d882f9.png)

![](images/49e85920ab09.png)

## 数据读取：Dataset 与 DataLoader 📂

![](images/5f427e1367ae.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_93.png)

![](images/3cdf95de2126.png)

![](images/92002ab775d7.png)

要训练和测试模型，首先需要将数据读入程序。在 PyTorch 中，这通过 `torch.utils.data.Dataset` 和 `torch.utils.data.DataLoader` 来完成。

![](images/daa4068d41ed.png)

![](images/3d8341d35618.png)

![](images/517f8d20a49b.png)

![](images/0409942cf4fe.png)

![](images/27576125e030.png)

- **`Dataset`**：负责将原始数据逐条读取，并用一个 Python 类进行封装，以便后续调用。它通常包含数据本身以及对应的预期标签。
- **`DataLoader`**：负责将 `Dataset` 中的单条数据合并成一个个的 **批次**，并可以进行并行化处理以加速数据读取。

![](images/d2ddb4c216b2.png)

![](images/8635559d3188.png)

![](images/142f8c367ded.png)

![](images/72d0ec9638aa.png)

以下是创建它们的基本代码框架：

![](images/4fed0dd852d7.png)

![](images/1bc5cc1ef9f8.png)

![](images/5aa475ea3a20.png)

![](images/41b183d4f88e.png)

```python
# 首先，需要自定义一个 Dataset 类
train_dataset = MyDataset(...)
# 然后，使用 DataLoader 进行封装
train_loader = DataLoader(train_dataset, batch_size=batch_size, shuffle=True)
```

![](images/5570c986d754.png)

![](images/803f0b7f7e5b.png)

![](images/d7d5eadc5540.png)

![](images/0850372d08b8.png)

在训练时，我们通常会对数据进行打乱；而在测试时则不需要。

![](images/d753abf5a0f5.png)

![](images/b80742203528.png)

![](images/8d91f70feb80.png)

![](images/80b4bd840722.png)

![](images/e27b0f549739.png)

## 自定义 Dataset 类 🛠️

![](images/4404b9fc8eb0.png)

![](images/3c780693a64e.png)

![](images/34fb1e1300f8.png)

![](images/35e667b678b5.png)

要自定义 Dataset，需要创建一个类并重写三个方法：

![](images/1e344237dd44.png)

![](images/bf9075e60e1a.png)

![](images/e340270980cf.png)

![](images/e0e4cc613852.png)

以下是需要重写的三个核心方法：

1. `__init__`：初始化函数，用于读取数据并进行预处理。
2. `__getitem__`：根据索引返回对应的单条数据（特征和标签）。
3. `__len__`：返回数据的总条数，让 DataLoader 知道如何划分批次。

![](images/f8b2b497074c.png)

![](images/09c06642e418.png)

![](images/b90398f1ecde.png)

![](images/295b3c4fffe1.png)

`DataLoader` 的工作流程是：根据设定的批次大小，多次调用 `Dataset` 的 `__getitem__` 方法获取数据，然后将这些数据合并成一个批次供模型使用。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_167.png)

![](images/1f485a2b905b.png)

![](images/4d3870eeeaf2.png)

![](images/2750a6d47b04.png)

## PyTorch 的核心：张量 🔢

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_175.png)

![](images/78a496eece2b.png)

![](images/4544a51268ef.png)

![](images/6ca0131be4ff.png)

![](images/7538b8b8ac52.png)

PyTorch 处理的数据主要是高维数组，称为 **张量**。例如，声音信号是一维张量，灰度图像是二维张量，彩色图像（RGB）是三维张量。

![](images/046b6abeedc9.png)

![](images/c49a008e6391.png)

![](images/a31a081394b4.png)

![](images/072a12be0b13.png)

![](images/29544b9d4cec.png)

你可以使用 `.shape` 属性查看张量的维度：

![](images/72f7f2f7da0a.png)

![](images/d2a9260a1cb7.png)

![](images/cedc1e053000.png)

![](images/02ee0ab26dfa.png)

```python
x = torch.tensor([[1, 2, 3], [4, 5, 6]])
print(x.shape)  # 输出：torch.Size([2, 3])
```

![](images/a3160078c6a0.png)

![](images/e9c871bdfedf.png)

![](images/a34f420b802b.png)

### 张量的创建与运算

![](images/fb0b6763cad3.png)

![](images/9a1c81aac9f7.png)

![](images/2def9a5bf25a.png)

![](images/c86cd12cfc58.png)

张量可以从列表或 NumPy 数组创建，也可以直接生成特定形状的张量：

![](images/f8cbf3eaeb25.png)

![](images/917ed342bf5a.png)

![](images/658a6168b919.png)

![](images/9b014f0931a9.png)

![](images/f54f91816161.png)

```python
# 从列表创建
data = [[1, 2], [3, 4]]
x = torch.tensor(data)

![](images/8f89f21d4c9e.png)

![](images/104570e51f08.png)

![](images/4da5194d90b9.png)

![](images/ad9652039702.png)

# 创建全零或全一张量
zeros_tensor = torch.zeros(2, 2)  # 2x2 的全零矩阵
ones_tensor = torch.ones(2, 2)    # 2x2 的全一矩阵
```

![](images/37ed933737b4.png)

![](images/f974ec2b4eba.png)

![](images/e8df3bb54094.png)

![](images/331f022b5eca.png)

PyTorch 支持丰富的张量运算，包括基本的加减乘除、求和、平均值，以及更复杂的数学运算如对数、幂运算等。

![](images/95b936190b5d.png)

![](images/3e2592c96648.png)

![](images/827d9df5cacf.png)

![](images/fe64ff554580.png)

![](images/e378b9bbbb46.png)

### 张量的形状操作

![](images/d953facb3ed8.png)

![](images/f8321cb8f018.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_257.png)

![](images/5962511c96de.png)

在模型训练中，经常需要调整张量的形状。以下是一些常用操作：

![](images/6c7b94d9c651.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_263.png)

![](images/2e60429c812d.png)

![](images/d6892bb805a3.png)

![](images/34785cebdb3e.png)

- **转置**：交换两个维度。`x = torch.tensor([[1, 2, 3], [4, 5, 6]]) # shape: [2, 3]
  y = x.transpose(0, 1) # shape: [3, 2]
  `
- **压缩维度**：移除长度为1的维度。`x = torch.ones(1, 2, 3) # shape: [1, 2, 3]
  y = x.squeeze(0)        # shape: [2, 3]
  `
- **扩展维度**：在指定位置增加一个长度为1的维度。`x = torch.ones(2, 3)    # shape: [2, 3]
  y = x.unsqueeze(1)      # shape: [2, 1, 3]
  `
- **拼接张量**：沿指定维度合并多个张量。`x = torch.ones(2, 1, 3)
  y = torch.ones(2, 3, 3)
  z = torch.cat([x, y], dim=1) # shape: [2, 4, 3]
  `

![](images/df965caabce7.png)

![](images/86970bde05cc.png)

![](images/cf47f266bd8c.png)

![](images/6435fb01fe31.png)

![](images/d9c45d1df173.png)

如果遇到不清楚的操作，查阅 PyTorch 官方文档是最佳途径。

![](images/0f96e8d60078.png)

![](images/2278f5d800a7.png)

![](images/d4f8c5042782.png)

![](images/b9f753d6d9d5.png)

## 常见问题与 GPU 加速 ⚡

![](images/de87ffff9e71.png)

![](images/8df5f657f13e.png)

![](images/0e598ae7278f.png)

![](images/51ce12a7e185.png)

![](images/cd61716d58e8.png)

初学者在使用张量时可能会遇到一些问题，例如数据类型不匹配（整数 vs 浮点数）。如果遇到与数据类型相关的错误，请检查并调整张量的 `dtype`。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_298.png)

![](images/266410c29e54.png)

![](images/806fdeeb5d1e.png)

![](images/a63e2d44a7e7.png)

PyTorch 与 NumPy 在 API 设计上非常相似，熟悉 NumPy 的同学会更容易上手。

![](images/614d1e694a10.png)

![](images/499d72e619ae.png)

![](images/6d9fb7964d85.png)

![](images/4ce9e8fa60f0.png)

![](images/f20bba94220f.png)

### 使用 GPU 加速

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_316.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_318.png)

![](images/3a79d8659531.png)

![](images/8178403deba7.png)

PyTorch 的一个关键优势是可以利用 GPU 加速计算。默认情况下，张量在 CPU 上运算。你可以使用 `.to()` 方法将其移动到 GPU：

![](images/ba5e81be567f.png)

![](images/6ff837d70f78.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_328.png)

![](images/86800fd8102f.png)

![](images/017259c43e7a.png)

```python
# 检查 GPU 是否可用
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

![](images/9bee4d3b9c53.png)

![](images/24cb2a537e8f.png)

![](images/52b889437041.png)

![](images/a76930c73d66.png)

![](images/2ddd81500269.png)

# 将张量移动到 GPU
x = torch.tensor([1.0, 2.0])
x = x.to(device)
```

![](images/ae2fc250a791.png)

![](images/fc6780f1589e.png)

![](images/6e0332e8bd6d.png)

![](images/9ae11ab6e93d.png)

GPU 通过其大量的核心并行处理矩阵运算，从而大幅提升训练速度。

![](images/5559419a169a.png)

![](images/7bfc1f498795.png)

![](images/49d374533c36.png)

![](images/59fd2f48f061.png)

![](images/9aa447382efd.png)

## 自动梯度计算 📈

![](images/cfa45be936d5.png)

![](images/e53862024b93.png)

![](images/801146394d61.png)

![](images/0264892f146e.png)

PyTorch 的另一个核心功能是 **自动微分**。它使得计算梯度变得极其简单，这是训练神经网络（通过梯度下降优化参数）的基础。

![](images/8ad224c9e8f5.png)

![](images/fcd0bd635e9c.png)

![](images/86654407c877.png)

![](images/015e4941170d.png)

![](images/2ea39787e403.png)

以下是一个计算梯度的简单示例：

![](images/df4c4585711a.png)

![](images/c3d6fe3ce17d.png)

![](images/f8d752aa8250.png)

![](images/533424f79598.png)

![](images/577d6f4ff5c0.png)

```python
# 1. 创建张量，并启用梯度追踪
x = torch.tensor([[1., 0.], [-1., 1.]], requires_grad=True)

![](images/f9e8eb6fa04e.png)

![](images/c3fe0ed09ae3.png)

![](images/25bbbcdb081c.png)

![](images/d7f4ea76e779.png)

# 2. 定义计算过程（例如计算平方和）
z = x.pow(2).sum()  # z = x[0,0]^2 + x[0,1]^2 + x[1,0]^2 + x[1,1]^2

![](images/0266485b804a.png)

![](images/e92c30add01c.png)

![](images/6be05137c9f4.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_404.png)

# 3. 反向传播，计算梯度
z.backward()

![](images/d66a606ab409.png)

![](images/ea37e4a8111c.png)

![](images/8601e51d359d.png)

![](images/12a6bb10c45a.png)

![](images/281e4e00cf1f.png)

# 4. 查看 x 的梯度
print(x.grad)
# 输出：tensor([[ 2.,  0.], [-2.,  2.]])
# 解释：对 x[i,j] 求导结果是 2*x[i,j]
```

![](images/6c69cf8277ed.png)

![](images/0aec3bfb31be.png)

![](images/4a6dcc541aab.png)

![](images/028f568b470f.png)

![](images/8c8cb1c773d1.png)

## 定义神经网络 🧠

![](images/08b88eaeb8b9.png)

![](images/3864aba53199.png)

![](images/884f7b13de7d.png)

![](images/3a1137ac1c47.png)

现在，我们来看看如何用 PyTorch 定义神经网络。这需要用到 `torch.nn` 模块。

![](images/3f0e8459d6da.png)

![](images/7856001afab3.png)

![](images/4592ab90869b.png)

![](images/0ad27b97ad93.png)

![](images/5f9e309df205.png)

一个最基本的层是线性层（`nn.Linear`，也称为全连接层）。它执行的操作是 `y = xA^T + b`，即矩阵乘法加上偏置。

![](images/da090258f1d0.png)

![](images/930057b1a4c1.png)

![](images/b30b42086055.png)

![](images/b414ba27f70e.png)

```python
import torch.nn as nn

![](images/e5c72dcfdaf2.png)

![](images/369f69c5a38c.png)

![](images/c826ca38856a.png)

![](images/4a9d69359929.png)

![](images/a34f3668eb4f.png)

# 定义一个输入特征为32，输出特征为64的线性层
linear_layer = nn.Linear(32, 64)
```

![](images/7ea3a84eb6bc.png)

![](images/c6eb5c33a07e.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_465.png)

![](images/32811e9b1fd3.png)

![](images/eb93eb394e29.png)

除了线性层，神经网络还需要非线性激活函数（如 Sigmoid, ReLU）来引入复杂性。PyTorch 也提供了这些函数。

![](images/4d4c1128d0c6.png)

![](images/4c0994aa3ad3.png)

![](images/b1bf8b63bbca.png)

### 构建自定义模型

![](images/9b341c7992a8.png)

![](images/b6416fa24ffd.png)

![](images/ff9984bd8d40.png)

![](images/de0a455a81c6.png)

![](images/f7bf04a1af67.png)

要构建自己的模型，需要创建一个继承自 `nn.Module` 的类，并重写 `__init__` 和 `forward` 方法。

![](images/3baf84a75c63.png)

![](images/1883f30b0095.png)

![](images/207460dc3771.png)

![](images/3726b1441cba.png)

```python
class MyModel(nn.Module):
    def __init__(self):
        super(MyModel, self).__init__()
        # 定义网络层
        self.net = nn.Sequential(
            nn.Linear(10, 32),
            nn.Sigmoid(),
            nn.Linear(32, 1)
        )

    def forward(self, x):
        # 定义前向传播过程
        return self.net(x)
```

![](images/56085b681211.png)

![](images/c7be0d7d6cd4.png)

![](images/6769e65c68ec.png)

![](images/976ce86cdbe1.png)

![](images/c55e72d2e8d9.png)

`nn.Sequential` 是一个容器，可以按顺序组合多个层。你也可以不使用它，而显式地在 `forward` 中逐层调用，两种方式是等价的。

![](images/8d795fce5f2f.png)

![](images/3c84fa71ece1.png)

![](images/1f0a44de41d7.png)

![](images/8e51cbbd604a.png)

## 损失函数与优化器 ⚖️

![](images/b281f55c1e28.png)

![](images/12683d98b58f.png)

![](images/1359ece6968b.png)

![](images/44cf2c9f3587.png)

![](images/3681ad6b6607.png)

定义好模型后，我们需要定义要最小化的目标，即 **损失函数**。PyTorch 在 `torch.nn` 中提供了许多常见的损失函数，例如均方误差（MSE）用于回归任务，交叉熵损失用于分类任务。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_523.png)

![](images/ccef2b4c0bd3.png)

![](images/3ec9cc7667a7.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_529.png)

![](images/ca1ae9c47297.png)

```python
criterion = nn.MSELoss()  # 均方误差损失
```

![](images/a9a8644569fc.png)

![](images/6efd3c1bd963.png)

![](images/7dc831025cef.png)

![](images/68a61f98267f.png)

接下来，需要选择一个 **优化器** 来根据计算出的梯度更新模型参数。PyTorch 在 `torch.optim` 中实现了多种优化算法，如最基础的随机梯度下降（SGD）。

![](images/cf462fd5213e.png)

![](images/09fa54c632c4.png)

![](images/80f14ff7cd9b.png)

![](images/af3b69150b7b.png)

![](images/e4c01655007f.png)

```python
import torch.optim as optim

![](images/ebc6b7b186b5.png)

![](images/bfdc11b69550.png)

![](images/e12e4aa7240f.png)

![](images/9fb4f3e4c57b.png)

![](images/371acbb12496.png)

model = MyModel()
optimizer = optim.SGD(model.parameters(), lr=0.01)  # 学习率设为0.01
```

![](images/09fb3ba75f15.png)

![](images/1debd4f3b8fe.png)

![](images/80ffaf4d057f.png)

![](images/3660d19c63d9.png)

在每次训练迭代中，使用优化器有三个标准步骤：

1. `optimizer.zero_grad()`：将上一轮计算的梯度清零。
2. `loss.backward()`：反向传播，计算当前梯度。
3. `optimizer.step()`：根据梯度更新模型参数。

![](images/6b3ed1a7f46f.png)

![](images/4cadc3261bd2.png)

![](images/9b0506d79756.png)

![](images/e586834b72a3.png)

![](images/2fc3cce8b2e0.png)

## 完整的训练与评估流程 🔄

![](images/02dfd7713635.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_581.png)

![](images/28c1429ac27f.png)

![](images/76ec37e8b89b.png)

现在，我们将之前的所有部分组合起来，形成一个完整的训练、验证和测试流程。

![](images/1de1c60416d1.png)

![](images/8a217c0f0543.png)

![](images/495ace913a01.png)

![](images/08dacf63486b.png)

![](images/ae8b7428934d.png)

### 训练流程

![](images/00c113105444.png)

![](images/78f64126ba03.png)

![](images/cf7d9778c6b2.png)

![](images/138d17df0ee5.png)

```python
# 0. 准备工作
model = MyModel().to(device)
criterion = nn.MSELoss()
optimizer = optim.SGD(model.parameters(), lr=0.01)

![](images/96fffb73eed0.png)

![](images/7c754e96c6c6.png)

![](images/4e85cee5e6de.png)

![](images/89568b2afabd.png)

![](images/f61f11d48044.png)

# 1. 设置模型为训练模式
model.train()

![](images/2dce20dee5a7.png)

![](images/1ca96d1ac2c1.png)

![](images/5538acc89341.png)

![](images/1a71afaff0f4.png)

![](images/e92b6b580463.png)

for epoch in range(num_epochs):
    for batch_x, batch_y in train_loader:  # 从 DataLoader 中取批次数据
        # 2. 梯度清零
        optimizer.zero_grad()
        # 3. 数据移至设备
        batch_x, batch_y = batch_x.to(device), batch_y.to(device)
        # 4. 前向传播
        pred = model(batch_x)
        # 5. 计算损失
        loss = criterion(pred, batch_y)
        # 6. 反向传播
        loss.backward()
        # 7. 更新参数
        optimizer.step()
```

![](images/8b8612672ef9.png)

![](images/ac2613b07960.png)

![](images/b5e5f46bb9b8.png)

![](images/e206ab8b17fc.png)

### 验证/测试流程

![](images/1b603f3cd818.png)

![](images/db201a59d01a.png)

![](images/18eb701af094.png)

![](images/445c1d7c79b7.png)

![](images/0b118114a8bb.png)

验证和测试流程与训练类似，但有三个关键区别：

1. 调用 `model.eval()` 将模型设置为评估模式。这对于某些层（如 Dropout, BatchNorm）的行为至关重要。
2. 使用 `with torch.no_grad():` 上下文管理器关闭梯度计算。这能提升速度并防止测试数据意外影响模型参数。
3. 测试时，数据集中可能没有标签，我们只获取模型预测结果。

![](images/3aded476b310.png)

![](images/f695ca50ec72.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_647.png)

![](images/0b0275d562a8.png)

```python
model.eval()  # 设置为评估模式
total_loss = 0

![](images/83e30a1e6775.png)

![](images/461cd8755212.png)

![](images/9d3a7651bd2a.png)

![](images/50b49919447f.png)

![](images/9db218a26510.png)

with torch.no_grad():  # 关闭梯度计算
    for batch_x, batch_y in valid_loader:
        batch_x, batch_y = batch_x.to(device), batch_y.to(device)
        pred = model(batch_x)
        loss = criterion(pred, batch_y)
        total_loss += loss.item()

![](images/94e2a771c5f3.png)

![](images/732e55cc2c2d.png)

![](images/2f34b86a71cc.png)

![](images/52e6cff5835a.png)

avg_loss = total_loss / len(valid_loader)
```

![](images/98e16fd94ec8.png)

![](images/bfce5af00e95.png)

![](images/66b9c090ad7b.png)

![](images/331b47c445f4.png)

![](images/2fe622eb5607.png)

## 模型的保存与加载 💾

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/c1be6461eafac84e9f3cb450f666a5f2_679.png)

![](images/a137ebe91285.png)

![](images/f3874604f710.png)

![](images/a3fd437a8bcc.png)

![](images/aaf0e4ece60d.png)

训练好的模型需要保存以备后用。PyTorch 提供了简单的保存和加载方法。

![](images/2631ed9095a9.png)

![](images/c158e05509ec.png)

![](images/2f545b43f46d.png)

![](images/c1af35caed9e.png)

```python
# 保存模型
torch.save(model.state_dict(), 'model_weights.pth')

![](images/9a0c60a40da6.png)

![](images/be4559940ad7.png)

![](images/e917598b99e5.png)

![](images/6590569230ba.png)

![](images/edbe2fcfe198.png)

# 加载模型
model = MyModel()
model.load_state_dict(torch.load('model_weights.pth'))
model.eval()
```

![](images/b8707eef73e8.png)

![](images/d27ac3a1d930.png)

![](images/43e45a17605d.png)

![](images/3c8e871313a7.png)

## 总结 🎉

![](images/2d62e4882dbf.png)

![](images/d997efb482e0.png)

![](images/7a8e0dd1b644.png)

![](images/1c3551424bc3.png)

本节课中我们一起学习了 PyTorch 的基础知识。我们了解了：

- PyTorch 是什么及其核心优势（GPU加速、自动梯度）。
- 如何使用 `Dataset` 和 `DataLoader` 读取数据。
- **张量** 的概念、创建及基本操作。
- 如何利用 `nn.Module` 定义神经网络模型。
- 如何选择损失函数和优化器。
- 如何组织完整的模型训练、验证和测试代码。
- 如何保存和加载训练好的模型。

![](images/330b535294b7.png)

![](images/2c4002a8027d.png)

![](images/b7a56bf05896.png)

![](images/227cd4adb098.png)

![](images/74a355b44de4.png)

PyTorch 是目前最流行的深度学习框架之一，广泛应用于学术界和工业界。掌握它将为你后续的机器学习学习和研究打下坚实的基础。如果在学习过程中遇到问题，除了参考教程，查阅 [PyTorch 官方文档](https://pytorch.org/docs/stable/index.html) 总是最有效的解决方法。
