---
layout:     post
title:      Few Shot Learning
subtitle:   少样本学习的概念、定义以及优缺点
date:       2021-1-21
author:     Zhao Zihao
header-img: img/post-bg-os-metro.jpg
catalog: false
tags:
    - 机器学习
---


### few-shot learning出现原因

众所周知，现在的主流的传统深度学习技术需要大量的数据来训练一个好的模型。例如典型的 MNIST 分类问题，一共有 10 个类，训练集一共有 6000 个样本，平均下来每个类大约 600 个样本，但是我们想一下我们人类自己，我们区分 0 到 9 的数字图片的时候需要看 6000 张图片才知道怎么区分吗？很显然，不需要！这表明当前的深度学习技术和我们人类智能差距还是很大的，要想弥补这一差距，少样本学习是一个很关键的问题。

另外还有一个重要原因是如果想要构建新的数据集，还是举分类数据集为例，我们需要标记大量的数据，但是有的时候标记数据集需要某些领域的专家（例如医学图像的标记），这费时又费力，因此如果我们可以解决少样本学习问题，只需要每个类标记几张图片就可以高准确率的给剩余大量图片自动标记。这两方面的原因都让少样本学习问题很吸引人。

在 few-shot learning 中有一个术语叫做*N*-way *K*-shot 问题，简单的说就是我们需要分类的样本属于 *N* 个类中一种，但是我们每个类训练集中的样本只有 *K* 个，即一共只有 *N***K*个样本的类别是已知的。

![](https://tva1.sinaimg.cn/large/008eGmZEly1gmxv2940cmj30io03x76z.jpg)

图片来自论文《Optimization as a Model for Few-Shot Learning.》，这是一个 5-way 1-shot 的图示，左边是训练集一共 5 张图片来自 5 个类，每个类只有一张图片。右边是测试集，理论上可以有任意多个图片用于测试，图中只给出了两张实例。

下图也是一个few-shot learning的例子：

![](https://tva1.sinaimg.cn/large/008eGmZEly1gmxvoceduij30im05cjud.jpg)



### few-shot learning中的重要概念

* episode：每一个episode便是训练一次，类似于传统训练方法里的一个batch，然后这个episode内部由两部分构成：support set和query set

- support set：在构建这个episode时候，会从全量数据中每个episode都随机选择一些类别，比如C个类别，然后从数据集中同样随机从这选定的C个类别中选取同样数量的K个样本，这便构成了support set，总共包含C * K个样本。一般而言模型会在这个上面进行一次训练。这样构造出来的任务便是C-way K-shot
- query set: 和support set类似，会在剩下的数据集样本中的C个类（注意这里的类别是从对应的spport set类别选择）采样一些样本作为query set，一般而言，模型在support set进行一次训练后，会在query set上求得loss
- task：一个episode包含一个support set和一个query set，一个episode对应的便是让模型在support set上进行学习后能够在query set上有比较好的预测表现。因此一个episode便对应为一个task

- **meta trainig set:** 通常而言，根据训练数据的规模大小，可以构建出来多个训练的episode，这些episode便可以称为meta-training set
- **meta test set**: 因为在meta training set的若干个task（也就是若干个episode）上已经训练好了一个模型，那么希望模型在一些新的task上也能有比较好的表现。因此，meta test set在数据构成上和meta training set完全一致，也包含了support set和query set，基本思想就是希望模型能够基于新任务下的support set，迅速抓住问题的本质，从而能够快速在meta test set中的query set上取得比较好的效果。

![](https://tva1.sinaimg.cn/large/008eGmZEly1gmxw5tux6fj30gf07r74m.jpg)

前面三个是三个 meta-training task （当然实际需要很多这样的 meta-training task 才能学到我们需要的先验知识），最后一个就是 meta-testing task。我们最后评价我们的 meta-learning 方法的好坏就是在红色标记部分上的 performance。



### few-shot learning的问题

Few-shot learning 问题的关键是解决过拟合 (overfitting) 的问题，因为训练的样本太少了，训练出的模型可能在训练集上效果还行，但是在测试集上面会遭遇灾难性的崩塌。



### 解决方案

#### 1 数据增强和正则化

这一类方法想法很直接简单，既然训练数据不够那我就增加训练样本，既然过拟合那我就使用正则化技术。

1. 数据加强：最常见的例子就是有时对 Omniglot 数据集的预处理，会将图片旋转 90 、180 、270 度，这样就可以使样本数量变为原来的 4 倍。
2. 正则化：在训练的时候加入一个正则项，这个正则项的构建选择是关键。比如 《Few-shot Classification on Graphs with Structural Regularized GCNs》。该论文讨论 Graph 中节点分类的 few-shot 问题，常见的节点分类是使用 GCN 从节点的特征向量 feature 学习一个 embedding 然后用 embedding 来做分类，如果是 few-shot 问题，性能会大大下降（准确率大约从 70% 到了 40%），作者在训练的时候给损失函数加了一个正则项。作者将 feature 到 embedding 的过程看成编码器 encoder，然后额外加了几层网络作为 decoder，将 embedding 重构为 feature ，然后重构误差作为正则项（准确率从 40% 提升了 50%，大约 10 个百分点）。（更多内容可以参考这篇论文和别的使用正则化技术的论文）

#### 2 Meta-learning（元学习）

元学习的核心想法是先学习一个先验知识（prior），这个先验知识对解决 few-shot learning 问题特别有帮助。Meta-learning 中有 task 的概念，比如上面图片讲的 5-way 1-shot 问题就是一个 task，我们需要先学习很多很多这样的 task，然后再来解决这个新的 task 。最最重要的一点，这是一个新的 task。分类问题中，这个新的 task 中的类别是之前我们学习过的 task 中没有见过的！

Meta-learning 中 task 的概念是和 meta-learning 的本质有关的。Meta-learning 其实还有一个名字叫做“学会学习” (Learn to learn)，这来自于对人类智能的基本认知：我们人类学习一个东西的时候不是从头开始学的，都是基于之前学习的知识来进行学习的。比如如果你会玩英雄联盟，那你学习玩王者荣耀会快很多。但是我们的深度学习模型呢，学习新的东西（可以看成新的类，新的 task）需要从头开始学习（ 当然需要大量的样本 ），即使你之前学过特别类似的东西。因此 Meta-learning 就像让深度学习学会学习，利用之前学过的知识在面对新的问题可以学习得又快又好，如何体现又快又好呢？ –答案是 few-shot learning 。

 因此 meta-learning 和 few-shot learning 现在的研究基本都是在一起的。

Meta-learning传送门:[Meta Learning](https://colinasda.github.io/2021/01/23/MetaLearning/)
