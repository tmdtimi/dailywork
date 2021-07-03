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




**Q-learning实例**

![](../pics/s1.png)

将编号为5的房间设置为目标，为每一扇门设置一个reward。 可以构建R矩阵代表环境

![](../pics/s2.png)

-1代表无路径。 100 代表可以到达目标。

可以构建一个矩阵Q，表示Agent已经从经验中学到的知识，矩阵Q和R同阶。

行表示状态，列表示行为。

由于刚开始，agent对外界一无所知，所以初始化为0；

![](../pics/s3.png)

在本例中，状态数量是已知的，

对于状态未知的情形，我们可以让Q从一个元素出发，每次发现一个新的状态时，就可以在Q中增加相应的行列。

算法的转移规则： ![](../pics/s4.png)

初始房间为1 ， R的第二行包含两个值不是-1 ，所以在1这个state下，下一步的行为可能有两种。 转移到3 或者 转移到5

随机转移到5，更新Q(1,5)

![](../pics/s5.png)

到达目标状态，一次episode完成，Q表更新。

![](../pics/s6.png)

执行更多次episode，矩阵Q最终收敛成

![](../pics/s7.png)

对其进行规范化，每个非零元素都除以矩阵Q的最大元素

![](../pics/s8.png)

一旦矩阵足够接近收敛状态，Agent便学习到了转移至目标状态的最佳路径。

Q-Learning的思想完全根据值迭代得到。然而面对无限的状态、动作空间的实际应用问题，没办法遍历所有的状态和动作，得到各状态期望价值的准确估计，只能利用有限的样本数据来更新Q值。

![](../pics/s9.png)

由于是有限的样本估计，没有直接将这个Q值（估计值）直接赋予新的Q值，而是采用渐进的方式类似梯度下降，朝目标更迈进一步，取决于α，这样能够减少估计误差造成的影响。类似梯度下降，最后可以收敛到最优的Q值。

上式可以看做：更新后的Q值=更新前的Q值+某种修正






**Sarsa**


![](../pics/sa4.png)


单步更新：直到获得宝藏时，才为获得宝藏的上一步更新

回合更新： 对所有步进行更新

![](../pics/sa1.png)

![](../pics/sa2.png)

sarsa(0) :单步更新

sarsa(1) :回合更新


![](../pics/sa3.png)



**DQN**

传统的表格形式存在瓶颈。 state / action 过多，内存    神经网络+Q learning

将state / action 作为NN的输入值 最后得到 Q值  直接使用神经网络生成Q值

或 只输入 state 然后输出所有Q 选择最大Q

![](../pics/nn1.png)

只输入状态值，输出所有的动作对应的Q值，按照Qlearning的原则直接选取最大值的动作作为下一个动作。

![](../pics/nn2.png)


DQN两大因素：能够提升DQN的效果

Experience replay: DQN中有一个记忆库用于学习之前的经历，Q learning是一个off policy的离线学习方式，能学习当前经历过的，也能学习过去经历过的，所以每次DQN更新的时候，我们可以随机抽取之前的经历进行学习。随机抽取打乱了经历之间的相关性，也使得神经网络更新更有效率。

Fixed Q-targets ： 也是一种打乱相关性的机理，

![](../pics/nn3.png)



单纯使用Q learning和神经网络 结果可能很难收敛

所以采用了： 记忆库（用于重复学习） experience replay / 神经网络计算Q值 / 暂时冻结q_target参数（切断相关性） Fixed Q-targets


































**Playing Atari with Deep Reinforcement Learning**

![](../pics/z1.png)



![](../pics/d1.png)



![](../pics/z2.png)


传统的方法为每一个action学习一个函数，而不是一个网络结构直接输出所有action的Q value


输入信息预处理

![](../pics/z3.png)


![](../pics/z4.png)

![](../pics/z5.png)


