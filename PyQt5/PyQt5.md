# PyQt5

## `QTDesigner`应用程序

> QTDesigner应用程序设计好的窗体可以被存储为`.ui`文件,我们通过将这个`.ui`文件转化为`.py`文件后就可以通过我们的`python`程序向我们的任意窗体应用我们定义的`Ui`样式

### `.ui`文件转换为`.py`文件

- `python -m PyQt5.uic.pyuic <path>.ui -o <path>.py`

#### `.ui`文件转换为`.py`文件后`.py`文件中的代码组成

```python
# -*- coding: utf-8 -*-

# Form implementation generated from reading ui file 'E:\PyQT实践项目\Designer\untitled.ui'
#
# Created by: PyQt5 UI code generator 5.13.2
#
# WARNING! All changes made in this file will be lost!


from PyQt5 import QtCore, QtGui, QtWidgets


class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        MainWindow.setObjectName("MainWindow")
        MainWindow.resize(800, 600)
        self.centralwidget = QtWidgets.QWidget(MainWindow)
        self.centralwidget.setObjectName("centralwidget")
        self.pushButton = QtWidgets.QPushButton(self.centralwidget)
        self.pushButton.setGeometry(QtCore.QRect(100, 80, 93, 28))
        self.pushButton.setObjectName("pushButton")
        self.toolButton = QtWidgets.QToolButton(self.centralwidget)
        self.toolButton.setGeometry(QtCore.QRect(120, 170, 47, 21))
        self.toolButton.setObjectName("toolButton")
        self.radioButton = QtWidgets.QRadioButton(self.centralwidget)
        self.radioButton.setGeometry(QtCore.QRect(100, 260, 115, 19))
        self.radioButton.setObjectName("radioButton")
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtWidgets.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 800, 26))
        self.menubar.setObjectName("menubar")
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtWidgets.QStatusBar(MainWindow)
        self.statusbar.setObjectName("statusbar")
        MainWindow.setStatusBar(self.statusbar)

        self.retranslateUi(MainWindow)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)

    def retranslateUi(self, MainWindow):
        _translate = QtCore.QCoreApplication.translate
        MainWindow.setWindowTitle(_translate("MainWindow", "MainWindow"))
        self.pushButton.setText(_translate("MainWindow", "PushButton"))
        self.toolButton.setText(_translate("MainWindow", "..."))
        self.radioButton.setText(_translate("MainWindow", "RadioButton"))

```

##### `Ui_MainWindow`类

- **作用**:一个承载了`MainWindow`窗口中所有我们使用`QtDesigner`添加的控件的`Ui代码`

- **内置方法**

  - :one:`setupUi(<WindowObject>)`:用于将其所属的类的`Ui`设置应用到我们传入的窗口对象`<WindowObject>`上

    ```python
    from PyQt5.QtWidgets import QMainWindow, QApplication
    from demo import Ui_MainWindow#from <.ui文件转化得到的.py文件名> import Ui_MainWindow
    import sys
    
    app = QApplication(sys.argv)
    MainWindow = QMainWindow()
    Ui = Ui_MainWindow()
    Ui.setupUi(MainWindow)
    MainWindow.show()
    sys.exit(app.exec())
    
    ```

### 设置快捷方式将`.ui`文件转换为`.py`文件

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519165537049.png" alt="image-20220519165537049" style="zoom:50%;" />

