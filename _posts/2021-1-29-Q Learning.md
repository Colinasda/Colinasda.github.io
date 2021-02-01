---
layout:     post
title:      强化学习Q Learning
subtitle:   Q Learning的介绍以及简单demo
date:       2021-1-29
author:     Zhao Zihao
header-img: img/22.jpg
catalog: false
tags:
    - 强化学习
---


#### Q Learning的介绍

智能体agent的每一次学习过程可以看作是从一个随机状态开始，采用一个策略来选择动作，如ε-greedy策略或Boltzamann分布策略。采用随机策略是为了**保证智能体能够搜索所有可能的动作**，对每个Q(s，a)进行更新。智能体在执行完所选的动作后，观察新的状态和回报，然后根据新状态的最大Q值和回报来更新上一个状态和动作的Q值。智能体将不断根据新的状态选择动作，直至到达一个终止状态。下面是Q—learning算法的描述：

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn889nwt86j30ir07ak2c.jpg)

每次更新我们都用到了 Q 现实和 Q 估计, 而且 Q-learning 的迷人之处就是 在 Q(s1, a2) 现实中, 也包含了一个 Q(s2) 的最大估计值, 将对下一步的衰减的最大估计和当前所得到的奖励当成这一步的现实, 很奇妙吧. 最后我们来说说这套算法中一些参数的意义. ε-greedy 是用在决策上的一种策略, 比如 ε= 0.9 时, 就说明有90% 的情况会按照 Q 表的最优值选择行为, 10% 的时间使用随机选行为. α是学习率, 来决定这次的误差有多少是要被学习的, α是一个小于1 的数. γ是对未来 reward 的衰减值. 

Q-learning 是一个 off-policy 的算法, 因为里面的 max action 让 Q table 的更新可以不基于正在经历的经验(可以是现在学习着很久以前的经验,甚至是学习他人的经验).

On-policy 与 off-policy 本质区别在于：更新Q值时所使用的方法是沿用既定的策略（on-policy）还是使用新策略（off-policy）

#### Q Learning的Demo

红色智能体在4x4的迷宫中寻找黄色的宝藏。找到宝藏将会的到+1的奖励，如果掉进黑色陷阱就回的到-1的奖励(惩罚)。

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn82txqokzg304g0587ws.gif)

首先我们对maze进行一个位置的标记：

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn88zrjkkvj307r08rjs9.jpg)

假设Q table是这样：

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn890mxn6yj30im049mxe.jpg)

其中，对Q(S0,A2)有：

Q(S0,A2) = Q(S0,A2) + α[R(S1) + γ*maxa Q(S1,a)-Q(S0,A2)]

在Q Learning 算法中，当智能体处于S0的状态时，它的**目标值**是: **R(S1) + γ\*maxa Q(S1,a)**。此时他还在S0的位置上，但是已经在计算S1上的最大Q值了。但是此时它并没有行动，也不一定会在S1采取Q值最大Q(S1, A2)的行动。因为它还有10%的概率随机选择其他的行动 (ε -Greedy method)。

完整代码：[]()

