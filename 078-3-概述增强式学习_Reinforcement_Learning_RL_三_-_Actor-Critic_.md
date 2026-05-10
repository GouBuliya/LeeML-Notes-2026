# 78：3-概述增强式学习 (Reinforcement Learning, RL) (三) - Actor-Critic 🤖

![](images/45d1d905d901.png)

![](images/74303cf52b53.png)

![](images/40b8ebf05639.png)

![](images/91a2796da6af.png)

![](images/59bd1734c96a.png)

![](images/538d2815253b.png)

![](images/42bc830084af.png)

在本节课中，我们将要学习增强式学习（RL）中的一个核心概念：Actor-Critic。我们将首先了解什么是“评论家”（Critic），然后探讨它如何帮助“演员”（Actor）进行学习，最终介绍一个结合两者的强大算法——优势演员-评论家（Advantage Actor-Critic）。

![](images/cbe331219407.png)

![](images/7c0e38fa6dbb.png)

![](images/f43b7e548e05.png)

---

![](images/2bad7e6f0973.png)

![](images/2ca76cbf4662.png)

![](images/21e528dc9cd6.png)

## 什么是评论家（Critic）？🎯

![](images/ec3fdb4d07f7.png)

![](images/878a60ddef13.png)

![](images/d9e78071efa2.png)

![](images/da5cbf0284b1.png)

![](images/e72656d663ef.png)

![](images/0953b00c697e.png)

上一节我们介绍了如何训练一个演员（Actor）。本节中，我们来看看评论家（Critic）是什么。

![](images/3a5dd0d18469.png)

![](images/fa0026d46d4a.png)

![](images/fd2a037dbb5b.png)

![](images/037d93677c97.png)

![](images/8208ab8de10b.png)

![](images/f74b264e436c.png)

评论家的工作是评估一个演员的好坏。具体来说，给定一个演员（其参数为 θ），评论家需要评估：当这个演员看到某个特定的游戏画面（Observation）时，它接下来可能获得多少累积奖励（Reward）。

![](images/024beeaa9802.png)

![](images/cb68254fa20c.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_54.png)

![](images/dde129b85794.png)

![](images/9005b4773203.png)

![](images/71364cc383f8.png)

![](images/c86d41398487.png)

评论家有多种形式。有的只看游戏画面来预测奖励，有的则同时考虑游戏画面和演员采取的动作。为了更具体，我们介绍一种在作业中会实际用到的评论家：**价值函数（Value Function）**。

![](images/8afde25a98a7.png)

![](images/f1b4bc9b3400.png)

![](images/711805203c84.png)

![](images/7467cd524b20.png)

![](images/cb574f4d5e9f.png)

![](images/0090755dab4f.png)

我们使用 **`V^θ(s)`** 来表示价值函数。它的输入是状态 `s`（例如游戏画面），输出是一个标量数值。这个数值的含义是：对于参数为 θ 的演员，当它看到状态 `s` 时，**预期**能获得的**折现累积奖励（Discounted Cumulative Reward）**。

![](images/fdde62ef9692.png)

![](images/ea7cbd0e5379.png)

![](images/36882eb8754e.png)

![](images/92454dff4d7f.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_84.png)

![](images/911585b32abf.png)

还记得折现累积奖励 `G_t^π` 吗？它表示从时间 `t` 开始，未来所有奖励的加权和，距离越远的奖励权重越小（乘以折扣因子 γ）。公式如下：

![](images/3b583178e46b.png)

![](images/3e04023af1a6.png)

![](images/2b4196106096.png)

![](images/e3e71f974a9c.png)

![](images/2e8cc2553a60.png)

![](images/acc4cd5a5901.png)

`G_t^π = R_t + γ * R_{t+1} + γ^2 * R_{t+2} + ...`

![](images/6a229a44f4f0.png)

![](images/ca4f689765e3.png)

![](images/a94a1278451c.png)

![](images/1ab85952e62b.png)

![](images/e4a3302634a9.png)

![](images/6610cc9d4102.png)

价值函数 `V^θ(s)` 的目标就是预测这个 `G_t^π` 的期望值。它需要在游戏还没结束、只看到当前画面 `s` 时，就“未卜先知”地预测演员的最终表现。

![](images/9f482859af71.png)

![](images/407b73b6638c.png)

![](images/f81f2c513188.png)

![](images/682825493ada.png)

![](images/048772ddd41a.png)