- `E:\Anaconda_aplication\envs\PyQT5_Practice\python.exe`
- ``-m PyQt5.uic.pyuic $FileName$ -o $FileNameWithoutExtension$.py`
- **本质**:实际上这就等价于`python -m PyQt5.uic.pyuic $FileName$ -o $FileNameWithoutExtension$.py`这个语句,只是我们使用`pycharm`的`$FilName$,$FileNameWithoutExtension$`等特殊参数实现了根据我们右键的文件直接进行语句操作

### `QTDesigner`四大布局

> **注意**:我们的`QTDesigner`中的四个布局是**可以嵌套使用的**,即我们可以将一种布局当作一个元素,加入到我们的布局方框中

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519163929604.png" alt="image-20220519163929604" style="zoom: 80%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519164401681.png" alt="image-20220519164401681" style="zoom: 67%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519170118886.png" alt="image-20220519170118886" style="zoom: 67%;" />

### `QTDesigner`绝对布局与相对布局

> 在`QTDesigner`中的容器如`frame`,当我们的``组件``放置在容器中时,位置的调整就是基于容器的左上角进行,此时就是`相对布局`,而当我们的`组件`直接放置在窗口内,那么位置的调整就是基于窗口的左上角进行,此时便是`绝对布局`

### `QTDesigner`的组件最大尺寸与最小尺寸控制

> 我们可以通过组件的`QWidget`属性下的`minmumSize`,`maxmumSize`来设置组件的最大最小尺寸.

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519210734021.png" alt="image-20220519210734021" style="zoom:67%;" />

### `QTDesigner`的组件尺寸策略

> `QTDesigner`的每个组件都有下面的两个方法
>
> ``SizeHint().<width()/height()>``:对于大部分组件而言这个方法中的``长宽``为只读属性,其中存储着**我们在`QTDesigner`设计时给该组件指定的长宽**.
>
> `minimumSizeHint()`

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519211925338.png" alt="image-20220519211925338" style="zoom: 67%;" />
- <img src="https://img-blog.csdnimg.cn/20200503233737791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p6eDE4ODg5MTAyMA==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:80%;" />

#### 水平与垂直策略

- `Fixed`:**固定值策略**,表示该组件无法随着容器大小的变化而改变大小
- `Minimum`:**最小值策略**,此时该组件的`SizeHint()`中存储的长宽被认为是组件的**最小大小**,因此组件初始化时就是**最小尺寸**,因此组件**只能**随着容器的变化而**增加长宽**,而不能随着容器变化减小长宽
- `Maximum`:**最大值策略**,此时该组件的`SizeHint()`中存储的长宽被认为是组件的**最大大小**,因此组件初始化时的尺寸就是**最大尺寸,**组件只能随着容器的变化而**减小长宽**,而不能随着容器变化**增加长宽**
- `Preferred`:**首选项策略**,此时该组件的`SizeHint()`中存储的长宽被认为是组件的**最佳大小**,组件**允许**随着容器的变化**增加,减少长宽**
- `MinimumExpanding`:**最小可扩展策略**,在此种策略下该组件的`SizeHint()`中存储的长宽为组件的**最小大小**,此时组件允许随着容器的变化而**增加长宽**,但
- `Expanding`:**扩展策略**,在此种策略下该组件的`SizeHint()`中存储的长宽为组件的**最佳大小**
- `ignored`:此种策略下,组件会尽可能获取窗口内**尽可能多的空间**

#### 水平与垂直伸展

> ``作用``:用于控制组件之间的比例``[2,1,1]``时就是第一个组件占据``50%``的容器,剩下两个组件一人占据`25%`

### `QTDesigner`伙伴设置

> `作用`:使得我们可以设置快捷键,从而当我们按下指定快捷键时,我们窗口的焦点就会跳转到该快捷键组件的伙伴上.如我们下面的例子就让我们按下`Alt+A`时跳转到`姓名`的文本输入框,`Alt+B`跳转到``年龄``的文本输入框,按下`Alt+C`跳转到`身高`的文本输入框

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519224542994.png" alt="image-20220519224542994" style="zoom:67%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519224618086.png" alt="image-20220519224618086" style="zoom:67%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519224640233.png" alt="image-20220519224640233" style="zoom:67%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519224713171.png" alt="image-20220519224713171" style="zoom:67%;" />

### `QtDesigner`修改组件之间的`Tab`顺序

> ``作用``:我们知道,对于一个窗口,我们不断地按下`Tab`可以遍历窗口中的每一个组件,而我们在`QTDesigner`中可以修改我们按`Tab`时遍历组件的顺序,我们只要在设计时切换到`Tab`顺序编辑界面即可改变

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519225055623.png" alt="image-20220519225055623" style="zoom:67%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519225123492.png" alt="image-20220519225123492" style="zoom:67%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519225222489.png" alt="image-20220519225222489" style="zoom:67%;" />

### `QTDesigner`中的信号与槽

> `信号`:当我们对一个控件进行一些操作如`点击,悬停`等时,这个控件就会产生一个信息,这个信息是可以被多个其他组件接收的.
>
> ``槽``:槽就是对于接收信号的组件的称呼
>
> 我们可以在`QTDesigner`中很容易地设置信号与槽
>
> **注意**:一个``信号``可以发射给多个``槽``,一个``槽``可以接收多个组件发射的``信号``

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519225956314.png" alt="image-20220519225956314" style="zoom:67%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519230127479.png" alt="image-20220519230127479" style="zoom:67%;" />

### `QTDesigner`的菜单栏与工具栏设计

> ``注意``:窗口的菜单栏与工具栏可以通过在我们设计时的对应窗口内右键创建与删除
>
> **菜单栏中项目的添加**:直接按照指引的`在这里输入`就可以添加项目,在创建好的`项目`上点击还能生成该项目下的子项目
>
> **工具栏中项目的添加**:添加的前提是我们菜单栏中的项目至少有一个具备子项目,这些菜单栏项目中的子项目都会出现在`QTDesigner`的`动作编辑器中`,我们直接从`动作编辑器`把这些子项目拖到我们的工具栏就可以

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519231420009.png" alt="image-20220519231420009" style="zoom: 67%;" />

- ****

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519231749029.png" alt="image-20220519231749029" style="zoom:67%;" />

- ****

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519231933106.png" alt="image-20220519231933106" style="zoom:67%;" />

- ****

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519232229755.png" alt="image-20220519232229755" style="zoom:67%;" />

- ****

- <img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220519232415834.png" alt="image-20220519232415834" style="zoom:67%;" />

****



****

## `QtWidgets`模块

### `QApplication`类

> **作用**:用于创建一个应用程序的实例,**这是我们使用`PyQt`创建窗口必须引入使用的类**

```python
from PyQt5.QtWidgets import QApplication
import sys
app = QApplication(sys.argv)
```

#### 常用方法

- `setWindowIcon(<IconObject>)`:用于设置**应用程序的图标**与应用程序**主窗口的图标**

- `instance()`:用于获取当前的应用对象并返回该对象

  - ```python
    app = QApplication.instance()
    ```

- `quit()`:退出当前应用程序

### `Qwidget`类

> 作用:创建一个窗口,其具备所有的组件,当我们不确定窗口的用途时可以使用这个类创建窗口

- `setLayout(<LayoutObject>)`:用于给窗口添加一个`LayoutObject`即给窗口添加一个布局
- `resize()`:调整窗口的大小
- `setWindowTitle()`:设置窗口的标题
- `setGeometry(<int>,<int>,<int>,<int>)`:设置窗口的位置与大小
- `move()`:将窗口的左上角移动到指定位置
- `show()`:将窗口显示出来
- `frameGeometry()`:获取窗口的基本信息
  - `width()`获取宽度
  - `height()`:获取高度(**包含标签栏**)
  - `x()`:获取窗口的横坐标
  - `y()`:获取窗口的纵坐标(**包含标签栏**)
- `geometry()`:获取当前窗口的`长宽信息对象`
  - `width()`获取宽度
  - `height()`:获取高度(**不包含标签栏**)
  - `x()`:获取窗口的横坐标
  - `y()`:获取窗口的纵坐标(**不包含标签栏**)
- `x()`:获取窗口的横坐标
- `y()`:获取窗口的纵坐标(**包含标签栏**)
- `width()`获取宽度
- `height()`:获取高度(**不包含标签栏**)
- `setCentralWidget(<WidgetObject>)`:用于将一个`WidgetObject`放置到我们的窗口中显示出来
- `sender()`:用于实例化一个信号槽,其可以接受到来自于与这个信号槽绑定的信号发射组件发出的信息
  - `text()`:获取接受到的信号中的文本信息
- `x()`:获取窗口的横坐标
- `y()`:获取窗口的纵坐标
- `statusBar()`:
  - `showMessage(<string>,<int>)`:在窗口中将文本信息呈现指定时间
- `setWindowIcon(<QIconObject>)`:给窗口设置图标
  - 注意:当我们用这个给主窗口设置图标后,`QApplication`中对于主窗口图标的设置就会被取消,但是对于应用程序的图标设置还是会生效
- `setToolTip(<string>):`设置鼠标悬浮在组件上时的提示信息

### `QDesktopWidget`类

### `QDialog`类 对话窗口

> **作用**:创建一个对话窗口,如对于我们的`Pycharm`当我们点击``文件->设置``就会弹出一个窗口,这个窗口就是一个对话窗口
>
> **特性**:对话窗口创建后,我们**只能对该应用程序的该对话窗口进行操作**,其他的任何窗口都会被冻结.只有当我们关闭对话窗口才会解除冻结

#### 具备的组件

- 标题栏
- 常用组件(`Button`等)

常用方法

- `frameGeometry()`:获取窗口的基本信息
  - `width()`获取宽度(**包含边框**)
  - `height()`:获取高度(**包含标签栏与边框**)
  - `x()`:获取窗口的横坐标(**包含边框**)
  - `y()`:获取窗口的纵坐标(**包含标签栏与边框**)
- `geometry()`:获取当前窗口的`长宽信息对象`
  - `width()`获取宽度(**不包含边框**)
  - `height()`:获取高度(**不包含标签栏与边框**)
  - `x()`:获取窗口的横坐标(**不包含边框**)
  - `y()`:获取窗口的纵坐标(**不包含标签栏与边框**)
- `x()`:获取窗口的横坐标(**包含边框**)
- `y()`:获取窗口的纵坐标(**不包含标签栏与边框**)
- `width()`获取宽度(**不包含边框**)
- `height()`:获取高度(**不包含标签栏与边框**)

### `QMainWindow`类 主窗口

> **作用**:用于创建一个==主窗口==,也即`WindowObject`,一个窗口对象
>
> ```
> from PyQt5.QtWidgets import QMainWindow
> import sys
> MainWindow = QMainWindow()
> ```

#### 具备的组件

- 菜单栏
- 工具栏
- 状态栏
- 标题栏
- 常用的基本组件(``Button`等)

