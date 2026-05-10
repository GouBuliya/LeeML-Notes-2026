# 77：增强式学习（二）—— Policy Gradient 详解 🎯

在本节课中，我们将深入学习增强式学习（Reinforcement Learning, RL）的核心算法之一——Policy Gradient。我们将探讨如何评估一个行为（action）的好坏，并理解如何通过策略梯度方法来训练智能体（actor）。

![](images/0a5425d5aeee.png)

---

![](images/98cb264ab6db.png)

## 核心概念：如何评估行为的好坏？

上一节我们介绍了增强式学习的基本框架。本节中，我们来看看如何具体评估一个行为（action）的好坏，这是训练智能体的关键。

### 版本0：即时奖励（不推荐）

![](images/4ae715b83dc7.png)

最简单的评估方法是直接使用执行某个行为后获得的即时奖励（reward）作为该行为的评分。

- **公式**：`A_t = r_t`
- **含义**：在状态 `s_t` 执行行为 `a_t` 后，立即获得奖励 `r_t`。`r_t` 为正则鼓励该行为，为负则惩罚。
- **问题**：这种方法使智能体变得“短视近利”。它只关注眼前利益，无法进行长远规划。例如，在游戏中，移动瞄准本身没有即时奖励，但却是后续射击命中的重要前提，这种方法会忽略这类关键行为。

![](images/ba2506aaa99e.png)

### 版本1：累积奖励

为了解决短视问题，我们使用从当前时刻开始，未来所有奖励的总和来评估当前行为。

- **公式**：`G_t = r_t + r_{t+1} + ... + r_T`
- **含义**：`G_t` 代表从时间 `t` 到回合结束的累积奖励。我们用 `G_t` 来评估在 `s_t` 执行 `a_t` 的好坏。
- **改进**：这样，即使某个行为（如移动瞄准）没有即时奖励，只要它有助于后续获得奖励（如射击命中），该行为也会得到正面评价。

### 版本2：折扣累积奖励

版本1假设未来所有奖励都与当前行为同等相关，这并不合理。距离当前越远的奖励，其不确定性越高，与当前行为的关联性也越低。因此，我们引入折扣因子（discount factor）。

- **公式**：`G'_t = r_t + γ * r_{t+1} + γ^2 * r_{t+2} + ... + γ^{T-t} * r_T`
- **含义**：`γ` 是一个介于0和1之间的折扣因子（如0.9）。未来的奖励会乘以 `γ` 的指数次幂，距离越远，权重越小。
- **代码表示**：`def calculate_discounted_return(rewards, gamma=0.99):
      G = 0
      discounted_returns = []
      for r in reversed(rewards): # 从后往前计算
          G = r + gamma * G
          discounted_returns.insert(0, G) # 插入列表开头
      return discounted_returns
  `
- **优点**：更符合实际，强调近期奖励，弱化远期奖励的影响。

### 版本3：标准化优势

好与坏是相对的。如果游戏中的所有奖励都是正数，那么所有行为都会得到正面的评价，无法区分真正的好坏。因此，我们需要对评分进行标准化。

- **方法**：引入基线（baseline）。常见的做法是从折扣累积奖励中减去一个基线值 `b`。
- **公式**：`A_t = G'_t - b`
- **含义**：`b` 可以是一个常数，也可以是所有 `G'_t` 的平均值等。目标是让评分 `A_t` 有正有负，高于平均的行为得到正分（鼓励），低于平均的行为得到负分（抑制）。
- **目的**：使评分具有相对性，能更有效地指导策略更新。

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/06706b9bea82618347ca796d7f31ce42_9.png)

---

## Policy Gradient 算法流程 🔄

![](images/f9689ae2038a.png)

理解了如何评估行为后，我们来看Policy Gradient算法的具体操作步骤。

以下是算法的核心循环：

![](images/7ff3ccd46cac.png)

![](images/eb2ed6fda8a7.png)

![](images/a28a3c8f134b.png)

1. **初始化**：随机初始化智能体的参数 `θ^0`。
2. **循环训练**（对于 `i = 1` 到 `N`）：
  
  **交互与收集数据**：使用当前智能体 `θ^{i-1}` 与环境互动，收集一个回合（episode）的数据，包括状态序列 `{s_1, s_2, ...}` 和对应的行为序列 `{a_1, a_2, ...}`。
  **评估行为**：使用上述方法（如版本2或3）计算每个行为 `a_t` 的优势分数 `A_t`。
  **定义损失函数**：损失函数旨在最大化智能体采取高评分行为的概率。对于收集到的一批数据，损失函数通常定义为：`L = - Σ (A_t * log P(a_t|s_t))`。其中 `P(a_t|s_t)` 是智能体在状态 `s_t` 下选择行为 `a_t` 的概率。
  **更新参数**：计算损失函数 `L` 关于参数 `θ` 的梯度，并使用梯度下降法更新参数：`θ^i = θ^{i-1} - η * ∇L`。`η` 是学习率。
