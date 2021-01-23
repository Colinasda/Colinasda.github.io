**meta learning与few-shot learning联系：**

我们需要元学习模型学习一个先验知识来帮助以后学习一个新的任务，这就导致很多元学习论文中会有  Task 或者 Episode 的概念，也就是我们需要学习很多类似的任务，然后用在这些任务上学到的先验知识使我们面对一个新问题的时候可以学习得又快又好，又快又好也就使得现在的元学习（meta learning）和少样本学习（few-shot learning）紧紧联系在一起。



### Meta learning的定义

​		 一般的机器学习任务是，通过训练数据得到一个模型，然后在测试数据上进行验证。一般来说我们仅关注模型在该任务上的表现。而meta learning则探讨解决另一个问题，就是我们能否通过学习不同的任务，从而让机器学会如何去学习呢？也就是learn to learn。我们关注的不再是模型在某个任务上的表现，而是模型在多个任务上学习的能力。

​		试想一下机器学习了100个任务，他在第101个任务上一般就可以学的更好。比如机器学习了图像分类、语音识别、推荐排序等任务后，在文本分类上，它就可以因为之前学到的东西，而学的更好。meta learning就是解决这个问题，如何让机器去学习。

![](https://tva1.sinaimg.cn/large/008eGmZEly1gmxn76tm5aj30iv0agafz.jpg)

一般的机器学习任务，我们是需要学习一个模型f，由输入x得到输出y。而meta learning，则是要学习一个F，用它来学习各种任务的f。如下图

![](https://tva1.sinaimg.cn/large/008eGmZEly1gmxx2ga8znj30iq07odi7.jpg)



### meta learning的优势

meta learning的优势主要有

1. 让学习更加有效率。我们通过多个task的学习，使得模型学习其他task时更加容易。
2. 样本数量比较少的任务上，更加需要有效率的学习，从而提升准确率和收敛速度。meta learning是few shot learning的一个比较好的解决方案



### meta learning的研究思路

**1 基于记忆Memory的方法**

基本思路：既然要通过以往的经验来学习，那么是不是可以通过在神经网络上添加Memory来实现呢？

代表文章：

[1] Santoro, Adam, Bartunov, Sergey, Botvinick, Matthew, Wierstra, Daan, and Lillicrap, Timothy. **Meta-learning with memory-augmented neural networks**. In Proceedings of The 33rd International Conference on Machine Learning, pp. 1842–1850, 2016.

[2] Munkhdalai T, Yu H. **Meta Networks**. arXiv preprint arXiv:1703.00837, 2017.

以Meta-Learning with memory-augmented neural networks这篇文章为例，网络结构如下：

![](https://tva1.sinaimg.cn/large/008eGmZEly1gmxx9p4mndj30jq05r762.jpg)

我们可以看到，网络的输入把上一次的y label也作为输入，并且添加了external memory存储上一次的x输入，这使得下一次输入后进行反向传播时，可以让y label和x建立联系，使得之后的x能够通过外部记忆获取相关图像进行比对来实现更好的预测。



**2 利用Attention注意力机制的方法**

基本思路：人的注意力是可以利用以往的经验来实现提升的，比如**我们看一个性感图片，我们会很自然的把注意力集中在关键位置**。那么，能不能利用以往的任务来训练一个Attention模型，从而面对新的任务，能够直接关注最重要的部分。

代表文章：

[1] Vinyals, Oriol, Blundell, Charles, Lillicrap, Tim, Wierstra, Daan, et al. **Matching networks for one shot learning**. In Advances in Neural Information Processing Systems, pp. 3630–3638, 2016.

![](https://tva1.sinaimg.cn/large/008eGmZEly1gmxxdb7vs1j30j60chaey.jpg)

这篇文章构造一个attention机制，也就是最后的label判断是通过attention的叠加得到的：

![](https://tva1.sinaimg.cn/large/008eGmZEly1gmxxdfpxzcj304d01w74a.jpg)

attention a则通过g和f得到。基本目的就是利用已有任务训练出一个好的attention model。