#### 常用方法

- `setLayout(<LayoutObject>)`:用于给窗口添加一个`LayoutObject`即给窗口添加一个布局
- `resize()`:调整窗口的大小
- `setWindowTitle()`:设置窗口的标题
- `setGeometry(<int>,<int>,<int>,<int>)`:设置窗口的位置与大小
- `move()`:将窗口的左上角移动到指定位置
- `show()`:将窗口显示出来
- `frameGeometry()`:获取窗口的基本信息
  - `width()`获取宽度
  - `height()`:获取高度(**包含标签栏**)
  - `x()`:获取窗口的横坐标
  - `y()`:获取窗口的纵坐标(**包含标签栏**)
- `geometry()`:获取当前窗口的`长宽信息对象`
  - `width()`获取宽度
  - `height()`:获取高度(**不包含标签栏**)
  - `x()`:获取窗口的横坐标
  - `y()`:获取窗口的纵坐标(**不包含标签栏**)
- `x()`:获取窗口的横坐标
- `y()`:获取窗口的纵坐标(**包含标签栏**)
- `width()`获取宽度
- `height()`:获取高度(**不包含标签栏**)
- `setCentralWidget(<WidgetObject>)`:用于将一个`WidgetObject`放置到我们的窗口中显示出来
- `sender()`:用于实例化一个信号槽,其可以接受到来自于与这个信号槽绑定的信号发射组件发出的信息
  - `text()`:获取接受到的信号中的文本信息
