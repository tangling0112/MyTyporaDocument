# [Visdom使用](https://github.com/fossasia/visdom#visimage)

## Visdom的类方法

### 基本使用步骤

- `pip install visdom`安装`visdom`库
- 在命令行通过`python -m visdom.server`来启动`visdom`
- 通过`from visdom import Visdom`来实例化一个`Visdom`类,其充当`visdom`网页上展示的一个窗口,如果我们有多个不同图形要绘制,那么就可以实例化多个`Visdom`类.

****

### `image`方法

- `img`:接收一个`C×H×W`的`Pytorch`张量或者`numpy`图像数组

- `win`:其一般传入一个``Visdom`对象,我们随后的操作就会在这个指定的窗口上进行.但当其设置为`image_history`时,只要我们的`store_history=True`,那么我们多次调用同一个函数生成的图片都会展示在同一个窗口中,如果不设置`win`参数那么就会每调用一次生成一个图像窗口

  - `'image_history'`:
  - `'latex_support'`:

- `opts`:接收一个字典,字典内具备以下自定义键

  - `title`:用于定义**窗口**的标题
  - `caption`:用于定义**图片**的标题
  - `jpgquality`:`0-100`,用于定义`jpg`的质量,如果不定义则使用过`Png`
  - <img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image_61.png" alt="image_61" style="zoom: 80%;" />

- 实例

  ```python
  from visdom import Visdom
  import numpy as np
  viz = Visdom()
  viz.image(
          np.random.rand(3, 196, 196),
          win='image_history',
          opts=dict(title = '这个是title',caption='这个是caption', store_history=True),
      )
  ```

****

### `Images`方法

- `img`:接收一个`N×C×H×W=(批量数,通道数,高度,宽度)`的`Pytorch`张量或者`numpy`数组
- `win`:其一般传入一个``Visdom`对象,我们随后的操作就会在这个指定的窗口上进行.但当其设置为`image_history`时,只要我们的`store_history=True`,那么我们多次调用同一个函数生成的图片都会展示在同一个窗口中,如果不设置`win`参数那么就会每调用一次生成一个图像窗口
- `opts`:接收一个字典,字典内具备以下自定义键
  - `title`:用于定义**窗口**的标题
  - `caption`:用于定义**图片**的标题
  - `jpgquality`:`0-100`,用于定义`jpg`的质量,如果不定义则使用过`Png`
  - <img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220428195701942.png" alt="image-20220428195701942" style="zoom: 67%;" />

****

### `text`方法

- 作用:可以在指定窗口内展示内容,可以是普通文本,也可以是`HTML5`,其会正确解析该`HTML5`
- <img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220428213401286.png" alt="image-20220428213401286" style="zoom: 50%;" /><img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220429114507178.png" alt="image-20220429114507178" style="zoom: 67%;" />

---

### `scatter`方法

- 作用:绘制 2D 或 3D 散点图
- `X`:一个`[N,2]`或`[N,3]`的`tensor`张量或`numpy`数组,用于指定散点的$x,y$或$x,y,z$轴的值
- `Y`:一个全部数值为整数且均大于等于$1$的一维`tensor`张量或`numpy`数组,用于指定对应的散点归属于哪个类别
  - ==**注意**:`X,Y`可以分别是`N×2`的张量或数组,此时就直接按照两类来创建==
  - ==我们还可以有有`Z,T`等等,这样就可以一次创建多类原点==
- `env`:用于指定绘画的`enviroment`,默认情况下为`main`,我们可以修改它为任意字符串,如果对应的命名的`enviroment`不存在,则会创建一个新的`enviroment`
- `win`:用于指定我们要绘制图形的窗口的名称,可以是`字符串`,也可以是实例化的`Visdom`类

- `opts`:接收一个字典,字典内具备以下自定义键

  - `legend`:一个列表,用于指定不同的散点的标签名
  - `xtickmin`:用于指定$x$轴的最小值
  - `xtickmax`:用于指定$x$轴的最大值
  - `xtickstep`:用于指定$x$轴每个数值之间的间隔大小
  - `xtickvals`:一维`List/Tensor/array`用于指定,$x$轴的取值,如[0,0.75,1.6,2.5]<img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220429114251751.png" alt="image-20220429114251751" style="zoom: 50%;" />
  - `ytickmin`:用于指定$y$轴的最小值
  - `ytickmax`:用于指定$y$轴的最大值
  - `ytickstep`:用于指定$y$轴每个数值之间的间隔大小
  - `ytickvals`:同`xtickvals`
  - `ztickmin`:用于指定$z$轴的最小值
  - `ztickmax`:用于指定$z$轴的最大值
  - `ztickstep`:用于指定$z$轴每个数值之间的间隔大小
  - `ztickvals`:同`xtickvals`
  - `webgl`:`Boolean`用于指定是否使用`WebGL`来绘图,当具有大量散点时设置为`True`可以加快速度
  - `markersymbol`:用于指定散点的样式,默认为`dot`,即使用原点作为每个散点.
  - `markersize`:用于指定散点的大小,默认为`10`
  - `markercolor`:是具有整数值的张量。张量的大小可以是`N`or`N x 3`或`K`或`K x 3`.
    - `N`：每个数据点的单个强度值。0 = 黑色，255 = 红色
    - `N x 3`：每个数据点的红色、绿色和蓝色强度。0,0,0 = 黑色，255,255,255 = 白色
    - `K`和`K x 3`：不是每个数据点都有唯一的颜色，而是为特定标签的所有点共享相同的颜色。
  - `textlabels`:一个含有`N`个元素列表,用于给每个点定义文本标签
  - 

