# C++

## 第一部分

### 数据类型

#### 变量

`数据类型`  `变量名` = `变量初始值`

|         数据类型          |                  占用空间                  |    取值范围     |
| :-----------------------: | :----------------------------------------: | :-------------: |
|       short(短整型)       |                2字节,16bit                 |  -2^15~2^15-1   |
|         int(整型)         |                4字节,32bit                 |  -2^31~2^31-1   |
|       long(长整型)        | 4字节,32bit或Linux32的32bit,Linux64的64bit |  -2^31~2^31-1   |
|    long long(超长整型)    |                8字节,64bit                 |  -2^63~2^63-1   |
|    float(单精度浮点数)    |                4字节,32bit                 |   7位有效数字   |
|   double(双精度浮点数)    |                8字节,64bit                 | 15-16位有效数字 |
|        char(字符)         |                 1字节,8bit                 |    一个字符     |
|       char 变量名[]       |                  指定字节                  |   指定个字符    |
| string(需include<string>) |                                            |                 |
|     bool(true/false)      |                 1字节,8bit                 |       1/0       |

#### 常量

- `const` `数据类型` `常量名` = `常量值`
- `#define` `常量名` `常量值`
- **特性**:常量的值不可被任何操作修改 ,如果我们试图修改则会报错

### 常用计算符号

| 符号 | 作用          |
| ---- | ------------- |
| +    | 加法          |
| -    | 减法          |
| *    | 乘法          |
| /    | 除法          |
| %    | 取余数        |
| ++   | 自增          |
| --   | 自减          |
| =    | 赋值          |
| a+=b | 等价于a = a+b |
| a-=b | 等价于a = a-b |
| a*=b | 等价于a = a*b |
| a/=b | 等价于a = a/b |
| a%=b | 等价于a = a%b |
| ==   | 等于          |
| !=   | 不等于        |
| <    | 小于          |
| >    | 大于          |
| >=   | 大于等于      |
| <=   | 小于等于      |
| !    | 非            |
| &&   | 与            |
| \|\| | 或            |

### 函数的分文件编写

- 编写用于函数声明的`.h`文件
- 编写函数语句体所在的`.cpp`文件
- 使用时先#`include "h文件名.h"`
- 然后直接调用函数

### 基础知识

| 语句        | 比较    |
| ----------- | ------- |
| 条件判断    | 与C一致 |
| 循环        | 与C一致 |
| switch case | 与C一致 |
| 一维数组    | 与C一致 |
| 二维数组    | 与C一致 |
| 函数的定义  | 与C一致 |
| 函数调用    | 与C一致 |
|             |         |

#### cin/cout

- 作用
  - `cin`:获取用户输入
  - `cout`:向用户输出
- 字符
  - `>>`:输入流
  - '<<':输出流
  - `endl`:结束标志符号

### 变量,常量命名规则

- 不能是关键字
- 只能由字母,数字,下划线组成
- 区分大小写
- 必须由下划线或字母开头

#### 三目运算符

- `表达式1` ? `表达式2` : `表达式3`

  ```C++
  等价于:
  if(表达式1){
  	表达式2
  }
  else{
  	表达式3
  }
  ```

#### goto	

- 作用:跳转到指定标志处执行

```C++
goto <标志名>

<标志名>:
	语句体
```

#### include<>与include ""

#### 常用技巧

| 技巧             | 作用                                 |
| ---------------- | ------------------------------------ |
| aeb              | 等价于a×10^b                         |
| (数据类型)变量名 | 将变量的数据类型强制转换为指定的类型 |
|                  |                                      |
|                  |                                      |
|                  |                                      |
|                  |                                      |
|                  |                                      |
|                  |                                      |
|                  |                                      |

#### 常用函数

| 函数     | 作用                                        |
| -------- | ------------------------------------------- |
| sizeof() | 求得指定变量或数据类型的所占字节数,静态函数 |
|          |                                             |
|          |                                             |
|          |                                             |
|          |                                             |
|          |                                             |
|          |                                             |
|          |                                             |
|          |                                             |