3. **输出**：训练完成后，得到最终的智能体参数 `θ^N`。

![](images/7905fbf0cc28.png)

![](images/15915bdecc5c.png)

一个关键点是：**每次参数更新后，必须用新的智能体重新收集数据**。这是因为数据是由旧策略 `θ^{i-1}` 产生的，它反映了旧策略的经验。新策略 `θ^i` 的行为分布已经改变，旧数据不能准确评估新策略下行为的好坏。这种模式称为 **On-policy（同策略）学习**。

---

![](images/a2e09e39e401.png)

## 重要概念延伸

### On-policy vs. Off-policy

![](images/2ed2bb717fac.png)

- **On-policy（同策略）**：用于与环境交互收集数据的策略（行为策略）和需要被评估、改进的策略（目标策略）是同一个。Policy Gradient 基础版本就是 On-policy。缺点是需要频繁重新采样数据，效率较低。
- **Off-policy（异策略）**：行为策略和目标策略可以不同。智能体可以从其他策略（如旧策略、探索性更强的策略）产生的经验中学习。这允许重复利用旧数据，大幅提升样本效率。
  
  **代表算法**：**PPO（Proximal Policy Optimization）** 是一种强大且常用的 Off-policy 算法。它的核心思想是约束新策略的更新幅度，使其不要偏离旧策略太远，从而能够安全地利用旧策略的数据进行学习。

![](images/9a458c527180.png)

![](images/cac245be0d5c.png)

![](https://github.com/OpenDocCN/dsai-notes-zh/raw/master/docs/leemldl-2026/img/06706b9bea82618347ca796d7f31ce42_31.png)

### 探索（Exploration）的重要性

![](images/7758c292a9a2.png)

![](images/80846deee3e2.png)

![](images/c064f3adc057.png)

![](images/815dfb1359a2.png)

![](images/243583b62d5f.png)

在训练初期，智能体需要尝试各种不同的行为，以了解哪些行为能带来高奖励。如果智能体过早地集中于某个看似不错的策略，可能会错过更优的策略。

![](images/911aa491d943.png)

![](images/af019dc138bf.png)

![](images/7382ed72bb30.png)

- **方法**：保持策略的随机性。例如，在输出行为概率时，可以增加熵（Entropy）正则化项来鼓励探索；或者直接在策略网络的参数上添加噪声。

---

![](images/60a1571dc292.png)

![](images/15cd298f5f84.png)

## 总结与回顾 🎓

![](images/305d1408459e.png)

![](images/eeb275d42d67.png)

![](images/8d3d5f8a9c28.png)

![](images/df1d8296df33.png)

![](images/9236791f0b8d.png)

![](images/e369e4702deb.png)

![](images/6d439711a8f1.png)

![](images/a83abf2f8a5c.png)

![](images/7dc9a253469d.png)

本节课我们一起学习了增强式学习中 Policy Gradient 方法的核心思想与流程。

![](images/e901ea0b2681.png)

![](images/67ae61850c75.png)

![](images/63f750411036.png)

![](images/bb7c474e21ad.png)

![](images/ff9eff12ca6e.png)

![](images/ac133acc1a9a.png)

![](images/7c0af9d320d9.png)

![](images/6e674983c338.png)

我们首先深入探讨了**如何评估行为**，从简单的即时奖励，发展到考虑长远规划的累积奖励和折扣累积奖励，最后通过标准化使评估更具相对性。这部分的公式 `G'_t = Σ γ^{k-t} * r_k` 和 `A_t = G'_t - b` 是关键。

![](images/1e0e7068d17c.png)

![](images/11ad5bb5b567.png)

![](images/06229af9a0f5.png)

![](images/0e2d3d1f39f0.png)

![](images/49f306c75bfc.png)

接着，我们梳理了 **Policy Gradient 算法的迭代步骤**：交互采样 -> 评估优势 -> 计算梯度 -> 更新策略。我们特别强调了其 **On-policy** 的特性，即必须用当前策略采样数据来更新自身，这导致了较高的采样成本。

![](images/010df05d54ca.png)

![](images/f1c94dc61540.png)

![](images/1f58163589df.png)

![](images/18d5d01bbf61.png)

![](images/04a8c25a8878.png)

![](images/2fcaa0adeea3.png)

最后，我们简要介绍了 **Off-policy** 学习（如 PPO）如何通过让智能体从其他策略的经验中学习来提高数据利用率，以及 **探索（Exploration）** 对于发现最优策略的必要性。

![](images/0f034a0ec427.png)

![](images/d4509bafcf11.png)

掌握这些内容，你便理解了 Policy Gradient 的基础，并能够着手实现和优化自己的增强式学习智能体了。
