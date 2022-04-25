# MoCo

## 基本问题

- ==**一致性**==
- ==**字典容量**==

- 为什么`MoCo`的负样本的编码器不能通过梯度累加进行求解？
  - `MoCo`设计一个队列形式的字典主要有两个目的：**保证一致性**，**保证字典足够大**。那么我们从字典大小的角度来进行切入
    - 前提:字典大小=$65536$,队列大小=$512$
    - 不能用梯度累加的原因在于如果我们对于每一个队列直接进行损失函数的计算,然后累加$\frac{65537}{512}=128$个队列的反向过程的方法的话,其本质上等于基于一个$512$大小的字典,对我们的模型进行了$128$次迭代,这时我们的字典大小显然是很小的.只有当我们的损失函数中利用到了所有的负样本的特征输出时才能说我们的模型是根据整个$65536$大小的字典进行更新的.在论文中也有论证,字典的大小直接地决定了我们的模型性能,一般而言字典越大,我们的模型性能越好
- `MoCo`是否可以采用对于每一个`Batch`的队列求出特征，并计算到损失函数中，然后再计算下一个`Batch`的队列以此类推直到达到整个字典大小的特征都归纳入了我们的损失函数，然后在此时对我们的损失函数进行梯度求解工作？
  - **不可以**。这个可以从我们的自动求导机制`AutoGrad`的原理来进行解释.如果使用上述的方法,我们的损失函数中实际上是涉及了$\frac{65536}{512}=128$个重复的编码器的`Weight`(每一个队列都由一个编码器生成),此时我们实际上是无法对我们的负样本编码器进行预期的更新的.因为我们的`AutoGrad`机制的原理决定了我们无法综合所有$128$个编码器求得一个梯度,而只能得到某一个编码器的梯度.(权重的反向传播决定了这一点,每一个权重是一个图结点,我们是无法通过`AutoGrad`机制综合全部$128$个编码器的梯度的)

## 对比学习的三种不同的字典构造

- `Memory Bank`:即建立一个字典数据库,每次在字典数据库中选取一部分样本的特征作为真正的用到对比学习中的字典,并对模型进行更新,并且更新之后用更新的模型重新计算再计算被选取出来的样本的特征并以该特征对字典数据库中存储的特征进行更新.
  - 代表作：`InstDisc`
- `End to End`:即不再考虑构造一个大的字典,而是直接使用一个`Batch`的样本作为字典,即若`Batch`为$512$那么模型负样本数目就是$511=512-1$.然后用这个512的字典来进行损失函数的计算
  - 代表作：`SimCLR`,`CMC`
- `Queque`:即MoCo使用到的字典构造方法

## 动量编码器

- 

### 损失函数:Info NCE

$$
L_q=-\log{\frac{e^{\frac{qk_+}{\sigma}}}{e^{\frac{qk_i}{\sigma}}}}
$$



## 基本架构

<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\MoCo.assets\image-20220422233423443.png" alt="image-20220422233423443" style="zoom:60%;" />

- $x^{query}与x_0^{key}$是由同一张输入图片通过各种数据增强手段产生的(如`Random Crop`,`CutOut`,`Sober`等),这两张图片被认为是正样本
- 我们的`MoCo`模型要做的就是,让$x^{query}$经过`Encoder`后的输出与$x_0^{key}$经过`Momentum Encoder`的输出尽可能接近,而与$x_1^{key},x_2^{key}\cdots x_3^{key}$有尽可能远的距离.
- 我们的`Encoder`与`Momentum Encoder在`MoCo`中使用的是相同的`Baseline`,即`ResNet50`模型.

## 对比学习常用trick

- `Momentum Encoder`
  - 动量编码器:起到对我们的字典`Encoder`进行梯度更新的作用,其与直接使用$query$的编码器的梯度进行更新的区别在于其更新公式为$Grad_{k+1}=m\theta_q^{k}+(1-m)\theta_q^{k-1}$而不是$Grad_{k+1}=\theta_q^{k}$

- `StopGradient`
  - 即不对我们的负样本编码器进行梯度求解,从而确保了一致性

- `Centering`
- `Predictproject`
  - 即对编码器的输出做了`MLP`之后进行的一个`Linear`操作

- `MultiCrop`(SwAV)
  - 目的:为了让我们的模型将视野更多地给到局部特征创建了多个不同尺寸的正样本
  - 224×224  =>  2×(160×160),4×(96×96)
- `OutPutMLP`(SimCLR)
  - 对输出的特征使用`MLP`,即$\begin{array}{l}O*W_1=MO\\MO*W_2=FinalOutPut\end{array}$
- `Clusting`(SwAV)
  - 作用:使用聚类中心矩阵$C$替代了我们的字典,因此我们的SwAV是没有负样本只有正样本的,其训练是通过基于聚类中心矩阵$C$来实现两个正样本之间的互相预测
- `QuequeDict`(MoCo)
  - 作用:在保证使用较少显卡与内存的情况下尽可能地扩展字典大小
- `MultiAugmentation`(SimCLR)
  - 作用:对图像进行更多的数据增强,并实验说明了`Crop+CuntOut`是最佳搭配组合
- `MultiPerspective`(CMC)
  - 作用:使用多个不同的传感器得到对于我们同一个图片的不同视角的画面(如景深图片,热力图片等)
- `MLP`
  - **SimCLR**<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\MoCo.assets\image-20220423102648036.png" alt="image-20220423102648036" style="zoom: 67%;" />
  - **MoCo V2**<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\MoCo.assets\image-20220423102800263.png" alt="image-20220423102800263" style="zoom:67%;" />
  - **BYOL**<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\MoCo.assets\image-20220423102847107.png" alt="image-20220423102847107" style="zoom:67%;" />


## 常见的代理任务(Pretext task)

- 个体判别(MoCo)
- 预测(CPC)

## BYOL

### 基本结构

<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\MoCo.assets\image-20220423102915162.png" alt="image-20220423102915162" style="zoom:80%;" />

### 损失函数

- MSELoss	
