# Deep_Learning_Keras
<br>notebooks of learning Deep learning by Keras


## 数据预处理，特征工程和特征学习
### 神经网络的数据预处理

1 . 向量化  
神经网络的所有输入和目标都必须是浮点数张量(特定情况下可以是整数张量)  
常用的编码方法为 `one-hot` 转换为float32格式的张量。  

2 . 值标准化  
对每个特征分别做标准化，使其均值为0，标准差为1.
如果输入数据相对较大，输入到神经网络钟不是不安全的，这么做可能导致较大的梯度更新，进而导致网络无法收敛，为了让网络的学习变得更容易，输入数据应该具有以下特征。
 * 取值较小:大部分值都应该在0~1范围内。  
 * 同质性:所有特征的取值都应该在大致相同的范围内。  
  - 将每个特征分别标准化，使其平均值为0
  - 将每个特征分别标准化，使其标准差为1  
  这对于Numpy数组很容易实现。  
  x-= x.mean(axis = 0)  
  x/= x.std(axis =0) 

3 . 处理缺失值
数据中有时可能会有缺失值，因为不是每个样本都有这个特征。一般来说，对于神经网络，将缺失值设置为0是安全的，只要0不是一个有意义的值。网络能够从数据中学到0意味着缺失数据，并且会忽略这个值。  

### 特征工程

特征工程的本质:用更简单的方式表述问题，从而使问题变得更容易。它通常需要深入理解问题。

即使有了深度神经网络，依然需要特征工程的原因:
* 良好的特征依然可以让你用更少的资源更优雅地解决问题。列如:使用卷积神经网络来读取钟面上的时间是非常可笑的。
* 良好的特征可以让你用更少的数据解决问题。

## 防止神经网络过拟合的常用方法:
* 获取更多的训练数据(最优解决方法)
* 减小网络容量(最简单的方法)  
  深度学习模型通常都很擅长拟合训练数据，但真正的挑战在于泛化，而不是拟合。  
  
  一般的工作流程是开始时选择相对较少的层和参数，然后逐渐增加层的大小或增加新层，直到这种增加对验证损失的影响变得很小。  
  - 容量更小的模型  
    更小的网络开始过拟合的时间要晚于参考网络，而且开始过拟合之后，它的性能变差的速度也更慢。
  - 容量更大的模型
    更大的网络只过极少的轮数就开始过拟合，过拟合也更严重，其验证损失的波动也更大。  
    
* 添加权重正则化  
  简单模型比复杂模型更不容易过拟合。简单的模型是指参数分布的熵更小的模型(或参数更少的模型)。一种常见的降低过拟合的方法就是强制让模型权重只能取较小的     值，从而限制模型的复杂度，这使得权重值的分布更加规则，这种方法叫做_权重正则化_(weight regularization),其实现方法是向网络损失函数中添加与较大权重值   相关的成本(cost)。  
  
  - L1正则化(L1 regularization):添加的成本与_权重系数的绝对值_[权重的L1范数(norm)]成正比
  - L2正则化(L2 regularization):添加的成本与_权重系数的平方_(权重的L2范数)成正比。神经网络的L2正则化也叫权_重衰减_(weight decay),权重衰减与L2正则     化在数学上是完全相同的。  
  
  Keras中，添加权重正则化的方式是向层传递_权重正则化实列_(weight regularizer instance)作为关键字参数。  
  
* 添加dropout
  dropout是神经网络最有效也最常用的正则化方法之一。对某一层使用dropout,就是在训练过程中国随机将该层的一些输出特征舍弃(设置为0)。  
  dropput比率(dropout rate)是被设为0的特征所占的比列，通常在0.2~0.5范围内。
