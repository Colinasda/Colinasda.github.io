---
layout:     post
title:      强化学习Sarsa和Sarsa Lambda
subtitle:   on-policy强化学习Sarsa和Sarsa Lambda
date:       2021-1-30
author:     Zhao Zihao
header-img: img/21.jpg
catalog: false
tags:
    - 强化学习
---


#### Sarsa

”SARSA“ 五个字母是当前 S (状态), A(行动), R(奖励) 与 下一步S'(状态) A'(行动) 的组合，即我们不仅需要知道当前的S, A, R 还需要知道下一步的 S' 和 A‘。

在Sarsa算法中，智能体的**目标**是**R(S1) + γ\*Q(S1,A)**

至于A是多少，完全取决于智能体实际上选择的哪一个Action。智能体有90%的概率会选择Q值最大的Action(A2)，还有10%的概率会随机选择一个Action。

所以，Sarsa的算法是这样的。

> Q(S0,A2) = Q(S0,A2) + α[R(S1) + γ*Q(S1,A)-Q(S0,A2)]

除了其目标值与Q learning 有所不同之外，其他的都是一模一样的。

所以Sarsa是在线学习(On Policy)的算法，因为他是在行动中学习的，而且至始至终**只有一个Policy.** 使用了两次greedy-epsilon 方法来选择出了Q(S,A)和q(S',A'）。

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn8aehfuirj30gs09nqfl.jpg)

而Q learning离线学习(Off Policy)的算法，QLearning选择Q(S,A)用了greedy方法，而计算A(S',A')时用的是max方法，而真正选择的时候又不一定会选择max的行动, 所以 Q learning 学习和行动分别采用了**两套不同的Policy**

Q learning 通过Max的函数，总是在寻找能最快获得宝藏的道路，所以他比较勇敢。

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn82n58o44g304g058e88.gif)

同样的maze game，我们可以发现Sarsa 却相对谨慎，会走很多重复的steps。

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gn82txqokzg304g0587ws.gif)



#### Sarsa Lambda

Q learning 和 Sarsa都是单步更新的算法。单步跟新的算法缺点就是在没有找到宝藏之前，智能体在原地打转的那些行动也被记录在案，并更新了Q表，即便那些行动都是没有意义的。

Lambda(λ)这个衰减系数的引入，就是为了解决这个问题的。与γ用来衰减未来预期Q的值一样，λ是当智能体获得宝藏之后，在更新Q表的时候，给智能体一个回头看之前走过的路程的机会。相当于，智能体每走一步就会在地上插一杆旗子，然后智能体每走一步旗子就会变小一点。

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn8ang7z1fj30gn0580z4.jpg)

Sarsa Lambda的pseudo code如下：

![](https://tva1.sinaimg.cn/large/008eGmZEly1gn8alpxvs1j30hf0a3jvv.jpg)

注意，该算法与Sarsa 算法不同的地方就是多乘了一个E(s, a) (Eligibility Trace"不可或缺性值")，而这个E(s, a)又同时受γ和λ调控。并且在更新Q表的时候，不仅仅是更新一个Q(S,A)，而是整个Q表所有的Q值都被更新了。



