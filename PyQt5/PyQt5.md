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

  

## `QtWidgets`模块

### `QApplication`类

#### 基本作用

用于创建一个应用程序的实例

```python
from PyQt5.QtWidgets import QApplication
import sys
app = QApplication(sys.argv)
```

### `Qwidget`类

### `QMainWindow`类

#### 基本作用

用于创建一个==主窗口==,也即`WindowObject`,一个窗口对象

```python
from PyQt5.QtWidgets import QMainWindow
import sys
MainWindow = QMainWindow()
```

