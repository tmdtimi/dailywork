

content based recommender system:

![图片](https://uploader.shimo.im/f/vcnh5xFLgXM0yERL.png!thumbnail)

变成一个线性回归问题 ：  预测打分

x1  x2  特征为电影类型的相关度

![图片](https://uploader.shimo.im/f/uAj5T95DFpY1dYZ1.png!thumbnail)

最小化代价函数

![图片](https://uploader.shimo.im/f/wdjir3ntCojauVnT.png!thumbnail)

梯度下降

![图片](https://uploader.shimo.im/f/j0dv2EGk6l6J6y9G.png!thumbnail)

collaborative filtering 协同过滤

![图片](https://uploader.shimo.im/f/WjHrrvkRxchrWCgX.png!thumbnail)



![图片](https://uploader.shimo.im/f/ITUpgFbzyByasJG8.png!thumbnail)

两种迭代方法：

1：给出一组特征来表示电影 去 学习 用户的参数 theta

2：给出一组theta    学习电影的特征

![图片](https://uploader.shimo.im/f/Uq8Eikk0d9P8uLtf.png!thumbnail)


合二为一

![图片](https://uploader.shimo.im/f/GHqb3yQ0vxV7DcjN.png!thumbnail)

![图片](https://uploader.shimo.im/f/pVNWNZhfPFt4Y2dk.png!thumbnail)

低秩矩阵

![图片](https://uploader.shimo.im/f/bbcfBMe5y1QTCa1D.png!thumbnail)

特征值距离最短 的  两部电影

![图片](https://uploader.shimo.im/f/sCO4dnwApDnz1RN5.png!thumbnail)

特征值稀疏稀疏  冷启动  预测的分数为0 ------均值一体化

![图片](https://uploader.shimo.im/f/lsNwYNvRTwxRRQps.png!thumbnail)

![图片](https://uploader.shimo.im/f/Kx9cT52J28TrcvFG.png!thumbnail)


