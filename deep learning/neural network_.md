A single hidden layer

![图片](https://uploader.shimo.im/f/lIycAlCsbw8gtI5P.png!thumbnail)


logic regression

![图片](https://uploader.shimo.im/f/ydXvbzLCfGpfdCIh.png!thumbnail)

![图片](https://uploader.shimo.im/f/OVYOXCHdAFPEuFRo.png!thumbnail)


# 向量化计算：

![图片](https://uploader.shimo.im/f/fSYO9ErwRkSCy1Eh.png!thumbnail)


![图片](https://uploader.shimo.im/f/YtiWx8j3qzUMgdz1.png!thumbnail)

![图片](https://uploader.shimo.im/f/IqGXJbwMGfN3Rh2w.png!thumbnail)

a[2][i]     2代表层数



![图片](https://uploader.shimo.im/f/ZitzHz53sss7M4kt.png!thumbnail)

![图片](https://uploader.shimo.im/f/IOVrVaWvsoLhtzHg.png!thumbnail)

A[i] ------横向指标代表了不同的训练样本得到的结果

竖向的指标代表了神经网络的不同节点

![图片](https://uploader.shimo.im/f/yJoW3pQ94cjQG4wD.png!thumbnail)

![图片](https://uploader.shimo.im/f/bC99p5XEZHoF009p.png!thumbnail)

# 激活函数：
sigmod(z)  tan(z)     ReLU    leakyReLU

![图片](https://uploader.shimo.im/f/HeD5I7L1LcBB2OwX.png!thumbnail)


![图片](https://uploader.shimo.im/f/afwChwzLAfmX7SE1.png!thumbnail)

如果不使用激活函数： 不管神经网络有多少层  都是在做计算线性的激活函数

--hidden layer无用

![图片](https://uploader.shimo.im/f/ytM8bJ6HTAOB5Eid.png!thumbnail)

激活函数的导数：

![图片](https://uploader.shimo.im/f/pBB87rArEOHjoahS.png!thumbnail)

![图片](https://uploader.shimo.im/f/UHvrE6uMKgbQJOuW.png!thumbnail)

![图片](https://uploader.shimo.im/f/NXZ1D2ZRsoVvFeTo.png!thumbnail)


# Gradient descent for neural networks:
梯度下降：  重复 更新 每一层的  w b

![图片](https://uploader.shimo.im/f/4Bpp0x75giFznrDE.png!thumbnail)

正向 反向 传播

![图片](https://uploader.shimo.im/f/E3A0kPSVs0jxCt3P.png!thumbnail)

# 理解反向传播：
单层 逻辑回归

链式求导

![图片](https://uploader.shimo.im/f/KtdE2BRGFvpuaHSj.png!thumbnail)

两层神经网络

链式求导

![图片](https://uploader.shimo.im/f/QPnDFnjNmqVRlJSu.png!thumbnail)

将结果向量化：

![图片](https://uploader.shimo.im/f/GcZR0oBKcEsW7Dt1.png!thumbnail)

向量化正向传播计算

![图片](https://uploader.shimo.im/f/Q9Zh4MYUxrTKeNPZ.png!thumbnail)

反向传播 链式求导向量化后：

![图片](https://uploader.shimo.im/f/agwl9byJaNi3GEi0.png!thumbnail)

# 随机初始化：
如果将w初始化为0

无论计算多少次，两个隐藏单元都在计算完全一样的函数

![图片](https://uploader.shimo.im/f/pVV44SuHT1TOoxy8.png!thumbnail)

---随机初始化所有参数

![图片](https://uploader.shimo.im/f/1jUQmoeGhvOUO0lB.png!thumbnail)

如果，w的初始值很大，可能会导致 z变大，使得a到达一个平缓的区域 ，学习的速度变慢



