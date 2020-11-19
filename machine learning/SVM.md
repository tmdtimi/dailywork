

![图片](https://uploader.shimo.im/f/N7KXBKmJjgWXuxc1.png!thumbnail)![图片](https://uploader.shimo.im/f/e6DpjslHpc0qBiTy.png!thumbnail)

![图片](https://uploader.shimo.im/f/VemAZ298Q0PR56tb.png!thumbnail)

![图片](https://uploader.shimo.im/f/Qjonlzmz7PtB5gDR.png!thumbnail)

![图片](https://uploader.shimo.im/f/HGyGFMDbddZMnfJ7.png!thumbnail)

![图片](https://uploader.shimo.im/f/rZeCb5HAc7UJOoVA.png!thumbnail)

大间距分类器：用尽可能大的间距将样本区分开


![图片](https://uploader.shimo.im/f/glvk6Zp0fyTKbxyI.png!thumbnail)



![图片](https://uploader.shimo.im/f/Kz48K4KwJUiShUMv.png!thumbnail)

![图片](https://uploader.shimo.im/f/XpDnowXMZmZY5m4I.png!thumbnail)


![图片](https://uploader.shimo.im/f/ak9wrD8Mwtxezoss.png!thumbnail)


![图片](https://uploader.shimo.im/f/tiugW8gVQQtEkPnP.png!thumbnail)


#### 核函数构造新特征：

![图片](https://uploader.shimo.im/f/14feMJhXw2xOWl3Q.png!thumbnail)

similatiy(x,l) :  a kernel function

exp:()  : a gaussian kernel

衡量 x 和 l 之间的相似度

![图片](https://uploader.shimo.im/f/xKml0zQusdK7Dszx.png!thumbnail)

高斯核函数：

![图片](https://uploader.shimo.im/f/XG93LEkMCTvAMhqC.png!thumbnail)

通过标记点和相似性函数来定义新的特征变量

从而训练更复杂的非线性边界 noliner classfiers

![图片](https://uploader.shimo.im/f/ozcLtyPtNzhPu4Pm.png!thumbnail)

选择标记点位置为训练集点

![图片](https://uploader.shimo.im/f/Ec5vmEtlXX6i3mQZ.png!thumbnail)

![图片](https://uploader.shimo.im/f/yAMidLVoqidOzXYZ.png!thumbnail)

训练集的点 和 标记点 通过核函数计算相似度得到的 f特征向量 用于描述 训练集

。。。。。

![图片](https://uploader.shimo.im/f/oYNONUVlK5GCDG5k.png!thumbnail)

![图片](https://uploader.shimo.im/f/QER7FWiufexMLYXP.png!thumbnail)



SVM ： 选择 C  正则参数

选择核函数： 维数少，特征值多

![图片](https://uploader.shimo.im/f/OVM3x4llqClZ7WTB.png!thumbnail)

如果选择高斯核函数

![图片](https://uploader.shimo.im/f/7K1HhCGF6VRBNAMB.png!thumbnail)

计算特征值前仍需 特征缩放



其他种类的核函数

![图片](https://uploader.shimo.im/f/8COwUEU5V5CDFqHV.png!thumbnail)、

模型选择问题：

![图片](https://uploader.shimo.im/f/eEvrDllsVk8g5813.png!thumbnail)