**需要特别注意**：价值函数的上标 θ 至关重要。它意味着这个价值函数是针对**特定演员 θ** 进行评估的。同样的游戏画面，不同的演员（例如一个高手和一个新手）会得到不同的价值评估。

![](images/e4b06913519f.png)

![](images/3b5370ab0eb2.png)

![](images/2e1b972f0420.png)

![](images/b23580077905.png)

![](images/45655c98558c.png)

---

![](images/428e355f7633.png)

![](images/6c4f8983d422.png)

![](images/a4a1349ae26d.png)

![](images/2ac07dc89841.png)

![](images/0f017425b31e.png)

![](images/e8d497b69e39.png)

## 如何训练评论家？🔧

![](images/79edcf451477.png)

![](images/e957b70ce7f7.png)

![](images/e072348d7f09.png)

![](images/f18be66a906d.png)

![](images/f707d3905436.png)

![](images/e9fd0b94cb37.png)

了解了评论家的作用后，我们来看看如何训练它。有两种常用的方法。

![](images/c383be541461.png)

![](images/8d9c82ddf114.png)

![](images/25e55ddd7872.png)

![](images/4e7d3ce5bd04.png)

![](images/257522f33570.png)

![](images/8c2437947c4a.png)

### 方法一：蒙特卡洛方法（Monte-Carlo, MC）

![](images/75ce5dc59968.png)

![](images/5e119eef6da3.png)

这种方法非常直观。以下是训练步骤：

![](images/93ed4f811e01.png)

![](images/507d2db2f6fd.png)

![](images/e1ebe2f23f7a.png)

![](images/ba4c91792284.png)

![](images/335740da907c.png)

![](images/b8d8c53033a8.png)

1. 让演员 θ 与环境进行多次互动，玩很多轮游戏，收集游戏记录。
2. 对于记录中出现的每一个状态 `s_t`，因为游戏已经玩完，我们可以直接计算出从 `s_t` 开始实际获得的折现累积奖励 `G_t^π`。
3. 这样，我们就得到了训练数据对：`(s_t, G_t^π)`。
4. 训练价值函数网络 `V^θ(s)`，使其在输入 `s_t` 时，输出值尽可能接近真实的 `G_t^π`。

![](images/dc060fab9874.png)

![](images/ffded7a6aac6.png)

![](images/cd3ac9a844fc.png)

![](images/0210ae4eaaa1.png)

![](images/f405f304dd0f.png)

**简单来说**：MC方法通过玩完整个游戏来获得每个状态的“标准答案”（`G_t^π`），然后用这个答案来监督训练价值函数。

![](images/195b7cf1f274.png)

![](images/27d1c08526cb.png)

![](images/dcdf5da7ef86.png)

![](images/d1281aad9050.png)

![](images/cd3c8963ee31.png)

![](images/aedfc702e078.png)

### 方法二：时序差分方法（Temporal-Difference, TD）

![](images/7d9e547424f7.png)

![](images/586bf9e55bc1.png)

![](images/a7ed695cc4d0.png)

![](images/b564d5fee420.png)

![](images/db8698ccf9ea.png)

TD方法不需要玩完整场游戏就能获得训练数据。它只需要一个短序列：`(s_t, a_t, r_t, s_{t+1})`，即“在状态 `s_t` 采取动作 `a_t`，获得奖励 `r_t`，并转移到新状态 `s_{t+1}`”。

![](images/a9437e0a44d4.png)

![](images/387fb3e8feeb.png)

![](images/b2217c4bfd43.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_222.png)

![](images/435237ac44be.png)

它的核心思想基于价值函数之间的一个关系。我们知道：  

`V^θ(s_t) = r_t + γ * V^θ(s_{t+1})` （在期望意义上成立）。

![](images/acec1c03497f.png)

![](images/d539f3d8ddae.png)

![](images/7147101b4250.png)

![](images/e5b91b7074fa.png)

![](images/66f77db23e0e.png)

![](images/e34592de3e95.png)

![](images/f89ae3e603c7.png)

因此，我们可以构造这样的训练目标：让 `V^θ(s_t)` 和 `γ * V^θ(s_{t+1}) + r_t` 尽可能接近。也就是说，我们希望 `V^θ(s_t) - γ * V^θ(s_{t+1})` 的值接近我们观察到的即时奖励 `r_t`。

![](images/79d57a29682b.png)