- `x()`:获取窗口的横坐标
- `y()`:获取窗口的纵坐标
- `statusBar()`:
  - `showMessage(<string>,<int>)`:在窗口中将文本信息呈现指定时间
- `setWindowIcon(<QIconObject>)`:给窗口设置图标
  - 注意:当我们用这个给主窗口设置图标后,`QApplication`中对于主窗口图标的设置就会被取消,但是对于应用程序的图标设置还是会生效
- `setToolTip(<string>):`设置鼠标悬浮在组件上时的提示信息

### `QDesktopWidget`类

> **作用**：用于获取我们当前程序允许的主机的桌面的一些基本信息，如`屏幕分辨率`等等

#### 常用方法

- `screenGeometry()`:获取当前主机的``屏幕分辨率信息对象``
  - `width()`获取宽度
  - `height()`:获取高度

### `QPushButton`类

> **作用**:实例化一个按钮对象,在实例化时可以传入一个字符串作为按钮的文本内容

- ```python
  button = QPushButton("这是一个按钮")
  ```

- `click()`:指代点击操作,用于发送信号

  - `connect(<FunctionObject>)`:绑定接收信号的槽为我们的`FunctionObject`

- `setText(<string>)`:设置按钮中的文本

- `setToolTip(<string>):`设置鼠标悬浮在组件上时的提示信息

