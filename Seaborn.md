#  Seaborn

## 整体风格设置

> **前提**:``seaborn``实际上是一个基于``matplotlib``库的操作的封装,因此我们的``seaborn``中的操作实际上等价于一些``matplotlib``操作的集合

### 五种整体风格

> darkgrid,whitegrid,dark,white,ticks

- **基本语法**：`sns.set_style('<style>')`

- ```python
  sns.set_style("darkgrid")
  ```

### 常用方法

- `sns.set()`:

- `sns.despine()`:用于去掉图的上方与右方的坐标轴
- `sns.despine(offset=<int>)`:用于设置画图与`x,y轴`的距离
- `sns.despine(<left/right/top/bottom>=True)`:用于指定将指定的坐标轴去掉
- `sns.set_context(<paper/talk/poster/notebook>,font_scale=<int>)`
  - ``<paper/talk/poster/notebook>``:用于指定不同的绘图大小风格.在不同的绘图大小风格下我们即便设置同样的绘图尺寸得到的也会是不同大小的绘图
  - `font_scale`:用于设置坐标轴刻度数字的大小

### 使用`with`的风格作用域

```python
#这里就会使用darkgrid风格绘图
with sns.axes_style("darkgrid"):
	plt.subplot(211)
	sinplot()
#这里会使用默认风格进行绘图
plt.subplot(212)
sinplot()
```

## 风格细节设置

## 调色板(``<colorObject>``)

- `<colorObject> = sns.color_palette()`:用于进行绘图颜色的实例化,我们传入各种`matplotlib`支持的绘图颜色的`RGB`等表示,其返回给我们一个`<coloObject>`对象,其可以作为`set_paltte()`函数的输入,用于改变我们后续的绘图颜色
  - :one:`<colorObject> = sns.color_palette("hls",<int>)`:将`HLS`颜色空间均匀地分为`<int>`个颜色块,并返回这`<int>`个颜色块的`<coloObject>`
  - :two:`sns.color_palatte("Paried",<int>)`:效果也是在颜色空间中获取`<int>`个颜色,但是其特点是,生成`<int>`个颜色并且**这些颜色两个为一对(同一对颜色类似,不同对颜色差异大)**
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508122124061.png" alt="image-20220508122124061" style="zoom:50%;" />
  - :three:`<coloObject> = sns.color_palette("cubehelix",<int>)`:返回`<int>`个颜色组成的`coloObject`并且这些颜色的亮度,饱和度等等性质都是线性变化的
  - :four:`<coloObject> = sns.color_palatte(<"Blues">,<int>)`:默认返回由六个颜色组成的`<coloObject>`颜色按照我们的指定的颜色从左到右渐变加深,我们可以通过在指定颜色的后方添加``"_r"``来让颜色变为从左到右依次减淡
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508122751407.png" alt="image-20220508122751407" style="zoom:50%;" /><img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508122841927.png" alt="image-20220508122841927" style="zoom:50%;" />
- `sns.set_palette(<colorObject>)`:用于将`<coloObject>`激活使用
- `sns.palplot(<colorObject>)`:用于绘制颜色图
  - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508121250857.png" alt="image-20220508121250857" style="zoom: 33%;" />

- `<colorObject> = sns.hls_palette(<int>,l=,s=)`:基本用途与上一个方法一致,只不过其支持使用`l,s`参数对亮度与饱和度进行指定

  - `l`:$[0,1]$,指定亮度
  - `s`:$[0,1]$,指定饱和度

- ``<coloObject> = sns.xckd_rgb[<'IndexName'>]`:`sns.xckd_rgb`是一个字典,其保存了`matplotlib`支持的颜色共$954$个,我们可以通过索引这个字典,获取到某一个颜色在`matplotlib`中的`colorObject`.

  ```python
  sns.xckd_rgb["pale red"]
  sns.xckd_rgb["medium green"]
  ```

- `<coloObject> = sns.cubehelix_palette(<int>,start=<float>,rot=<float>)`:与上面类似`rot,start`用于指定颜色变换的区间

- `<coloObject> = sns.light_palette("<colorName>",<int>,reverse=<boolean>)`

- `<coloObject> = sns.dark_palette("<colorName>",<int>,reverse=<boolean>)`

****

## `distplot()`直方图

- `sns.distplot(<data>,ked=<boolean>,bins=<int>,hist=<boolean>,rug=<boolean>,axlabel=<string>)`

  ```python
  x = np.random.normal(size=(100,1))
  colorObject = sns.color_palette("Blues_r")
  sns.set_palette(colorObject)
  sns.distplot(x,kde=True,bins=10,hist=True,rug=False)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508141412279.png" alt="image-20220508141412279" style="zoom:61.8%;" />

