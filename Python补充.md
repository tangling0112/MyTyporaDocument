## 默认参数与动态参数

## 匿名函数

## 三元运算

## 模块与包

### 模块

- 在`Python`中我们将一个`.py`文件称之为一个模块,我们可以通过`from <模块名> import <函数名>`的方式引入该模块下的指定函数
- **注意**:我们在一个`.py`文件中使用`__add__=[<函数名>]`的方式指定该模块下哪些函数可以被我们调用.

### 包

- 在python中一个包含了大量`.py`文件的文件夹为一个包,我们可以通过`from <包名>.<模块名> import <函数名>`的方式从指定文件夹下的指定`.py`文件中导入指定的函数==(:red_circle: **注意**:我们是无法用.来导入模块内的函数的,因为`.`的最细粒度只到模块,如果想导入函数则要是`from Package.module import 函数名 `,而`import Package.module.函数名`是会报错的)==
- :red_circle: **注意**:我们自定义包时,应该在对应文件夹下定义一个`__init__.py`文件,用于给包给出一些注释,包的版本,使用`__add__=[]`定义的用于指定哪些模块可以调用的规则.值得注意的是,每当我们调用该包下的任意模块时,都会自动执行这个`__init__.py`文件.
  - ![image-20220424215433787](C:\Users\Administrator\Desktop\Typora文档\Python补充.assets\image-20220424215433787.png)![image-20220424215448928](C:\Users\Administrator\Desktop\Typora文档\Python补充.assets\image-20220424215448928.png)
  - ![image-20220424215513661](C:\Users\Administrator\Desktop\Typora文档\Python补充.assets\image-20220424215513661.png)<img src="C:\Users\Administrator\Desktop\Typora文档\Python补充.assets\image-20220424215520912.png" alt="image-20220424215520912" style="zoom: 80%;" />

### 导入路径

- 前提:当我们需要导入的包和我们当前的`.py`文件在同一目录下(或几个Python内部指定的文件夹下)时,我们使用上述导入方法是没有问题的.:red_car:但是当我们要导入的包或模块不在当前`.py`文件同目录下(或几个Python内部指定的文件夹下)时,就需要使用其他的方式进行导入

  ```python
  import sys
  for i in sys.path:
      print(i)
      
      
  输出:
  E:\Anaconda_aplication\envs\Pytorch\python38.zip
  E:\Anaconda_aplication\envs\Pytorch\DLLs
  E:\Anaconda_aplication\envs\Pytorch\lib
  E:\Anaconda_aplication\envs\Pytorch
  E:\Anaconda_aplication\envs\Pytorch\lib\site-packages
  ```

- ==通过`sys.path`来添加查找路径== 

  ```python
  import sys
  sys.path.append(<要查找的路径>)
  ```

  - :red_circle: **问题1**:Python的模块的查找是按照从sys.path的路径按照顺序进行搜索的.因此当我们定义的模块存在同名模块时,Python就会导入先搜索到的模块,因此我们在对模块,包进行命名时,要保证不和搜索路径下的模块或包重名

  - :red_circle: **问题2:*我们必须对添加的路径使用一定的处理,才能保证当我们将文件发送给别人时,包或模块的导入不会失败.

    ```python
    import sys
    sys.path.append(os.path.dirname(os.path.abspath(__file__)))
    
    ```

## from导入与import导入的粒度区别

- **from可以做到下列功能(最细粒度为函数)**
  - 导入包中的模块
  - 导入包中的包
  - 导入模块中的函数
  - 导入包中的某个模块的某个函数
- **import只能导入到模块粒度**
  - 导入包
  - 导入模块
  - 导入模块中的包
  - 导入包中的包