### `QRadioButton`类

### `QcheckBox`类

### `QComboBox`类

### `QLabel`类

#### 常用方法

- `setToolTip(<string>):`设置鼠标悬浮在组件上时的提示信息

- `setAlignment()`:设置文本对齐方式
  - `Qt.AlignCenter`
  - `Qt.AlignRight`
- `setIndent()`:设置文本缩进
- `text()`:获取文本内容
- `setBuddy()`:设置伙伴关系
- `setText()`:设置文本内容
- `selectedText()`:返回所选择的字符
- `setWordWrap()`:设置是否允许换行
- `setPalette(<PaletteObject>)`:用于设置文本的背景颜色
- `setAutoFillBackground(<boolean>)`:用于设定是否填充标签的背景颜色
- `setOpenExternalLinks(<boolean>)`:用于设定点击是是打开外部连接还是调用槽函数
- `setPixmap(<QPixmapObject>)`:用于使用该组件展示图片
- `setBuddy(<实例化的其他控件>)`:设置与指定控件的伙伴关系

#### 常用事件

- `linkHovered`:鼠标经过控件时触发
- `linkActivated`:鼠标点击控件时触发

### `QLineEdit`类

> 作用:创建一个一行的文本输入框

- `setPlaceholderText(<strig>)`:用于添加在没有输入文本时的那种灰色提示性文本
- `setEchoMode(<QLineEdit.Normal|QLineEdit.NoEcho|QLineEdit.Passward|QLineEdit.PasswardEchoOnEdit>)`
  - `Normal`:常规显示模式
  - `NoEcho`:输入字符不会被显示出来,但是会正确传入系统
  - `Passward`:输入的字符会被显示为一个黑圆圈,但是输入内容会被正确传入系统
  - `PasswardEchoEdit`:正在输入时会正常显示,但是当我们焦点发生转换就会自动转换显示为黑色圆圈,但是输入的内容会被正确传入系统<img src="C:\Users\Administrator\Desktop\Typora文档\PyQt5\PyQt5.assets\image-20220520135944707.png" alt="image-20220520135944707" style="zoom:50%;" />
- 

### `QTextEdit`类

### `QHBoxLayout`类

> **作用**:实例化一个水平布局

- `addWidget(<Object>)`:用于向布局中添加组件

### `QVBoxLayout`类

> **作用**:实例化一个垂直布局

- `addWidget(<Object>)`:用于向布局中添加组件

### `QGridLayout`类

> **作用**:实例化一个栅格布局