****

## `jointplot()`直方散点图

- `sns.jointplot(x="<Column-1-Name>",y="Column-2-Name",data=<DataFrameObject>,color="<NameColor>",kind=<scatter|kde|hist|hex|reg|resid>)`

  ```python
  import seaborn as sns
  import matplotlib as mlp
  import matplotlib.pyplot as plt
  import numpy as np
  import pandas as pd
  %matplotlib inline
  colorObject = sns.color_palette('hls',3)
  x = np.random.normal(size=(50,1))
  y = np.random.normal(size=(50,1))
  data = np.hstack((x,y))
  data = pd.DataFrame(data,columns=["x","y"])
  sns.set_palette(colorObject)
  sns.jointplot(x="x",y="y",data=data)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508140237689.png" alt="image-20220508140237689" style="zoom: 61.8%;" />

****

## `pairplot()`变量相关分析图

> `注意`:目前为止的所有`seaborn`绘图函数只有这个函数的返回值不是`matplotlib AxiesObject`

- `sns.pairplot(<data>,kind=<scatter|kde|hist|reg>,diag_kind=<auto|kde|hist|None>,markers="o",height=<float>,aspect=<float>)`

  - `kind`:指定非对角线的图的绘制类型
  - `diag_kind`:指定对角线的图的绘制类型
  - `markers`:指定散点的类型
  - `height`:指定每个图的高度
  - `aspect`:指定每个图的宽度

- ```python
  x = np.random.normal(loc=2,scale=1.5,size=(100,1))
  y = np.random.normal(loc=4,scale=1.0,size=(100,1))
  data = np.hstack((x,y))
  data = pd.DataFrame(data,columns=["x","y"])
  sns.pairplot(data,diag_kind='kde',markers="*",height=4,aspect=2)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508171520019.png" alt="image-20220508171520019" style="zoom:40%;" />

****

## `regplot()`回归图

- `sns.regplot(x="<X-column-Name>",y="<Y-column-Name>",data=<DataFrameObject>,scatter=<boolean>,fit_reg=<boolean>,marker="<MarkStyleName>",x_estimator=<CallbleFunction>,x_ci=<int>[0,100],ci=<int>[0,100],x_jitter=<float>,color=,palette=`

  - `fit_reg`:用于指定是否呈现回归直线
  - `marker`:用于指定散点的样式
  - `x_estimator`:用于自定义回归直线方程
  - `x_jitter`:用于自动个每个$x$值加入指定幅度的扰动
  - `x_ci`:
  - `ci`:用于规定计算回归置信区间的$1-\alpha$%显著性水平值

- ```python
  x = np.random.normal(loc=2,scale=1.5,size=(100,1))
  y = np.random.normal(loc=4,scale=1.0,size=(100,1))
  data = np.hstack((x,y))
  data = pd.DataFrame(data,columns=["x","y"])
  sns.regplot(x="x",y="y",data=data,fit_reg=True,marker="*",ci=95)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508170452541.png" alt="image-20220508170452541" style="zoom:61.8%;" />

****

## `stripplot()`离散`x`取值图

- `sns.stripplot(x="<X-column-Name>",y="<Y-column-Name>",data=<DataFrameObject>,jitter=<boolean>,orient=<v|h>,color=,palette=,size=<float>,edgecolor=,linewidth=<float>,hue=<String_Of_DataFrame>)`

  - `jitter`:用于规定是否给`x`值加扰动

  - `orient`:用于指定绘图是横着画还是竖着画

    - ```python
      sns.stripplot(x="y",y="x",data=data,jitter=False,linewidth=2,edgecolor='red',size=10,color='gray',orient="h")
      ```

      <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508211416697.png" alt="image-20220508211416697" style="zoom:50%;" />

      

    - 注意:如果我们在更换`x,y`之后不对`orient`进行调整就会出现下面的情况

      ```python
      sns.stripplot(x="y",y="x",data=data,jitter=False,linewidth=2,edgecolor='red',size=10,color='gray')
      ```

      <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508211608150.png" alt="image-20220508211608150" style="zoom:50%;" />

  - `color`:用于指定绘图的色调

  - `palatte`:用于接收`colorObject`

  - `size`:用于指定点的大小

  - `edgecolor`:用于指定点的边线的颜色

  - `linewidth`:用于指定点的边线的厚度

  - `hue`:接收一个我们传入的`DataFrame`中的某一个列的名字,指定之后,对于我们绘制的点而言,如果点对应的`hue`的这一列属于不同类别,他们就会具备不同颜色

  - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508175354126.png" alt="image-20220508175354126" style="zoom:33%;" />

- ```python
  x = np.array([1,2,3,4,5,1,5,2,3,4,2,5,3,4,1,1,1,5,2,2,4,4,5,3,2,1,4,2,3,4,1,2,3,5,4,1,2,3,5,4,2,5,5,5,5,4,4,4,4,1])
  print(x.shape)
  x = x.reshape((50,1))
  y = np.random.normal(size=(50,1))
  data = np.hstack((x,y))
  data = pd.DataFrame(data,columns=["x","y"])
  sns.stripplot(x="x",y="y",data=data,jitter=False,linewidth=2,edgecolor='red',size=10,color='gray')
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508172647079.png" alt="image-20220508172647079" style="zoom:61.8%;" />

