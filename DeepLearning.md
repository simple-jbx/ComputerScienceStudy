# [深度学习](https://github.com/exacity/deeplearningbook-chinese)

# 机器学习基础

深度学习是机器学习的一个分支。学习深度学习必须要对机器学习的基本原理要有深刻的理解。

## 学习算法

学习算法——一种能够从数据中学习的算法。

何为学习？——‘‘对于某类任务 T 和性能度量 P，一个计算机程序被认为可以从经验 E 中学习是指，通过经验 E 改进后，它在任务 T 上由性能度量 P 衡量的性能有所提升。”

### 任务T

从 ‘‘任务’’ 的相对正式的定义上说，学习过程本身不能算是任务。学习是我们所谓的获取完成任务的能力。

- 分类
- 输入缺失分类（某些输入缺失）
- 回归（对给定的输入预测输出数值）
- 转录（将相对非结构化表示的数据转录信息为离散的文本形式，例如，光学字符识别、语音识别）
- 机器翻译
- 结构化输出（将输入转为结构化（向量、包含多个值的数据结构）的输出，语法分析、标注型任务）
- 异常检测
- 合成和采样（生成和训练数据相似的新样本，游戏中自动生成大型物体或风景的纹理）
- 缺失值填补（图像修复）
- 去噪（根据损坏样本预测干净样本）
- 密度估计或概率质量函数估计
- …

### 性能度量P

为了评估机器学习算法的能力，必须设计其性能的定量度量。通常性能性能度量P是特定于系统执行的任务T而言的。

准确率、错误率

测试集进行性能度量

有时没有一个理想的P，则需要设计一个近似理想的P

### 经验E

机器学习算法可以大致分为**无监督（unsupervised）**算法和**监督（supervised）**算法。

大部分学习算法可以理解为在整个**数据集（dateset）**上获取经验。数据集由很多组样本组合而成，有时也将样本成为**数据点（date point）**。

**无监督学习算法（unsupervised learning algorithm）**训练含有很多特征的数据集，然后学习出这个数据集上有用的结构性质。

**监督学习算法（supervised learning algorithm）**训练含有很多特征的数据集，数据集中的样本都有对应的**标签（label）**或**目标（target）**。

无监督学习涉及到观察随机向量**x**的好几个样本，试图显式或隐式学习出概率分布***p(x)***; 监督学习包含观察随机向量**x**及其相关联的值或向量**y**，然后从**x**预测**y**，通常是估计**p(y | x)**。

无监督学习与监督学习并不是泾渭分明的，有时可以通过无监督学习方式解决监督学习的问题，或者反过来。

还有些机器学习算法并不是训练于一个固定的数据集上。例如，**强化学习（reinforcement learning）**算法会和环境进行交互，学习系统会和它的训练过程会有反馈回路。

常用**设计矩阵（design matrix）**来表示数据集。

## 容量、过拟合、欠拟合

**泛化（generalization）**：在先前未观测到的输入上表现良好的能力称为泛化。

通常情况下，当我们训练机器学习模型时，我们可以使用某个训练集，在训练

决定机器学习算法效果好坏的因素：

1. 降低训练误差
2. 缩小训练误差和测试误差的差距

这两个因素对应机器学习的两个主要挑战：**欠拟合（underfitting）**和**过拟合（overfitting）**。

通过调整模型的**容量（capacity）**，可以控制模型是否偏向于过拟合或者欠拟合。模型的容量是指其拟合各种函数的能力。容量低的模型可能很难拟合训练集。容量高的模型可能会过拟合。

一种控制训练算法容量的方法是选择**假设空间（hypothesis space）**