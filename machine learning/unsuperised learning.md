unlabeled traning set

![图片](https://uploader.shimo.im/f/DwJxD8Vf8rvzRhe6.png!thumbnail)

![图片](https://uploader.shimo.im/f/AF8ByzWbhg6iG6WG.png!thumbnail)

### 
## K-means clustering algorithm

![图片](https://uploader.shimo.im/f/1Gv5RhzzkPcG1gmF.png!thumbnail)

不断移动cluster centroids

![图片](https://uploader.shimo.im/f/AodygSMfGgFl7euC.png!thumbnail)

![图片](https://uploader.shimo.im/f/gvoNGNyIG68urx7a.png!thumbnail)

![图片](https://uploader.shimo.im/f/WzDRV0PaBePcGU9j.png!thumbnail)



![图片](https://uploader.shimo.im/f/oBsmQLoHPl1jxh4n.png!thumbnail)

![图片](https://uploader.shimo.im/f/zU1zycy6hp1b2mCZ.png!thumbnail)

### 初始化k近邻聚类算法 避免局部最优

![图片](https://uploader.shimo.im/f/30SkLRzBw8wFptV6.png!thumbnail)

随机初始化聚类中心 cluster centroids

![图片](https://uploader.shimo.im/f/O0YYEzJ4o23ftLS5.png!thumbnail)

### 局部最优

随机初始化可能得到局部最优

![图片](https://uploader.shimo.im/f/yE0LmTG3M35JXYUH.png!thumbnail)

所以需多次随机初始化：

![图片](https://uploader.shimo.im/f/Re3Hkm7f9plx13ci.png!thumbnail)

### 选取聚类数量

![图片](https://uploader.shimo.im/f/QuZd1Cgh2QRuWfVa.png!thumbnail)

![图片](https://uploader.shimo.im/f/llHWM8OcxdhlWWHQ.png!thumbnail)


## Dimensionality Reduction

数据压缩

![图片](https://uploader.shimo.im/f/l7DIjbcDAo7JLFhG.png!thumbnail)

投影  所需内存减半

![图片](https://uploader.shimo.im/f/w2gkvcgMMnR7xbad.png!thumbnail)

![图片](https://uploader.shimo.im/f/FECgSrdLD304g0I0.png!thumbnail)

![图片](https://uploader.shimo.im/f/TUpTHYNPNwCVhFaR.png!thumbnail)

![图片](https://uploader.shimo.im/f/ASAPejRixi5nC5k3.png!thumbnail)

PCA （principal component analysis）: 找到一个低维平面对数据进行投影

![图片](https://uploader.shimo.im/f/y6QFnW7HaTCJ0Mtf.png!thumbnail)

![图片](https://uploader.shimo.im/f/8RjIHa22EaajXb1g.png!thumbnail)

![图片](https://uploader.shimo.im/f/exz6lTjdqOuPhL9w.png!thumbnail)

PCA:

数据预处理：特征缩放

![图片](https://uploader.shimo.im/f/iFQ8OuJ2jPMzvfw5.png!thumbnail)

找到u

![图片](https://uploader.shimo.im/f/FmUsSQYzkPEn8vDc.png!thumbnail)

svd:奇异值分解

![图片](https://uploader.shimo.im/f/2aJoKVWGoTtYo2aQ.png!thumbnail)

得到 n*k 的降维矩阵

![图片](https://uploader.shimo.im/f/KIz49y7bQ26f17hl.png!thumbnail)


mean normalization 特征缩放

![图片](https://uploader.shimo.im/f/Wf8pzvtEtbtmhmJ5.png!thumbnail)

#### how to choose the parameter k of PCA

![图片](https://uploader.shimo.im/f/U0obJvuBQKfLSh8n.png!thumbnail)

![图片](https://uploader.shimo.im/f/aCYUtFRGLzGxcN1O.png!thumbnail)

PCA: a  compression  algorithm

three dimension data compress into two dimension presentation

#### way to back to your original  high dimension data

![图片](https://uploader.shimo.im/f/SiNx8ZuLNgzOgQ3h.png!thumbnail)

![图片](https://uploader.shimo.im/f/9I3lDFci2dh3SFVM.png!thumbnail)

![图片](https://uploader.shimo.im/f/Gt9YIjEQDORq7rEP.png!thumbnail)

![图片](https://uploader.shimo.im/f/CsNDhYXNmGMWD1US.png!thumbnail)

![图片](https://uploader.shimo.im/f/5vr4otrkAhmjQric.png!thumbnail)