![](images/0835ab7910d2.png)

![](images/4ae2bcc8424e.png)

![](images/54372047d879.png)

**TD方法的优势**：对于很长的或永不结束的游戏，MC方法需要等到回合结束才能更新，效率低下。TD方法可以每一步都进行更新，更灵活、更高效。

![](images/b40bf2740f76.png)

![](images/2bbd2fbfe5e4.png)

![](images/15441311b6b7.png)

![](images/fea044b6eafb.png)

**MC与TD的差异**：这两种方法基于不同的假设，对同一组数据估算出的价值函数可能不同。MC严格依赖实际观测到的完整回报，而TD则假设相邻状态的价值满足某种一致性关系。

![](images/e9eb6ab85f90.png)

![](images/08e5baef8877.png)

![](images/5de42902ba2b.png)

![](images/b88b22221bfa.png)

![](images/8e776eb72727.png)

![](images/1a4a0af3e3e3.png)

![](images/aff83a822b61.png)

---

![](images/743191cda0e0.png)

![](images/152eedf0f55e.png)

## 用评论家来帮助演员：优势演员-评论家（Advantage Actor-Critic）🚀

![](images/e7d88e253c84.png)

![](images/3807faddc590.png)

![](images/13c06d466121.png)

![](images/77a6804a16e6.png)

![](images/4bf8b5eed83f.png)

![](images/fcff40eb57ec.png)

现在，我们进入核心部分：如何利用训练好的评论家来更有效地训练演员？

![](images/641ebf8f7726.png)

![](images/122c4194b97b.png)

![](images/4a3cc396451b.png)

![](images/3b09707ed7fb.png)

回顾上一讲，我们训练演员时，需要为每一个“状态-动作对” `(s_t, a_t)` 分配一个分数 `A_t`，用以判断这个动作的好坏。我们当时使用 `G_t^π`（实际获得的累积奖励）减去一个基线（Baseline）`b` 来计算 `A_t`，即 `A_t = G_t^π - b`。但基线 `b` 设为多少合适呢？

![](images/082bc5b6158c.png)

![](images/15bb83a25ead.png)

![](images/9f36fe7a8631.png)

![](images/023077cd13f5.png)

![](images/bef808555e9d.png)

一个合理的选择是将基线 `b` 设为价值函数 `V^θ(s_t)`。即：  

`A_t = G_t^π - V^θ(s_t)`

![](images/5f379b9b9c58.png)

![](images/dee05cafa963.png)

![](images/42c0c249694a.png)

![](images/e2e7617ab454.png)

![](images/537b203b12f2.png)

![](images/8274403977f7.png)

**为什么这是合理的？**

- `V^θ(s_t)` 代表在状态 `s_t` 下，演员**平均**能获得的累积奖励期望值。
- `G_t^π` 是在状态 `s_t` 下**实际执行动作 `a_t`** 后获得的累积奖励（一次采样结果）。
- 如果 `A_t > 0`，说明执行 `a_t` 的效果**好于**平均水平，应该鼓励。
- 如果 `A_t < 0`，说明执行 `a_t` 的效果**差于**平均水平，应该抑制。

![](images/3af609baddae.png)

![](images/cbc31dfb385a.png)

![](images/7ae83984b556.png)

![](images/23e439107e48.png)

![](images/9be8e592855f.png)

![](images/accaf5cc90a7.png)

![](images/b7662533ceae.png)

然而，`G_t^π` 是一个具体的采样结果，可能波动很大（运气好或差）。一个更稳定的做法是使用期望值来代替这次采样。这就是 **优势函数（Advantage Function）** 的思想。

![](images/18cbca4eed61.png)

![](images/4035fe965996.png)

![](images/1cf3dbb91bc2.png)

![](images/02574eebe323.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_337.png)

![](images/7b330943bfec.png)

我们利用评论家来做这件事。在状态 `s_t` 执行 `a_t` 后，我们得到奖励 `r_t` 并进入 `s_{t+1}`。那么，执行 `a_t` 的期望回报可以近似为 `r_t + γ * V^θ(s_{t+1})`。将其与平均表现 `V^θ(s_t)` 对比，就得到了优势函数估计：

![](images/44c60acfd45a.png)

![](images/4f2918faa027.png)

![](images/8c8e4d9086db.png)

![](images/d0271fb8530c.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_349.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_351.png)

