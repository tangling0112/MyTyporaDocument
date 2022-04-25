# Vision Transformer

## 基本架构

![image-20220423112950673](C:\Users\Administrator\Desktop\Typora文档\深度学习\ViT.assets\image-20220423112950673.png)

## Embedding操作解读

### Image Embedding

- 前提：以$224×224×3$的图片为例(即$3$通道的$224$长宽的图片),`Patch`的大小为$16×16$,共有$(\frac{224}{16})^2=196$个Patch
- 首先每一个`Patch`有$16×16=256$个像素,共有三个通道因此我们直接用这些像素作为`Embedding`,共$16×16×3=768$个像素,因此`ImageEmbedding`的维度为$768$
- **注意**:我们这里的`Patch`是$196$个,但是实际上我们输入模型的是$197$个`Embedding`,其原因与`Bert`相同,我们在第一位添加了一个`[CLS]`标志($768$维度`Embeddding`),用于在我们执行分类任务时使用这个标识对应的向量做`Linear`从而使用`SoftMax`进行分类任务的学习

### Position Embedding

- 前提:每一张图片有$196$个`Patch`
- 我们直接使用`WordEmbedding`做出一个$196$个位置的维度为$768$的`PositionEmbedding`即可,基本类似于我们的`Transformer`中的`PositionEmbedding`,只不过`Transformer`是以单词为单位,而`ViT`则以`Patch`为单位

### Linear Projection

