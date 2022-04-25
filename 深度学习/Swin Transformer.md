# Swin Transformer

## 多尺度采样

<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Swin Transformer.assets\image-20220423130057233.png" alt="image-20220423130057233" style="zoom:80%;" />

## Relative position bias

$Attention(Q,K,V)=SoftMax(QK^T/d+B)V,其中B为M^2×M^2的矩阵,Q,K,V为M^2×d的矩阵$

## Patch Merging

- $\left(\begin{array}{c c c c}1&2&1&2\\3&4&3&4\\1&2&1&2\\3&4&3&4\end{array}\right),H×W×C\rightarrow^{空洞卷积}\begin{array}{l}\left(\begin{array}{c c}1&1\\1&1\end{array}\right)&\left(\begin{array}{c c}2&2\\2&2\end{array}\right)\\\left(\begin{array}{c c}3&3\\3&3\end{array}\right)&\left(\begin{array}{c c}4&4\\4&4\end{array}\right)\end{array},\frac{H}{2}×\frac{W}{2}×C×4\rightarrow^{concat}\frac{H}{2}×\frac{W}{2}×4C\rightarrow^{1×1卷积}\frac{H}{2}×\frac{W}{2}×2C$

## Shifted Window Based Self-Attention

### Window Based Self-Attention

<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Swin Transformer.assets\image-20220423143059120.png" alt="image-20220423143059120" style="zoom:50%;" />

- 前提:对于一张224×224×3的图片,我们使用4×4的Patch则会得到56×56×48的矩阵,再通过Linear Embedding得到56×56×C的矩阵
- 在进入Swin Transformer Block时输入为$56×56×C$,我们的Swin Transformer不像ViT一样对整个$56×56×C$的矩阵做自注意,而是将其划分为$\frac{56}{M}$个子矩阵即$\frac{56}{M}×\frac{56}{M}×C$,然后对子矩阵分别做自注意,并且由于Self-Attention是不改变输入维度的,因此输出还是$\frac{56}{M}×\frac{56}{M}×C$,此时我们按照原来划分的方式,把这些输出再拼接回$56×56×M$的矩阵
  - 具体实现方式应该是
    - 给出$56×56×C$的输入
    - 按窗口划分成$\frac{56}{M}×\frac{56}{M}$个不重叠的子窗口$M×M×C$
    - 然后将每个子窗口的前两个维度打平变成$M^2×C$的二维矩阵
    - 按照子窗口的顺序对上一步的二维矩阵进行排列,变成$56^2×C$的二维矩阵,$56^2=\frac{56^2}{M^2}×M^2$
    - 然后进行`Self-Attention`操作计算$Q,K,V$计算$QK^T$计算$Softmax(\frac{QK^T}{\sqrt{d_k}})V$
- **为什么要使用窗口?**

### Shift Window

<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Swin Transformer.assets\image-20220423143134328.png" alt="image-20220423143134328" style="zoom:50%;" />

- **为什么要使用移动窗口?**
- **如何实现移动窗口?**
- **通过MSAK来解决移动窗口后无法通过叠加Batch的方式来进行多个窗口同时计算自注意的问题**
  - <img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Swin Transformer.assets\image-20220423202645495.png" alt="image-20220423202645495" style="zoom:60%;" />
  - 这里的`Mask Window0-3`就是用来在计算`window0-3`的$QK^T$之后用来对其结果进行掩码操作的,不同的`window`由不同的`mask Window`进行掩码操作.
    - <img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Swin Transformer.assets\image-20220423201905691.png" alt="image-20220423201905691" style="zoom:50%;" />
  - **注意**:在得到`Window0-3`的`Self-Attention`矩阵后,我们不能直接进行拼接操作,而是要对每一个矩阵做转置后将`Window0`与`Window3`互换,`Window1`与`Window2`互换才是我们最终得到的$56×56×C$的输出
  - ==**具体为什么mask矩阵是这样我们放图在最后**==
  - ==黑色部分全为0，黄色部分全为-100==

## 基本结构

<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Swin Transformer.assets\image-20220423132420618.png" alt="image-20220423132420618" style="zoom:150%;" />

## Swin Transformer的不同大小模型

![image-20220423204221554](C:\Users\Administrator\Desktop\Typora文档\深度学习\Swin Transformer.assets\image-20220423204221554.png)

## 这是Window1的mask矩阵的成因

<img src="C:\Users\Administrator\Desktop\Typora文档\深度学习\Swin Transformer.assets\绘图1-16507204571941.jpg" alt="绘图1" style="zoom:80%;" />