- `addWidget(<Object>)`:用于向布局中添加组件

### `QGridLayout`类

> **作用**:实例化一个栅格布局

- `addWidget(<Object>,<int>,<int>,<int>,<int>)`:用于向布局中添加组件,四个`int`用于指定控件`左上角行位置,左上角列位置,控件占用几行,控件占用几列`

### `QFormLayout`类

> **作用**:实例化一个表单布局

- `addRow(<label>,<Object>)`:用于向布局中添加组件,并给组件命名

  ```python
  formlayer.addRow("组件",<Object>)
  ```

### ==注意事项==

#### `QWidget`,`QMainWindow`与`QDialog`的显示问题

##### `QMainWindow`

- 实例化控件

  ```python
  self.button = QpushButton('这是一个按钮')
  ```

- 实例化布局

  ```python
  self.Layout = QHBoxLayout()
  ```

- 将控件添加到布局内

  ```python
  self.Layout.addWidget(self.button)
  ```

- 实例化`Qwidget`对象

  ```python
  self.Widget = QWidget()
  ```

- 将布局添加到`Qwidget`对象

  ```python
  self.Widget.setLayout(self.Layout)
  ```

- 将`QWidget`添加到我们的当前窗口内

  ```python
  self.setCentralWidget(self.Widget)
  ```

##### `QWidget`与`QDialog`

- 实例化控件

  ```python
  self.button = QpushButton('这是一个按钮')
  ```

- 实例化布局

  ```python
  self.Layout = QHBoxLayout()
  ```

- 将控件添加到布局内

  ```python
  self.Layout.addWidget(self.button)
  ```

- 将布局引用给窗体

  ```python
  self.setLayout(Layout)
  ```

  

## `QtGui`模块

## `QtGui`模块

### `QIcon`类

> **作用**:将传入的图像实例化为`PyQt5`可接受的`IconObject`

```python
Icon = QIcon(<filepath>)
```

### `QPalette`类

> 作用:创建一个调色板对象`PaletteObject`

- `setColor(,<colorObject>)`:设置背景色
  - `Qt.blue`

### `QPixmap`类

> 作用:实例化一个图片对象`ImageObject`

```python
Image = QPixmap(<filepath>)
```

### `QIntValidator`类

> 

### `QDoubleValidator`类

### `QRegExpValidator`类

## `QtCore`模块

### `Qt`类

- `Qt.AlignCenter`
- `Qt.AlignRight`
- `Qt.Blue`

### `QRegExp`类

## 构建自己的窗口

```python
import sys
from PyQt5.QtWidgets import QMainWindow, QApplication
from PyQt5.QtGui import QIcon


class MyMainWindow(QMainWindow):
    def __init__(self):
        super(MyMainWindow, self).__init__()
        self.setWindowTitle("MainWindow Of me")
        self.resize(300, 400)


app = QApplication(sys.argv)
# app.setWindowIcon(QIcon('图标路径'))
main = MyMainWindow()
main.show()
sys.exit(app.exec())
```

## 代码实现的信号与槽的绑定

```python
import sys
from PyQt5.QtWidgets import QMainWindow, QApplication,QPushButton,QHBoxLayout,QWidget
from PyQt5.QtGui import QIcon


class MyMainWindow(QMainWindow):
    def __init__(self):
        super(MyMainWindow, self).__init__()
        self.setWindowTitle("MainWindow Of me")
        self.resize(300, 400)
        self.button = QPushButton('这个是一个按钮')
        
        self.button.clicked.connect(self.On_Click)
        
        self.layout = QHBoxLayout()
        self.layout.addWidget(self.button)
        self.Widget = QWidget()
        self.Widget.setLayout(self.layout)
        self.setCentralWidget(self.Widget)
    def On_Click(self):
        print('槽接受到信号')


app = QApplication(sys.argv)
# app.setWindowIcon(QIcon('图标路径'))
main = MyMainWindow()
main.show()
sys.exit(app.exec())
```