****

## `swarmplot()`离散`x`取值图

- `sns.swarmplot(x="<X-column-Name>",y="<Y-column-Name>",data=<DataFrameObject>,orient=<v|h>,color=,palette=,size=<float>,edgecolor=,linewidth=<float>,hue=<String_Of_DataFrame>)`

- ```python
  x = np.array([1,2,1,1,1,1,1,1,1,1,1,1,1,4,1,1,1,1,1,1,1,1,1,1,2,1,4,2,3,4,1,2,3,5,4,1,2,3,5,4,2,5,5,5,5,4,4,4,4,1])
  print(x.shape)
  x = x.reshape((50,1))
  y = np.random.normal(size=(50,1))
  data = np.hstack((x,y))
  data = pd.DataFrame(data,columns=["x","y"])
  sns.swarmplot(x="x",y="y",data=data)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508174704378.png" alt="image-20220508174704378" style="zoom:80%;" />

****

## `boxplot()`离散`x`取值箱线图

- `sns.boxplot(x="<X-column-Name>",y="<Y-column-Name>",data=<DataFrameObject>,hue=<String_Of_DataFrame>,color=,palette=,width=<float>,dodge=<boolean>,whis=<float>,hue_order=<list>,fliersize=<float>,linewidth=<float>,order=<list>)`

  - `width`:指定箱子的宽度
  - `dodge`:指定当给出了`hue`参数时,不同`hue`类别是竖直排列还是并列排列
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508194119933.png" alt="image-20220508194119933" style="zoom: 50%;" />
  - `whis`:用于指定分位数,取$\alpha$则表示$\alpha$%作为最小分位数,$(1-\alpha)$%为最大分位数
  - `order`:用于指定要使用`x`列的那些值进入绘图
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508195053811.png" alt="image-20220508195053811" style="zoom:50%;" />
  - `hue_order`:用于指定要匹配`hue`对应的列的那些取值作为绘图使用
  - `linewidth`:指定图中的线的厚度
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508194435623.png" alt="image-20220508194435623" style="zoom:50%;" />
  - `fliersize`:用于指定离群点的大小

- ```python
  x = np.array([1,2,1,1,1,1,1,1,1,1,1,1,1,4,1,1,1,1,1,1,1,1,1,1,2,1,4,2,3,4,1,2,3,5,4,1,2,3,5,4,2,5,5,5,5,4,4,4,4,1])
  hue = np.array([1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,1,1,1,1,1,1,2,2,2,2,2,1,1,1,1,2,2,2,1,1,1,1,1,2,2,2,2,1,1,1,2,2,2,1])
  print(hue.shape)
  x = x.reshape((50,1))
  hue = hue.reshape((50,1))
  y = np.random.normal(size=(50,1))
  data = np.hstack((x,y,hue))
  data = pd.DataFrame(data,columns=["x","y","hue"])
  colorObject = sns.color_palette("YlGn_r")
  sns.boxplot(x="x",y="y",data=data,orient="v",dodge=True,hue="hue",hue_order=[1,2],palette=colorObject)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508194057815.png" alt="image-20220508194057815" style="zoom:80%;" />

****

## `violinplot()`离散`x`取值小提琴图