- ```python
  Y = np.random.rand(100)
      old_scatter = viz.scatter(
          X=np.random.rand(100, 2),
          Y=(Y[Y > 0] + 1.5).astype(int),
          opts=dict(
              legend=['Didnt', 'Update'],
              xtickmin=-50,
              xtickmax=50,
              xtickstep=0.5,
              ytickmin=-50,
              ytickmax=50,
              ytickstep=0.5,
              markersymbol='cross-thin-open',
          ),
      )
  ```

- <img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220428203613039.png" alt="image-20220428203613039" style="zoom:50%;" />

- 扩展

  - `win`:用于指定我们要绘制图形的窗口的名称,可以是字符串,也可以是实例化的`Visdom`类
  - ==`name`:用于给指定窗口添加新散点时指定新添加的散点的名字,一般用于$X,Y$都为`N×1`张量或数组时==
  - `update`:`new/append/replace/remove`
    - `new`
      - 当我们此时指定的`name`参数在指定窗口中已经重名了,那么就相当于我们的函数失效,即不会增加任何散点
      - 当我们此时指定的`name`参数在指定窗口中没有重名,那么就会以我们指定的`name`来进行添加
    - `append`
      - 当我们此时指定的`name`参数在指定窗口中已经重名了,那么就会直接添加到该重名的绘制下
      - 当我们此时指定的`name`参数在指定窗口中没有重名,那么就会以我们指定的`name`来进行添加
    - `replace`

****

### `update_window_opts`方法

- 用于更新指定窗口的设置

- `win`:用于指定我们要绘制图形的窗口的名称,可以是`字符串`,也可以是实例化的`Visdom`类

- `opts`:接收一个字典,针对不同的修改对象字典内具备不同的自定义键

- ```python
  viz.update_window_opts(
          win=old_scatter,
          opts=dict(
              legend=['Apples', 'Pears'],
              xtickmin=0,
              xtickmax=1,
              xtickstep=0.5,
              ytickmin=0,
              ytickmax=1,
              ytickstep=0.5,
              markersymbol='cross-thin-open',
          ),
      )
  ```

- <img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220428203640304.png" alt="image-20220428203640304" style="zoom:50%;" />

****

### `line`方法

- 用于绘制折线图

- `X`:一个`N×m`个元素的`m`维`Tensor`或`numpy`数组,用于指定$x$轴上的值

- `Y`:一个`N×m`个元素的`m`维`Tensor`或`numpy`数组,用于指定$y$轴上的值

  - 这里的$x,y$会在对应的维度上对应起来,因此这里的$X,Y$数组可以产生`m`条折线

- `env`:用于指定绘画的`enviroment`,默认情况下为`main`,我们可以修改它为任意字符串,如果对应的命名的`enviroment`不存在,则会创建一个新的`enviroment`

- `win`:用于指定我们要绘制图形的窗口的名称,可以是字符串,也可以是实例化的`Visdom`类

- `name`:用于指定绘制的跟踪名,例如当我们后续使用`update=append`时,如果`name`和以前绘制的跟踪名一致,就会继续绘制那条有一致跟踪名的`折线`,而不会绘制一条新`折线`

- `update`:用于指定绘制模式

- `opts`

  - `showlegend`:`Boolean`,
  - `legend`:`M`个元素的列表,用于给每一条直线指定其标签名
  - `title`:`string`用于指定折线图的标题
  - `xlabel`:`string`,用于指定$x$轴的标签
  - `ylabel`:`string`,用于指定$y$轴的标签
  - `ytype`:`string`,可以选择为`log`,此时$y$轴就是对数坐标轴
  - `markers`:`Boolean`,`default=false`,用于指定是否标出每一个样本点
  - `markersymbol`:`string`,`default='dot'`,用于指定用什么来标志来标记样本点
  - `markersize`:`number`,`default=10`,用于指定标记样本点的图案的大小
  - `linecolor`:`M×1`或`M×3`的`Tensor`张量或`numpy`数组,用于指定不同线条的颜色
  - `webgl`:`Boolean`,用于指定是否使用`WebGL`进行绘制,绘制复杂情况下,设置为`True`可以提高速度
  - `fillarea`:`Boolean`,用于指定是否给折线的下方做填充
    - <img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220429112804594.png" alt="image-20220429112804594" style="zoom: 25%;" /><img src="C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220429112818704.png" alt="image-20220429112818704" style="zoom: 25%;" />

  - `dash`:
  - `width`:指定窗口宽度
  - `height`:指定窗口高度
  - `marginleft`:指定折线图离窗口左端的距离
  - `marginright`:指定折线图离窗口右端的距离
  - `margintop`:指定折线图离窗口上端的距离
  - `marginbottom`:指定折线图离窗口下端的距离

