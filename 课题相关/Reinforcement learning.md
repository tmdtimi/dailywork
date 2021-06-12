##强化学习

![](../pics/r1.png)

![](../pics/r2.png)



假设我是一个agent,处于一个environment当中，这个环境有一个特定的state状态，agent在environment中做出action。这些动作有时会带来reward，同时action也导致了environment的改变，导致了新的state,在新的state下，agent又采取新的动作。如此持续下去。

agent采取什么样的规则和规律选择action，构成了我们的policy。

![](../pics/r3.png)


环境有随机性，每当我们采取某种action时，都有一定概率导致某种state。所有可能的state和action的集合，state间互相转换的规律就形成了markov decession process。


马尔可夫链：

 状态矩阵  、状态转移矩阵P不变 ： 马尔可夫状态只与当前状态有关，而与之前的状态无关。


每一串连续的state,action,reward，叫做一个episode 

 ![](../pics/r4.png)

如在t时刻，观察到s(t)，采取行动a(t)，后来得到奖励r(t+1)，新的状态为s(t+1)，直到达到最终状态s(n)。

markov decession process遵循markov假设：下一步的状态只取决于这一步的状态和采取的措施。


**discounted future reward 和 长期策略**

为了长期的良好表现。需要考虑即时的奖励和未来的收益。

给定一个markov decision process ， 我们可以轻易获得一个episode的总奖励。


 ![](../pics/r5.png)

但是由于，环境的随机性，无法保证下次采取同样的行为还能得到同样的奖励。越往未来的时刻看，可能的情况越多，不确定的情况越大。

所以使用discounted 收益，越未来的reward 越贬值。

 ![](../pics/r6.png)

一个好的策略，是能最大化discount reward的策略。


**Q-learning**

定义一个Q值，来表示我们在某状态S下，采取某个行为a,所能得到的最大 discounted reward，由于不确定性，这个奖励是一个期望值。

 ![](../pics/r7.png)

Q可以理解为，在s(t)状态下，采取a(t)行为后，整个episode最后可能的最大好处。

如果这个Q值存在，那么很简单，每一步都选择Q最大的行为就行了。

但问题在于，如果只有现在的状态和行为，未来的状态和行为尚未发生，如何估计episode结束时的收益Q呢？

通过 Bellman equation得到。

**Bellman equation**

观察一次的状态变化<s,a,r,s`>，可以写出状态变化前后Q值得迭代关系，也就是bellman equation

 ![](../pics/r8.png)

Q-learning的目标是估计Q值，获得一个Q-table：

 ![](../pics/q1.png)

Q-learning获得Qtable的方法是通过基于Bellman equation不断迭代更新，最终收敛到准确的Q值。


 ![](../pics/q2.png)

**Deep Q Network**

若以图片的像素值作为state，图片可能得状态值太多了。尽管可能大多数状态不会出现，可以将状态空间表示得更稀疏，只保留观察到的现象。

但仍面临状态空间过大且多数状态很少被观测到，Q table的估计将花费大量的时间且很难收敛。

况且对于大量未观察到的状态，我们也希望进行Q值估计。

所以采用DQN 神经网络的方法：

用NN来近似 [公式] ，以状态（如四张图）和动作为输入，输出对应的Q值。

 ![](../pics/q3.png)

以状态作为输入，输入各种动作下对应的Q值，这种方法的优点是可以方便地从所有输出中找到最高的Q值，从而决定最优的动作。

 ![](../pics/q4.png)


基于以上的分析和所设计的architecture，NN以像素值为输入，输出若干个对应不同动作的实数Q值。这实际是一个用NN实现的regression，可以用简单的误差平方作为损失函数：

 ![](../pics/q5.png)

	对一次状态变化 <s,a,r,s'> ：
	1. forward pass 获得对当前状态和所有动作的Q值，即Q(s,a1)，Q(s,a2)...
	2. forward pass 获得对下一状态的最大Q值，即maxQ(s',a')=Q(s',a*)
	3. 对动作a，更新target；其他动作target不变
	4. backpropagation 依据每个动作的target和prediction，反向传播更新参数

