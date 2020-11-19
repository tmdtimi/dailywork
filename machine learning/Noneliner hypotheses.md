too many features

neural network：

![图片](https://uploader.shimo.im/f/VbGvA8FpXNciZCWu.png!thumbnail)

![图片](https://uploader.shimo.im/f/GxfKCJtcorV0fw8Z.png!thumbnail)

vectorized implementation

前向传播  forwards propagayion

![图片](https://uploader.shimo.im/f/Fo0rjtxdC0srvKan.png!thumbnail)

![图片](https://uploader.shimo.im/f/C5PPNCJK4ENOLcvS.png!thumbnail)

&&

![图片](https://uploader.shimo.im/f/OYif6pQ6jYrikFka.png!thumbnail)

!

![图片](https://uploader.shimo.im/f/O2OCcIhTr2cW0zBf.png!thumbnail)

![图片](https://uploader.shimo.im/f/dwX5R6KjPP7qPWSo.png!thumbnail)

![图片](https://uploader.shimo.im/f/IRcEmTLgi0HkfEQ6.png!thumbnail)


多类别分类问题：

![图片](https://uploader.shimo.im/f/aLG4ItzaUNm4u6aX.png!thumbnail)

![图片](https://uploader.shimo.im/f/KuL0vZwnkgsYryu1.png!thumbnail)

![图片](https://uploader.shimo.im/f/NvaBXF4jCYWJiLkl.png!thumbnail)

![图片](https://uploader.shimo.im/f/gprn06IsVanSis8f.png!thumbnail)


梯度下降：

![图片](https://uploader.shimo.im/f/bFmWJ2XOsawUsO32.png!thumbnail)

forward propagation 正向传播：

![图片](https://uploader.shimo.im/f/ol5LfnQqdLEEIPT8.png!thumbnail)

#### 

#### back propagation  反向传播

![图片](https://uploader.shimo.im/f/o3c6sXQXVaaLqzxJ.png!thumbnail)

其实就是链式求导： 求得是  d损失函数/d每一层到下一层的系数

![图片](https://uploader.shimo.im/f/S2PsjiZKU0CBCHgT.png!thumbnail)

正向传播：

![图片](https://uploader.shimo.im/f/ciDuwy1803bSMmrH.png!thumbnail)


根据链式求导规则 求导每一层对的 w系数

![图片](https://uploader.shimo.im/f/SisKax5jD9eGhdfG.png!thumbnail)

![图片](https://uploader.shimo.im/f/iAU13QqIX7eJhNAF.png!thumbnail)

运用梯度下降法更新每一层的w

![图片](https://uploader.shimo.im/f/M512MCnpC31BfI7D.png!thumbnail)

![图片](https://uploader.shimo.im/f/ZADT428ehS9lRXIN.png!thumbnail)

![图片](https://uploader.shimo.im/f/5EJosWXyvs0kWoWb.png!thumbnail)

![图片](https://uploader.shimo.im/f/wRwxi3Km7XZ6Dv4m.png!thumbnail)

代用梯度下降法更新w

![图片](https://uploader.shimo.im/f/g44v9TUzVeF7FcU6.png!thumbnail)









![图片](https://uploader.shimo.im/f/K0oBoiWrNOFcPYG3.png!thumbnail)


![图片](https://uploader.shimo.im/f/nybEE7OOlAhiXnlB.png!thumbnail)


![图片](https://uploader.shimo.im/f/EGCivlh2GDkFNt6f.png!thumbnail)


反向传播 梯度检验  ---bug

![图片](https://uploader.shimo.im/f/pvSutH1zXVjJoSX6.png!thumbnail)

![图片](https://uploader.shimo.im/f/ne7xwWN74fC24TuD.png!thumbnail)

![图片](https://uploader.shimo.im/f/IPzh4FZWMFMEOB2p.png!thumbnail)


初始化值选择：

计算冗余

![图片](https://uploader.shimo.im/f/CcHRfq5thtJNoj9H.png!thumbnail)

将权重随机初始化为一个接近0的 一定范围的数

然后  反向传播  ---梯度检验 ---- 梯度下降/其他优化算法


#### 训练神经网络步骤

![图片](https://uploader.shimo.im/f/6vq1qiY2rMbvPrER.png!thumbnail)

![图片](https://uploader.shimo.im/f/XfFDf4nvc8UyVakl.png!thumbnail)

![图片](https://uploader.shimo.im/f/e12JrYYSzhHTRWl3.png!thumbnail)