- ```python
  from visdom import Visdom
  import numpy as np
  
  viz = Visdom()
  Y = np.linspace(0, 4, 200)
  viz.line(Y=np.column_stack((np.sqrt(Y), np.sqrt(Y) + 2)),
           X=np.column_stack((Y, Y)),
           env='mm',
           opts=dict(
               fillarea=False,
               showlegend=False,
               width=800,
               height=800,
               xlabel='Time',
               ylabel='Volume',
               ytype='log',
               title='Stacked area plot',
               marginleft=30,
               marginright=30,
               marginbottom=80,
               margintop=30
           ))
  
  ```

  

****

### `matplot`方法



### `audio`方法

- `audiofile`:用于指定音频文件的保存路径地址

- `tensor`:一般为`N`的一维张量,用于保存音频

- `env`:用于指定音频呈现的`enviroment`,默认情况下为`main`,我们可以修改它为任意字符串

- `win`:用于指定我们要绘制图形的窗口的名称,可以是字符串,也可以是实例化的`Visdom`类

- `opts`

  - `sample_frequency`:`>0,且为整数`用于指定音频采样的频率,例如一个`441000`的一维张量,在`441000`的采样频率下就会认为这个一维张量是一个`一秒的音频`

- ```python
  def misc_audio_basic(viz, env, args):
      tensor = np.random.uniform(-1, 1, 441000)
      viz.audio(tensor=tensor, opts={'sample_frequency': 441000}, env=env)
  
  # audio demo:
  # download from http://www.externalharddrive.com/waves/animal/dolphin.wav
  def misc_audio_download(viz, env, args):
      try:
          audio_url = 'http://www.externalharddrive.com/waves/animal/dolphin.wav'
          audiofile = os.path.join(tempfile.gettempdir(), 'dolphin.wav')
          urllib.request.urlretrieve(audio_url, audiofile)
  
          if os.path.isfile(audiofile):
              viz.audio(audiofile=audiofile, env=env)
      except BaseException:
          print('Skipped audio example')
  ```

  

****

### `video`方法

- `videofile`:存储视频的本地文件路径地址
- `tensor`:`L×C×H×W`的`Tensor`或`numpy`数组,是一个逐帧存放视频图像的张量或数组
- `env`:用于指定绘画的`enviroment`,默认情况下为`main`,我们可以修改它为任意字符串
- `win`:用于指定我们要绘制图形的窗口的名称,可以是字符串,也可以是实例化的`Visdom`类
- `opts`
  - `fps`:用于指定视频播放的帧率
  - `width`:用于指定视频的窗口宽度
  - `height`:用于指定视频的窗口高度

****

### `close`方法

****

### `win`,`name`,`env`与`update`的作用

- `env`
  - ![image-20220429112648909](C:\Users\Administrator\Desktop\Typora文档\Visdom使用.assets\image-20220429112648909.png)

## `opts`

- `opts.title` : figure title
- `opts.width` : figure width
- `opts.height` : figure height
- `opts.showlegend` : show legend (`true` or `false`)
- `opts.xtype` : type of x-axis (`'linear'` or `'log'`)
- `opts.xlabel` : label of x-axis
- `opts.xtick` : show ticks on x-axis (`boolean`)
- `opts.xtickmin` : first tick on x-axis (`number`)
- `opts.xtickmax` : last tick on x-axis (`number`)
- `opts.xtickvals` : locations of ticks on x-axis (`table` of `number`s)
- `opts.xticklabels` : ticks labels on x-axis (`table` of `string`s)
- `opts.xtickstep` : distances between ticks on x-axis (`number`)
- `opts.xtickfont` : font for x-axis labels (dict of [font information](https://plot.ly/javascript/reference/#layout-font))
- `opts.ytype` : type of y-axis (`'linear'` or `'log'`)
- `opts.ylabel` : label of y-axis
- `opts.ytick` : show ticks on y-axis (`boolean`)
- `opts.ytickmin` : first tick on y-axis (`number`)
- `opts.ytickmax` : last tick on y-axis (`number`)
- `opts.ytickvals` : locations of ticks on y-axis (`table` of `number`s)
- `opts.yticklabels` : ticks labels on y-axis (`table` of `string`s)
- `opts.ytickstep` : distances between ticks on y-axis (`number`)
- `opts.ytickfont` : font for y-axis labels (dict of [font information](https://plot.ly/javascript/reference/#layout-font))
- `opts.marginleft` : left margin (in pixels)
- `opts.marginright` : right margin (in pixels)
- `opts.margintop` : top margin (in pixels)
- `opts.marginbottom`: bottom margin (in pixels)