- `sns.violinplot(x="<X-column-Name>",y="<Y-column-Name>",data=<DataFrameObject>,hue=<String_Of_DataFrame>,color=,palette=,width=<float>,dodge=<boolean>,hue_order=<list>,linewidth=<float>,bw=<scott|silverman|<float>>,cut=<float>,scala=<area|count|width>,scale_hue=<boolean>,inner=<box|quartile|point|stick|None>,split=<boolean>)`

  - `split`:当使用了`hue`参数时,可以让我们对称的小提琴图左边绘制一个`hue`类别的分布右边绘制另一个`hue`类别的分布
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508202309742.png" alt="image-20220508202309742" style="zoom:50%;" />
  - `dodge`:与`boxplot`的效果相同,只不过应在`split=False`时使用
  - `bw`:
  - `cut`:
  - `scale`:
  - `scale_hue`:用于规定是否对使用了`hue`的情况动用`scale`属性的设置
  - `inner`:用于指定小提琴图内部的图的类型
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508203456757.png" alt="image-20220508203456757" style="zoom:50%;" /><img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508203850240.png" alt="image-20220508203850240" style="zoom:50%;" />

- ```python
  x = np.array([1,2,1,1,1,1,1,1,1,1,1,1,1,4,1,1,1,1,1,1,1,1,1,1,2,1,4,2,3,4,1,2,3,5,4,1,2,3,5,4,2,5,5,5,5,4,4,4,4,1])
  x = x.reshape((50,1))
  y = np.random.normal(size=(50,1))
  data = np.hstack((x,y))
  data = pd.DataFrame(data,columns=["x","y"])
  colorObject = sns.color_palette("Blues_r")
  with sns.axes_style("darkgrid"):
      sns.violinplot(x="x",y="y",data=data,orient="v",palette=colorObject,split=False)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508202335522.png" alt="image-20220508202335522" style="zoom:80%;" />

****

## `barplot`条形图

- `sns.barplot(x="<X-column-Name>",y="<Y-column-Name>",data=<DataFrameObject>,hue=<String_Of_DataFrame>,color=,palette=,width=<float>,hue_order=<list>,ci=<int>[1,100],n_boot=<int>,errcolor=,errwidth=<float>,capsize=<float|boolean>,dodge=<boolean>,estimator=<CallableFunction>)`

  - `width`:用于指定条形的宽度
  - `errwidth`:用于规定置信直线的厚度
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508204906287.png" alt="image-20220508204906287" style="zoom: 50%;" />
  - `errcolor`:用于指定置信直线的颜色
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508204953190.png" alt="image-20220508204953190" style="zoom:50%;" />
  - `capsize`:用于指定是否给出置信区间两端的直线,或者直线的长度
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508205150114.png" alt="image-20220508205150114" style="zoom:50%;" />
  - `ci`:用于规定计算回归置信区间的$1-\alpha$%显著性水平值
  - `dodge`:指定当给出了`hue`参数时,不同`hue`类别是竖直排列还是并列排列
  - `n_boot`:计算置信区间时使用的迭代次数,理论上次数越大置信区间越准确
  - `estimator`:用于计算每个离散`x`值对应的`y`值的函数,默认情况下为``均值函数``

- ```python
  x = np.array([1,2,1,1,1,1,1,1,1,1,1,1,1,4,1,1,1,1,1,1,1,1,1,1,2,1,4,2,3,4,1,2,3,5,4,1,2,3,5,4,2,5,5,5,5,4,4,4,4,1])
  hue = np.array([1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,1,1,1,1,1,1,2,2,2,2,2,1,1,1,1,2,2,2,1,1,1,1,1,2,2,2,2,1,1,1,2,2,2,1])
  print(hue.shape)
  x = x.reshape((50,1))
  hue = hue.reshape((50,1))
  y = np.random.normal(size=(50,1))
  y = np.abs(y)
  data = np.hstack((x,y,hue))
  data = pd.DataFrame(data,columns=["x","y","hue"])
  colorObject = sns.color_palette("YlGn_r")
  sns.barplot(x="x",y="y",data=data,palette=colorObject)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508204818490.png" alt="image-20220508204818490" style="zoom:80%;" />

****

## `pointplot`点图

- `sns.pointplot(x="<X-column-Name>",y="<Y-column-Name>",data=<DataFrameObject>,hue=<String_Of_DataFrame>,color=,palette=,hue_order=<list>,ci=<int>[1,100],n_boot=<int>,errwidth=<float>,capsize=<float|boolean>,dodge=<boolean>,estimator=<CallableFunction>,markers=<string|listOfString>,linestyles=<string|listOfString>,join=<boolean>,scale=<float>,orient="v|h")`

  - `markers`:用于指定散点的样式
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508212244293.png" alt="image-20220508212244293" style="zoom: 67%;" />
  - `linestyle`:用于指定连线的样式
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508212550865.png" alt="image-20220508212550865" style="zoom:67%;" />
  - `join`:用于指定是否连线
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508212443911.png" alt="image-20220508212443911" style="zoom:67%;" />
  - `scale`:

- ```python
  x = np.array([1,2,1,1,1,1,1,1,1,1,1,1,1,4,1,1,1,1,1,1,1,1,1,1,2,1,4,2,3,4,1,2,3,5,4,1,2,3,5,4,2,5,5,5,5,4,4,4,4,1])
  hue = np.array([1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,1,1,1,1,1,1,2,2,2,2,2,1,1,1,1,2,2,2,1,1,1,1,1,2,2,2,2,1,1,1,2,2,2,1])
  print(hue.shape)
  x = x.reshape((50,1))
  hue = hue.reshape((50,1))
  y = np.random.normal(size=(50,1))
  y = np.abs(y)
  data = np.hstack((x,y,hue))
  data = pd.DataFrame(data,columns=["x","y","hue"])
  colorObject = sns.color_palette("hls",8)
  sns.pointplot(x="x",y="y",data=data,palette=colorObject,hue="hue")
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508210718730.png" alt="image-20220508210718730" style="zoom:80%;" />

****

## `factorplot()`

****

## `FacetGrid()`	

****

## `heatmap()`热度图

- `sns.heatmap(data=<data>,vmin=<float>,vmax=<float>,cmap=<colorObject|>,center=<float>,annot=<boolean|2-D Dataset>,robust=<boolean>,fmt=".2g",annot_kws,linewidth=<float>,linecolor,cbar=<boolean>,cbar_kws=,cbar_ax=,square=<boolean>,xtickslabel=<auto|boolean|list|int>,ytickslabel=<auto|boolean|list|int>,mask=<BooleanArray|BooleanDataFrame>,)`

  - `vmin,vmax`:用于指定右边颜色条的最大值与最小值
  - `robust`:用于`vmin,vmax`没有指定时,其可以让我们的的颜色条的上下限以一种合适的方式被计算得到
  - `annot`:当为`True`时便会将我们输入给``data``参数的2-D矩阵的值也展示出来,当为`2-D Dataset`时就会按照给出的数据展示
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508230508890.png" alt="image-20220508230508890" style="zoom:67%;" /><img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508230641225.png" alt="image-20220508230641225" style="zoom:67%;" />
  - `fmt`:用于指定数字的展示形式,具体设置可以参考`C语言`的``printf``函数使用的输入输出格式符
    - ​                     左`.1g`                                               右`.3g`
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508231119128.png" alt="image-20220508231119128" style="zoom:67%;" /><img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508231134918.png" alt="image-20220508231134918" style="zoom:67%;" />
  - `annot_kws`:
  - `linewidth`:用于指定分隔线的厚度
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508231443206.png" alt="image-20220508231443206" style="zoom: 67%;" />
  - `linecolor`:用于指定分隔线颜色
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508231518344.png" alt="image-20220508231518344" style="zoom:67%;" />
  - `cbar`:用于控制是否展示颜色条
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508231635445.png" alt="image-20220508231635445" style="zoom:67%;" />
  - `cbar_kws`:
  - `cbar_ax`:
  - `square`:
  - `xtickslabel,ytickslabel`:用于指定横纵坐标的标识
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508232155484.png" alt="image-20220508232155484" style="zoom:67%;" />
  - `mask`:用于指定哪些格子展示颜色,哪些不展示
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508232405012.png" alt="image-20220508232405012" style="zoom:67%;" />

- ```python
  data2D = np.random.normal(size=(5,5))
  data2D = pd.DataFrame(data2D,columns=['A','B','C','D','E'],index=['R','T','Y','U','I'])
  ax2 = sns.heatmap(data2D)
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508225846268.png" alt="image-20220508225846268" style="zoom:80%;" />

****

## `orient`属性的作用

- `orient`:用于指定绘图是横着画还是竖着画

  ```
  sns.stripplot(x="y",y="x",data=data,jitter=False,linewidth=2,edgecolor='red',size=10,color='gray',orient="h")
  ```

  

<img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508211416697.png" alt="image-20220508211416697" style="zoom: 67%;" />



- ==注意==:如果我们在更换`x,y`之后不对`orient`进行调整就会出现下面的情况

  ```python
  sns.stripplot(x="y",y="x",data=data,jitter=False,linewidth=2,edgecolor='red',size=10,color='gray')
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\Seaborn.assets\image-20220508211608150.png" alt="image-20220508211608150" style="zoom: 67%;" />







