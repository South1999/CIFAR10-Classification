# CIFAR10-Classification
___

![](https://img.shields.io/badge/language-python-blue) ![](https://img.shields.io/badge/-pytorch-orange)


基于pytorch的CIFAR10数据集分类。

## 目录
___
* [项目简介](#项目简介)
* [环境依赖](#环境依赖)
* [网络结构](#网络结构)
* [不同参数下的结果](#不同参数下的结果)
* [结论](#结论)
* [作者](#作者)


### 项目简介
___
本次作业是对CIFAR10数据集进行分类，是计算机视觉中的分类任务。我使用的[CIFAR10数据集](http://www.cs.toronto.edu/~kriz/cifar.html)主要包含了6个文件，每个文件的格式包含了10000张图片，格式为10000×3072，其中每张图片前1024个字节为R通道，之后1024个字节为G通道，最后1024个字节为B通道。6个文件中其中1个为测试集，其他5个划分为训练集和验证集（训练集大小:验证集大小=4:1）。提供数据集的网站中给出了读取文件的函数，之后可将数据调整为自己需要的格式。


### 环境依赖
___
下面仅是我个人的环境配置。
* Python 3.8.8
* PyTorch 1.9.0

### 网络结构
___
本次作业中使用的网络结构为ResNet-18，主要是由残差模块堆叠而成的。残差模块结构有两种，如下图所示，其中区别就是恒等映射是否使用1×1的卷积层。

![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE1.jpg?raw=true)

本文中使用的整体网络结构如下图所示，上下两幅图片均来自李沫的《动手学深度学习》（图片有修改），整体网络结构有所修改，第一层网络在书中是7×7的卷积网络，因为图片大小为32×32，7×7的卷积网络较大，不适合本数据集，因此本次作业中使用3×3的卷积网络。

![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE2.jpg?raw=true)

### 不同参数下的结果
___
下面的表格描述了不同超参数下，模型在训练集、验证集和测试集上的精度，其中优化器使用的是SGD，测试集的精度计算使用的是在验证集上表现最好的模型，训练集和验证集的精度是最后一轮的结果，图像是精度和损失的变化，第7个模型每30轮改变一次学习率。

| 序号 | num epoches | learning rate  | batch size | 图像增广 |                                         精度和损失变化                                          |
| ---- | ----------- | -------------- | ---------- | -------- | ---------------------------------------------------------------------------------------------- |
| 1    | 50          | 0.1            | 128        | 是       | ![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE3.jpg?raw=true) |
| 2    | 50          | 0.1            | 128        | 否       | ![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE4.jpg?raw=true) |
| 3    | 50          | 0.01           | 128        | 是       | ![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE5.jpg?raw=true) |
| 4    | 50          | 0.001          | 128        | 是       | ![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE6.jpg?raw=true) |
| 5    | 50          | 0.0001         | 128        | 是       | ![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE7.jpg?raw=true) |
| 6    | 50          | 0.0001         | 32         | 是       | ![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE8.jpg?raw=true) |
| 7    | 90          | 0.1→0.01→0.001 | 128        | 是       | ![](https://github.com/South1999/CIFAR10-Classification/blob/main/img/%E5%9B%BE9.jpg?raw=true) |


### 结论
___
图像增广可以扩大训练集的规模，提高模型的泛化能力，一定程度上缓解过拟合现象，对比模型1和2，两个模型在测试集上的表现几乎一样，但是模型2在训练集上精度比模型1更高，过拟合现象更严重一些。通常情况下，学习率影响模型的收敛速度和精度，过大的学习率虽然会使模型收敛很快，但是容易震荡，模型精度不高且不稳定，过低的学习率会导致收敛过慢，甚至陷入局部最优而无法跳出这个点，对比模型1、3、4、5、7可以得出上述结论，其中调整学习率的模型7效果最好，可以看出一般情况下，在训练前期模型学习率需要大一些，后期需要小一些。本次作业中使用的是批量梯度下降，对比模型5和6可以看出，每批训练样本少，收敛速度快，但是可能会出现不稳定的情况，每批训练样本多，收敛速度慢，但是较为稳定（这里可能因为32也不算很小，看不出模型6的精度有很明显抖动，但是收敛速度明显比5快）。关于训练轮数，训练过少会出现欠拟合现象，模型在训练集和验证集上的表现都不算好，训练过多会出现过拟合现象，模型在训练集上表现很好，但是在验证集上表现差。在训练模型至其在验证集上表现较好时，模型可能具有较好的泛化能力，才算是一个较为优秀的模型。在本次实验中，最好的模型是模型7，在测试集上的精度有0.858，但这个精度不算高。由于硬件条件有限，就选择了Resnet-18网络并调整一些超参数进行实验，目前已经有模型在CIFAR10测试集上的精度可达0.99以上，之后如果有能力条件会去试试。

### 作者
___
<a href="https://github.com/South1999"><img src="https://avatars.githubusercontent.com/u/37793548?v=4" width=180 height=180/></a>