`A_t = (r_t + γ * V^θ(s_{t+1})) - V^θ(s_t)`

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_353.png)

![](images/af9e1340618c.png)

![](images/8270671b4874.png)

![](images/afa4645d9849.png)

![](images/ad4c4b581d8e.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_363.png)

这个 `A_t` 就是“优势演员-评论家”算法中用来评估动作好坏的核心指标。它衡量了特定动作 `a_t` 相对于平均水平的优势（或劣势）。

![](images/4466eeb7fd6b.png)

![](images/2f5c5b4e4433.png)

![](images/27acd48c71da.png)

![](images/4e2c229eac6f.png)

![](images/8bbf75d4ac71.png)

---

![](images/3448b28374ee.png)

![](images/f4c80e30fd63.png)

![](images/d393187d61d6.png)

![](images/bb9c55f46061.png)

![](images/c458acddce5f.png)

![](images/dd163634ff27.png)

![](images/018ac777b38a.png)

## 实战技巧：网络参数共享 💡

![](images/d249daf4e942.png)

![](images/6243d653b6d3.png)

![](images/a07003e2d000.png)

![](images/aa8b025a8ef5.png)

![](images/164f42207900.png)

![](images/396534c7c768.png)

在实现Actor-Critic时，有一个实用的技巧。演员网络和评论家网络通常接收相同的输入（如游戏画面）。它们的前几层（例如用于理解图像特征的卷积层）很可能学习到相似的特征。

![](images/b4f4b0c6cc94.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_403.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_405.png)

![](images/0a64d0b02204.png)

![](images/fe7279da7b86.png)

![](images/496646a5fda7.png)

因此，我们可以让演员和评论家**共享前几层的网络参数**，只在最后的分支层分别输出动作概率分布（Actor）和状态价值标量（Critic）。这样做可以减少参数量，提高训练效率，并让特征表示更一致。

![](images/b0a277014b16.png)

![](images/0bd7fa33c395.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/1da2592808784ec8e7b94bc3bc33f72c_417.png)

![](images/195e7be5b0b6.png)

![](images/1902ce91c202.png)

![](images/09f009679f92.png)

---

![](images/d22e817dd4bc.png)

![](images/5c541eef3534.png)

![](images/a6dedeec8225.png)

![](images/cf6af8d401e6.png)

![](images/e9137e40893a.png)

![](images/20e1d80b69d4.png)

## 总结与拓展 📚

![](images/439f71df7c0c.png)

![](images/539cb5d048c0.png)

![](images/da6981252983.png)

![](images/17c35ffdbdc1.png)

![](images/20250577f85e.png)

![](images/8c567e7f195c.png)

![](images/34c4e0bf9e4b.png)

![](images/dd22e4019a38.png)

本节课中我们一起学习了：

1. **评论家（Critic）** 的概念，特别是价值函数 `V^θ(s)`，它用于评估给定演员在某个状态下的预期回报。
2. 训练评论家的两种方法：**蒙特卡洛（MC）** 和**时序差分（TD）**，并理解了它们的区别。
3. 如何利用评论家来优化演员的训练，引入了**优势函数** `A_t`，并最终得到了 **优势演员-评论家（Advantage Actor-Critic）** 算法。
4. 一个实用的实现技巧：让Actor和Critic共享部分网络参数。

![](images/98c9ff772770.png)

![](images/49a222195f10.png)

![](images/916c2d566fc6.png)

![](images/3c40d58a4320.png)

![](images/b4a8aaefd5f2.png)

![](images/14bdad803fdc.png)

增强式学习领域还有很多其他重要方法，例如著名的 **深度Q网络（Deep Q-Network, DQN）** 系列，它直接使用评论家（Q函数）来选取最优动作。DQN有很多改进版本，集大成者可以参考一篇名为 **《Rainbow》** 的论文，它结合了DQN的多种改进技巧。

![](images/a18283f3c2cb.png)

![](images/0334bd361360.png)

![](images/f9462fdfd2ee.png)

![](images/cf62db50c3b6.png)

![](images/f2facd0d63e7.png)

---

![](images/a6233d231843.png)

![](images/71d61de75219.png)

![](images/bd03b110b6b9.png)

![](images/94ce94ff1325.png)

![](images/8d199794d7cf.png)

**本节课中，我们从理解评论家开始，逐步将其与演员的训练相结合，最终构建出更强大、更稳定的Actor-Critic学习框架。**
