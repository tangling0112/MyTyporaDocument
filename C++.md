# C++

## 第一部分

### 数据类型

#### 变量

**定义方式**：`数据类型`  `变量名` = `变量初始值`

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

****

### 结构体

- 结构体的定义:`struct 结构体名 {结构体成员}`

- 创建结构体变量

  - `struct 结构体名 变量名`

  - `struct 结构体名 变量名 = {结构体成员值1,值2...}`

  - 在结构体定义时便定义好

    ```C++
    struct Student{
    	string name;
    	int age;
    	double score;
    }变量名;
    ```

#### 结构体数组

- 前提:结构体已经定义好
- 定义方式
  - `struct 结构体名 数组名[结构体个数]`
  - `struct 结构体名 数组名[结构体个数] = {{结构体1成员值(以逗号分隔)},{结构体2成员值}...}`
- 结构体数组的访问
  - `结构体数组名[结构体索引].结构体成员名`

#### 结构体指针

- 前提:结构体已经定义好
- 定义方式
  - `struct 结构体名* 结构体指针名 = &结构体变量名`
  - `struct 结构体名* 结构体指针名`   `结构体指针名=&结构体变量名`
- 通过指针访问结构体成员
  - `结构体指针名->成员名`

#### 结构体的`.`方法与`->`方法

- `.`:用于访问结构体变量的成员

  ```c++
  结构体变量名.成员名
  Student.name
  ```

- `->`:用于访问结构体指针的成员

  ```c++
  结构体指针->成员名
  strcut Student* zs
  zs->name
  ```

#### 结构体的嵌套

##### 普通结构体嵌套普通结构体

- 通过`结构体变量名.子结构体变量名.子结构体的成员名`的方式来访问子结构体的成员

```c++
struct Student{
    string name;
    int age;
    int score;
};
struct teacher{
    string name;
    int age;
    struct Student student;
};
```

##### 普通结构体嵌套指针结构体

- 通过`结构体变量名.子结构体变量名->子结构体成员名`来访问子结构体成员
- :red_circle: **注意**:当我们创建了一个指向主结构体的结构体指针时通过`结构体指针名->子结构体变量名->子结构体成员`来访问子结构体成员

```c++
struct Student{
    string name;
    int age;
    int score;
};
struct teacher{
    string name;
    int age;
    struct Student* student;
};
```

##### 结构体传参

- **传地址**
  - 形式参数应该定义为`struct 结构体名* 结构体指针名`
  - 直接传递`&结构体变量名`
  - 然后使用`形式参数名->结构体成员名`即可访问
  - :red_circle: **特性**:**在子函数中修改结构体变量的成员,主函数中对应的结构体变量的成员也会发生修改**
- **传值**
  - 形式参数定义为`struct 结构体名 形式参数名`
  - 直接传递`结构体变量名`
  - 然后使用`形式参数名.结构体成员名`即可访问
  - :small_red_triangle:**特性**:**在子函数中无论如何修改结构体变量的成员,主函数中的对应结构体变量的成员都不会被修改**

##### 结构体中使用const

- 定义方式
  - `const struct 结构体名 结构体变量名`
  - `const struct 结构体名 结构体变量名`
- 作用
  - 一般用在结构体传地址时,避免子函数内修改结构体变量的成员.这样做**既可以保证主函数的结构体变量不被修改,又可以利用传递指针来节省内存空间**

##### 值传递与地址传递

- `变量,数组,结构体`都具备`值传递`与`地址传递`两种传递参数的方式
- **值传递**
  - 形式参数定义为普通数据类型
  - **不修改**主函数内被传值的对象
- **地址传递**
  - 形式参数定义为对应的指针
  - **修改**主函数内被传地址的对象

****

### C++内存模型

#### 代码区

#### 全局区

#### 栈区

#### 堆区

### new运算符分配地址与delete释放内存

- new分配地址

  ```c++
  基本语法：数据类型* 指针名 = new 数据类型(指针指向的内存中存储的值)
  
  创建一般变量,例子如下:
  int* 指针名 = new int(10);
  string* 指针名 = new string('这是一个字符串');
  
  创建数组,例子如下:
  int* 指针名 = new int[10];构造了一个长度为10的一维数组
  ```

- delete释放内存地址

  ```C++
  基本语法:delete 指针名
  ```

****

### :yellow_heart: 静态变量

- 定义方式
  - `static 数据类型 变量名`
  - `static 数据类型 变量名 = 变量初始值`
- :blue_heart: **特性**:静态变量只有主函数被释放时才会被释放,即所有被主函数调用的函数,以及这些函数调用的函数创建的静态变量,都只会在主函数被释放时才会被释放.

### :red_circle: 引用

> **作用**:用于给变量起别名

- 语法:`数据类型 &别名 = 原名`
- **特性**:此时的别名与原名都共享同一块内存,因此通过别名进行修改,原名的值也会修改
- ==`经典作用`:可以用于函数传参时,以`&别名`作为形式参数,**这样我们的子函数就可以修改主函数中被传递的变量的值,而不用借助指针来进行操作,注意,此时传参时直接传变量名即可,不用传地址**==
- **注意**
  - 引用的创建必须严格按照语法进行
  - 引用创建后就不能改变(b应用了a,那么b就无法被修改为c的引用)

#### 引用作为函数的返回值

- **作用**:

- > **前提**
>
  > - **不要传递函数的局部变量**的引用(因为其在函数结束执行后,局部变量就会被自动释放)

- **特性**:当我们的函数返回引用时,我们的函数就可以作为赋值运算的左值

  ```C++
  func() = 1000
  会修改func()函数返回的变量的值,我们再使用别名来进行获取时,其值就会变成修改后的值
  ```

- **定义方法**

  ```C++
  int& 函数名(){
  	语句体
  	return 变量名
  }
  调用方法:
  
  int& 别名 = 函数名()
      
  例子:
  int& func(){
  	static int a = 10
  	return a
  }
  调用方法:
  int& b = func()
  ```

#### 引用的本质

- `int &别名 = 原名`等价于`int* const 别名 = &原名`
- 并且在我们使用别名时也会自动转换为`*别名`

#### 以常量形式引用

- `const 数据类型&别名 = 原名`
- **特性**:此时我们无法通过这样创建的别名来修改原名的值,要修改的话我们只能通过原名来进行.
- **等价于**`const 数据类型* const 别名 = &原名`

****

## 第二部分

### 函数的高阶操作

#### 函数传参

- 基本类型

  - 一般参数:`数据类型 形式参数名`
  - 默认参数:`int 形式参数名 = 默认值`

  - 占位参数:`数据类型`或`数据类型 = 默认值`
    - 作用:

- 注意

  - 形式参数必须从左到右先是**一般参数**,再是**默认参数**,再是**占位参数**

  - ==**一般我们都是在函数的声明中给出默认参数.我们必须确保函数的声明与函数的定义时只有一个定义默认参数**==

    ```C++
    int func(int a,int b=10);声明
    int func(int a,int b);定义
    或
    int func(int a, int b);声明
    int func(int a, int b=10);定义
    下面是错误的
    int func(int a, int b=10);声明
    int func(int a, int b=10);定义
    ```

#### 函数重载

> **作用**:支持多个函数同名,根据我们传递的参数,来调用同名函数中的某一个

- **条件**
  - 同名函数在同一作用域
  - 同名函数之间在参数数量,参数顺序,参数类型三者中至少有一个不同
- 引用作为参数的重载
  - `const 数据类型 &别名`与`数据类型 &别名`是不同的是可以正确创建重载的
    - 传变量那么进第二个
    - 传常量那么进第一个
- 默认参数的重载
  - `数据类型 形式参数名`与`数据类型 形式参数名 = 默认值`是**不可以**正确创建重载的
  - `数据类型 形式参数名`与`,数据类型 形式参数名,数据类型 形式参数名 = 默认值`是可以正确创建重载的,但是当我们不给默认值传递参数时,程序就不知道进哪一个函数,就会报错.因此尽量不要在涉及重载时使用默认参数

****

### 类与对象

#### 类

> 三大特性:==封装,继承,多态==

##### 封装

##### 使用class定义类

- 基本语法:`class 类名(访问权限 类属性 类方法 访问权限 类属性 类方法...)`

  ```c++
  class Circle{
  public:
      int r;
      double caculate(){
          return 2*3.14*r;
      };
  };
  ```

- **特性**

  - ==我们必须使用`类名 类对象名`来实例化类对==
  - 类内的属性属于类作用域下的全局变量,因此类中的任意方法都可以直接调用,修改类内的属性.
  - ==我们可以使用`实例化的类对象名.属性`的方式来访问实例化的类对象的属性==
  - ==我们可以使用`实例化的类对象名.方法()`的方式来调用实例化的类对象的方法==

##### :red_circle: 访问权限

- `public`:``public`的类内**属性与方法**,**可以**被类内方法访问,也**可以**通过`类实例.`的方式访问,还**可以被继承了该类的子类访问**

- `private`:`private`的类内属性与方法,**只能**被类内的方法访问,**不可以**通过`类实例.`的方式访问,也**不能被继承了该类的子类访问**

- `protected`:`protected`的类内**属性与方法**,**只能**被类内的方法访问,**不可以**通过`类实例.`的方式访问,但**可以被继承了该类的子类访问**

- ```C++
  //创建Persong类
  class Person{
  public:
  	string name;
  protected:
  	string car;
  private:
  	string passward;
  };
  //建立继承Persong类的子类
  
  ```

##### 使用`struct`定义类

- ==与`class`的区别:`struct`的默认权限为`public`,而`class`的默认权限为`private`==

- 实例

  ```c++
  struct Person{
  	string name;
      int func(){
          cout << '函数' << endl;
      };
  };
  ```

****

#### 对象

##### 对象的清理与初始化

> **作用**：即每一个对象在被创建时以及被销毁时都需要有对应的设置。而**构造函数**与**析构函数**就是用来实现这一点的

- ==**构造函数**==：在类被实例化时自动调用

  - **基本语法**：`类名(函数参数){语句体}`
  - **特性**
    - 没有返回值,不用写`return`
    - 函数名与类名相同
    - 可以有参数,可以重载
    - 对象被创建时,该函数会被自动调用

- ==**析构函数**==：在类实例被销毁时自动调用

  - **基本语法**:`~类名(函数参数){语句体}`
  - **特性**
    - 没有返回值,不用写`return`
    - 函数名称为`~类名`
    - 不可以有参数,也不可以重载
    - 对象被销毁时会自动调用

- 实例

  ```C++
  class Person{
  	string name;
  	Person(){
  		cout << '调用构造函数' << 'endl';
  	}
  	~Person(){
  		cout << '调用析构函数' << 'endl';
  	}
  }
  ```

##### 构造函数的分类与调用

###### 分类

- 按照参数分类
  - 有参数构造
  - 无参数构造
- 按照类型分类
  - 普通构造函数
  - ==拷贝构造函数==
    - 语法:`类名(const 类名 &任意名称){}`
    - **作用**:将指定类的引用传递给构造函数

###### ==调用==

- ==括号法==
  - `类名 对象名`:自动调用**无参数**构造函数
  - `类名 对象名(参数)`:自动调用**有参数**构造函数
  - `类名 对象名(已存在对象)`:自动调用**拷贝**构造函数
  - **注意**:若要调用无参数构造函数一定不能用`类名 对象名()` 
- 显示法
  - `类名 对象名 = 类名`:自动调用**无参数**构造函数
  - `类名 对象名 = 类名(参数)`:自动调用**有参数**构造函数
  - `类名 对象名 = 类名(已存在对象)`:自动调用**拷贝**构造函数
- 隐式转换法
  - `类名 对象名 = 参数`:等价于`类名 对象名(参数)`
  - `类名 对象名 = 已存在对象`等价于`类名 对象名(已存在对象)`

****

##### 匿名对象

- `类名`或`类名(参数)`
- **注意**:`类名(对象名)`等价于`类名 对象名`

****

##### ==拷贝构造函数的作用==

- 语法:`类名(const 类名 &任意名称){}`
- 作用
  - ==用于在创建一个新类时,通过拷贝构造函数,给新类的一部分属性初始化为已经存在的类的属性的值或方法的返回值==
  - 值传递类时被调用(原因在于值传递等价于`类名 形式参数名 = 被传递的类的名称`,这就是一个隐式转换法的拷贝构造函数调用)
  - 值方式返回类时被调用(原因在于在主函数中为`函数的返回值类型 对象名 = 函数名()`这就等价于`类名 对象名 = 即将被销毁的类的名称`,这也是一个隐式转换法的拷贝构造函数调用)

##### :japanese_goblin: 构造函数的调用规则

- 默认情况下,编译器会自动给类创建
  - **无参构造函数(函数体为空,没有实际作用)**
  - **析构函数(函数体为空,没有实际作用)**
  - **拷贝构造函数(会自动实现属性拷贝)**
- 若我们**自定义**了**有参构造**函数,则**只会**自动创建**拷贝构造函数(会自动实现属性拷贝)**
- 若我们**自定义**了**拷贝构造**参数,则**不会**自动创建任何构造函数(**此时便不会实现自动属性拷贝**)
- :red_circle:  **注意**:由于在一定情况下,编译器不会自动生成构造函数,因此我们应该在调用析构函数之前考虑是否存在该函数.

****

##### ==深拷贝与浅拷贝==

- 浅拷贝:通过简单的赋值来进行拷贝,并且对于指针类型的属性则直接传地址(编译器自动实现的拷贝就是浅拷贝)

  > - **问题**:我们通过浅拷贝创建的第二个类对象的指针与原类对象指向的是同一个内存地址,因此当我们释放了两者任意一个指针指向的内存地址后,如果在用另外一个再次执行内存地址释放操作,就会引起内存释放的非法操作.因此对于指针属性,我们应该尽可能地使用深拷贝来进行拷贝操作.

- 深拷贝:在堆区申请新的空间来进行拷贝

  > - **问题**:由于在拷贝构造时,会申请新的堆区内存空间进行存储与指向,因此我们拷贝构造的指针就与原指针指向不同的内存地址,但是他们的值在拷贝时是一致的.因此由于指向内存地址不同,因此就不过发生浅拷贝所引发的非法操作问题

- 实例

  - 浅拷贝

    ```c++
    class Person{
    	int age;
        int* heigth;
        Person(int age,int height){
            age = age;
            height = new int(height);
        };
        ~Person(){
            if(height!=NULL){
                delete height;
                height=NULL;
            };
        };
    };
    Person Person1(18,180);
    Person Person2(Person1);
    
    由于C++的先进后出原则因此函数结束时先释放Person2,再释放Person1,并且此时使用的是编译器自动实现的浅拷贝
    问题:
    前提Person1与Person2的height指针指向同一内存地址
    先释放Person2时,height的内存地址通过析构函数被释放了
    再来释放Person1时,又会对上一步同一个地址再次释放,这是系统不允许的,因此会出现错误
    ```
  
  - 深拷贝
  
    ```C++
    class Person{
    	int age;
        int* heigth;
        Person(int age,int height){
            age = age;
            height = new int(height);
        };
        ~Person(){
            if(height!=NULL){
                delete height;
                height=NULL;
            };
        };
        Person(const Person &P){
            age = P.age;
            heigth = new int(*P.height);
            //height = P.height;此为编译器自己实现的方式
        };
    };
    Person Person1(18,180);
    Person Person2(Person1);
    由于C++的先进后出原则因此函数结束时先释放Person2,再释放Person1,并且此时使用的是我们自己实现的深拷贝,因此不会出现浅拷贝的问题
    ```

****

##### 基于初始化列表以及构造函数的类属性初始化

- 基本语法
  - `类名(参数):属性1(值1),属性2(值2)...{语句体}`
  - `类名(参数1,参数2...):属性1(参数1),属性2(参数2)...{语句体}`
  
- 作用:用在构造函数上,用于给指定的属性分配初值

- ```c++
  class Person{
  	int m_age;
  	string m_name;
  	Person(int age,string name):m_age(age),m_name(name){
  		
  	}
  }
  ```


****

##### 类中的类

> **作用**:让指定的类作为其他类的成员属性,并可以通过`.`方法来调用类成员的属性

- **注意**:在主类构造时,其类成员先被构造完成,然后我们的主类才被构造完成.当主类销毁时,先销毁主类,再销毁类成员

- 实例

  - ```C++
    class Student{
    public:
    	string S_name;
    	int S_age;
    	Student(string name,int age){
    		S_name = name;
    		S_age = age;
    	};
    };
    class Teacher{
    public:
    	string T_name;
    	int T_age;
    	Student ZH;
    	Teacher(string name,int age,Student ZH){
    		T_name = name;
    		T_age = age;
    		this->ZH.S_name = ZH.S_name;
    		this->ZH.S_age = ZH.S_age;
    	};
    };
    Student ZhangSan = Student('ZhangSan',16);
    Teacher LiSi = Teacher('LiSi',38,ZhangSan);
    ```


****

##### 静态变量

- 特性

  - **所有的实例化对象共享一个静态变量**,**一个实例化对象修改了静态变量,那么其他实例化对象在访问该静态变量时,就得到的是修改后的值**
  - 在**编译阶段**静态变量便分配好了内存在全局区(一般的成员变量是在类实例化时获得内存分配,并且实例化多个类对象就会有**多个****存储在不同内存地址**的从属于不同类对象的成员变量)
  - 我们的静态变量需要在类内部进行定义,但是**其初始化必须在比当前类高一层级或以上的作用域进行**.

- 访问

  - 通过实例化类对象访问
  - 直接使用类名进行访问,无需实例化的类对象

- ```c++
  class Person{
  	static int num;
  };
  int Person::num = 100;
  void main(void)
  {
      Person ZhangSan;
      Person LiSi;
      //使用实例化对像访问
      cout << ZhangSan.num << endl;
      //探查不同对象共享同一静态变量的特性
      LiSi.num = 22;
      cout << ZhangSan.num << endl;
      
      //使用类名直接访问
      cout << Person::num << endl;
  }
  
  ```

##### 静态成员函数

- **特性**

  - 静态成员函数**只能访问静态变量**
  - 静态成员函数无需其所在类实例化便可通过`类名::静态函数名(参数)`的方式调用
  - 静态成员函数在编译阶段便进行了内存分配

- **访问**

  - 通过类名直接访问
  - 通过类实例进行访问

- ```C++
  class Person{
  public:
  	static Func(){
  		cout << '调用了静态函数Func' << endl;
  	};
  };
  void main(){
      //直接类名访问
      Person::Func();
      Person ZhangSan;
      ZhangSan.Func();
  };
  ```

****

##### 成员与成员函数的分离存储特性

> 前提:我们`C++`中成员属性与成员函数有不同的保存规则

- :one: **成员属性**

  - 每一个实例化对象都拥有其自己的成员属性,其成员属性各自占据内存空间,不与其他类对象共享内存空间

- :two: **成员函数**

  - 每一个类对象的成员函数都是与其他的类对象共享使用的同一个成员函数.其与静态成员函数的不同在于,其可以访问对应类对象的非静态成员属性,而且其不可以通过类名直接访问(当然其可以使用空指针访问).

- :red_circle: **注意**:如果我们用`sizeof`来探查一个类实例的占用内存大小就会发现**我们获取到的内存只是该类实例的==所有非静态成员属性==所占据的内存大小**,而其**成员函数,静态成员属性,静态成员函数都==不会算入==该类实例的内存占用**,因为他们与类的非静态成员属性是分开保存的并且他们是被所有的类对象共享使用的

- ```C++
  class Person{
  	int num;
  	static int Snum;
  	static void Func(){
  		cout << "静态函数" << endl;
  	};
  	void Func(){
  		cout << "非静态函数" << endl;
  	};
  };
  int Person::Snum = 102;
  void main(){
  	Person SS;
  	cout << sizeof(SS) << endl;
  };
  ```

****

##### `this`指针

> **背景**:由于类的成员函数是被共享使用的,因此`C++`的每一个成员函数都会自动维护一个`this`指针,其指向调用该成员函数的类实例.

- **作用**

  - :one: 当成员函数的形式参数与类的成员属性名称相同时(**我们的成员函数会直接把函数内部所有的与形式参数名称相同的变量解释为形式参数,而不会解释为类成员函数**),便可以使用`this->成员属性名`的方式来解决这一问题
  - :two: 我们可以给成员函数的返回值返回`*this`(此时成员函数要用`类名&`来修饰)从而实现`.`方法的链式调用

- ```C++
  class Person{
      int age;
      Person(int age){
          this->age = age;
      };
  };
  ```

- ```C++
  class Person{
  	int age = 0;
      Person& Func(){
          this->age += 1;
          return *this;
      };
  };
  int main(){
   	Person ZS;
      ZS.Func().Func().Func();
  };
  ```

****

##### 空指针访问

- > **作用**:用于在不实例化类对象的情况下调用成员函数,值得注意的是我们是不可以访问成员属性的,因为空指针是没有成员属性的.

- **实例**

  ```C++
  class Person{
  	int age;
      void Func(){
          /*if(this==NULL){
              return;
          };*/
          cout << 调用Func函数 << endl;
      };
  };
  void main(){
      Person* ZS = NULL;
  	ZS->Func();
      *ZS.Func();
  };
  ```

****

##### `const`修饰成员函数

- **作用**:这样定义的成员函数虽然可以读取成员属性,但是其不能修改成员属性的值(当然,如果成员属性在定义时使用了`mutable`关键字进行修饰,那么就能被修改)

- **语法**:`返回值类型 函数名() const {语句体}`

  ```C++
  class Person {
  public:
      int age;
      mutable int M_age;
      void Func() const {
          //age = 10;
          //cout << age << endl;
          M_age = 10;
          cout << M_age << endl;
      };
  };
  void main() {
      Person ZS;
      ZS.Func();
  };
  ```

****

##### `const`在类对象实例化时使用

- **作用**:将该类实例定义为常对象

- **特性**

  - :one:常对象**可以**访问常函数
  - :two:常对象可以访问声明时使用`mutable`修饰的成员属性
  - :three:常对象**不能**访问成员属性
  - :four:常对象**不能**访问成员函数
  - :five:常函数**可以**访问静态成员函数与静态成员属性

- ```C++
  class Person {
  public:
      static int Sage;
      int age;
      mutable int M_age;
      void Func() const {
          //age = 10;
          //cout << age << endl;
          M_age = 10;
          cout << M_age << endl;
      };
  };
  void main() {
      const Person ZS;
      ZS.Sage;
      //ZS.age;
      //ZS.M_age;
      cout << ZS.age << endl;
  };
  ```

****

##### 友元

- > **前提**:我们知道对于私有权限的成员属性与成员函数,除了我们的类对象内部的函数以外是无法被外接访问的.

- **作用**:通过`friend`关键字来指定`成员函数`,`类`,`全局函数`作为友元,让他们可以访问授予他们友元权限的类的内部``私有成员属性``或者``私有成员函数**``**

- **全局函数做友元**

  - :one:**语法**:`friend 全局函数的声明`
  - :two:**作用**:让该全局函数**可以**访问**该类实例**的私有成员属性与私有成员函数

- **成员函数做友元**

  - :one:**语法**:`friend 成员函数返回值类型 成员函数所属类::成员函数名(参数声明)`
  - :two:**作用**:让该``指定的成员函数``**可以**访问该**类实例**的私有成员属性与私有成员函数

- **类做友元**

  - :one:**语法**:`friend 类名`
  - :two:**作用**:让该类的**所有成员函数**都可以访问授权友元权限的类的实例的任何私有成员属性与私有成员函数

- ```C++
  #include<iostream>
  #include<string>
  using namespace std;
  class Person;
  void GloFunc(Person& P);
  class GoodGay {
  public:
      Person* P;
      GoodGay();
  public:
      void Visit1();
  };
  class Person {
      friend void GoodGay::Visit1();
  private:
      int num = 10;
      void Func() {
          cout << "调用私有函数" << endl;
      };
      friend void GloFunc(Person& P);
      friend class Test;
  };
  GoodGay::GoodGay() {
      P = new Person;
  };
  void GoodGay::Visit1() {
      P->Func();
  };
  class Test {
  public:
      Person* P = new Person;
      void Func() {
          P->Func();
          cout << P->num << endl;
      };
  };
  void GloFunc(Person& P) {
      P.Func();
      cout << P.num << endl;
  }
  void main() {
      Person P;
      Test T;
      GoodGay S;
      S.Visit1();
      T.Func();
  };
  
  ```

##### ==:red_circle: 重大问题:red_circle:==

- 关于成员函数做友元时可能会遇到的难以解决的一个问题

- **错误的原因**:我们的错误来源于我们对`C++`的声明机制的错误理解。`C++`的结构体与类是可以声明的,**但是我们必须认识到,与一般的全局函数的声明不同的是,在结构体或类的内部可能会涉及到对其他的结构体或类的成员函数与成员属性的使用,但是对于我们的`C++`编译器而言,编译时在真正编译到结构体或类的定义时,我们是无法得知他们内部有什么成员属性或成员函数的,但是我们的编译器又是不可能再回头去修改已经编译的内容的**.虽然我们在他们的定义之前可以通过声明机制,让我们可以在编写时能够通过各种方式写出访问这些成员函数或成员属性的语句,但是由于我们的编译器在编译时并不知道他们在真正被定义时会保存在哪里,因此**这些操作如果在编译的阶段就要分配内存的话,那么他们都将会是错误的无效操作(如==友元的授予工作==,其在编译阶段执行到授予语句时就已经进行了给指定内存分配友元的工作,并且不管是否已经定义)**,如我们如果在类定义之前通过`new`来创建该类的一个指针那么**结果一定会是我们调用了未知的类**,因为编译器虽然知道它是一个类,但是不知道它里面有什么,自然也就无法正确的分配内存.总而言之就是,**==当我们涉及到对于类与结构体的问题时,一定要保证,对他们的成员函数与成员属性的调用一定要在该类或结构体的定义位置之后.==** **如果实在无法以常规的方法使得前面的条件成立,那么我们可以通过`返回值类型 类名::函数名(参数){语句体}`的方式在需要被调用的结构体或类后面来给我们的成员函数进行定义(如下面正确示范的提示位置的用法.)**

- 错误示范

  ```C++
  #include<iostream>
  #include<string>
  using namespace std;
  class GoodGay;
  class Person {
      friend void GoodGay::Visit1();
  private:
      int num = 10;
      void Func() {
          cout << "调用私有函数" << endl;
      };
  };
  class GoodGay {
  public:
      Person* P;
      GoodGay();
  public:
      void Visit1();
  };
  GoodGay::GoodGay() {
      P = new Person;
  };
  void GoodGay::Visit1() {
      P->Func();
  };
  void main() {
      GoodGay S;
      S.Visit1();
  };
  
  ```

- 正确示范

  ```c++
  #include<iostream>
  #include<string>
  using namespace std;
  class Person;
  class GoodGay {
  public:
      Person* P;
      //使用了类外定义函数的方法
      GoodGay();
      void Visit1();
  };
  class Person {
      friend void GoodGay::Visit1();
  private:
      int num = 10;
      void Func() {
          cout << "调用私有函数" << endl;
      };
  };
  
  //在GoodGay类外定义GoodGay类的无参数构造函数/
  GoodGay::GoodGay() {
      P = new Person;
  };
  void GoodGay::Visit1() {
      P->Func();
  };
  //###################################//
  
  
  void main() {
      GoodGay S;
      S.Visit1();
  };
  ```

##### ==链式函数调用==

- > **前提**:对于结构体或者类实例而言,我们可以通过`实例化对象.成员函数名(参数)`的方式来调用该对象的成员函数,并且每一个成员函数中都自动维护一个`this`指针来指向调用当前成员函数的对象.

- 作用:通过让成员函数的返回值为我们实例化对象的引用便可以让我们可以使用链式调用方法`对线.函数.函数.函数...`从而方便我们对对象进行操作

- ```C++
  class Person{
  public:
      int count=0;
      Person& Func(){
          cout << count << endl;
          count += 1;
          return *this;
      };
  };
  int main(void){
      Person P;
      P.Func().Func().Func();
      return 0;
  }
  ```

****

##### 运算符重载

##### 常见运算符对应的函数名称

- |      |      |
  | ---- | ---- |
  |      |      |
  |      |      |
  |      |      |

- 加号重载

- 左移运算符重载

- 递增运算符重载

- 赋值重载

- 关系符号重载

- 函数调用重载

### 继承

#### 公共继承

- > **特性**
>
  > - 继承的类具备父类的共性也具备其自身的特性
  > - 继承的类可以**默认为具备所有的父类的==公共与保护的==成员属性与成员函数**(包括静态的),当然我们可以在继承的类内部**重定义这些成员属性或者成员函数**.值得注意的是,公共的成员属性与成员函数继承过来依旧是公共的,保护的成员属性与成员函数继承过来依旧是保护的

- **公共继承的语法**:`class 类名:public 要继承的类的类名{语句体}`

  - :red_circle: **注意**:父类的定义必须在子类之前

- ```C++
  class Base{
  public:
  	void Top(){
          cout << "调用父类的Top函数" << endl;
      };
      void Right(){
          cout << "调用父类的Right函数" << endl;
      };
  private:
      void PLeft(){
          cout << "调用父类的私有函数" << endl;
      };
  };
  class Page1:public Base{
  public:
      void Right(){
          cout << "调用子类的Right函数" << endl;
      };
  };
  int main(void){
      Page1 P;
      P.Right();
      P.Top();
      //P.PLeft();
      return 0;
  }
  ```

  ![image-20220430123856486](C:\Users\Administrator\Desktop\Typora文档\C++.assets\image-20220430123856486-16512938070791.png)

#### 保护继承

- > **特性**
  > - 公共的与私有的成员函数或成员属性都会被继承过来(包括静态的),但是公共的成员函数或成员属性继承过来后就**会变为保护权限的**.
- 语法:`class 类名:protected 父类的类名`

#### 私有继承

- > **特性**
  >
  > - 公共的与私有的成员函数或成员属性都会被继承过来(包括静态的),但是公共的成员函数或成员属性继承过来后就**会变为私有权限的**.
- 语法:`class 类名:private 父类的类名`

<img src="C:\Users\Administrator\Desktop\Typora文档\C++.assets\image-20220430124654110.png" alt="image-20220430124654110" style="zoom: 56%;" />

- **注意**

  - :one:继承了父类的子类也是可以被孙子类继续继承的.此时的子类的各个成员函数与成员属性的访问权限由继承父类时选择的继承方式与子类内部自己的定义决定.
  - :two:事实上父类的**所有成员函数以及成员属性(包括静态的)都会被继承到我们的子类**当中,只不过父类中的私有成员函数与私有成员属性会被我们的`C++`编译器隐藏,让我们无法通过任何方式访问,但是**虽然他们被隐藏了,但是他们占据的空间还是实实在在存在的**.
  - :three:实例化子类时先调用父类的构造函数再调用子类的构造函数,先调用子类的析构函数再调用父类的析构函数
  - :four::red_circle:当我们的子类中定义了与父类函数名相同的函数时,我们通过子类调用该函数时调用的便会是子类中的函数而非父类中的函数,当然我们可以通过`子类名.父类名::函数名(参数)`的方式来调用父类中的同名函数.并且值得注意的是,只要我们的子类中有同名函数的定义,即便我们的父类中对于该名字定义了几种不同的重载函数,那他们依然会被屏蔽掉,我们必须通过`子类名.父类名::函数名(参数)`才能访问

- ```C++
  #include<iostream>
  #include<string>
  using namespace std;
  class Person {
  public:
      void Func() {
          cout << "调用父类的Func函数" << endl;
      };
      void Func(int age) {
          cout << "调用了父类中的重载函数" << endl;
      };
  };
  class Son:public Person {
  public:
      void Func() {
          cout << "调用了子类的Func函数" << endl;
      };
  };
  int main(void) {
      Son S;
      S.Func();
      S.Person::Func();
      S.Person::Func(10);
      //S.Func(10);
  }
  ```

#### 子类访问父类静态成员属性与成员函数

- **静态成员属性访问**

  - `子类名.静态成员属性名`

  - `子类名.父类名::静态成员属性名`

  - `子类名::父类名::静态成员属性名`

- **静态成员函数访问**

  - `子类名.静态成员函数名`

  - `子类名.父类名::静态成员函数名`

  - `子类名::父类名::静态成员函数名`

#### 继承多个类

- 语法:`class 类名:继承方式 父类1名,继承方式 父类2名....{语句体}`
- > ==**注意**==:由于继承了多个类,因此会大大增加重名函数的问题,因此当我们使用到多继承时,如果希望使用任意父类成员函数与成员属性,我们都应该通过`::`来指定好作用域.

#### ==菱形继承==

- **问题**:

- **利用虚继承解决数据重复问题**

  - 语法:`class 类名:virtual 继承方式 父类名{语句体}`
  - 作用:我们便不会再拥有两份同样的数据,无论是通过羊的作用域进行访问还是用驼的作用域进行访问,还是自身直接访问都会直接访问到同一个
  - 特性
    - 虚继承对于我们的子函数而言是没有任何影响的，我们可以正常地使用这些虚继承地类，他们和正常继承并没有区别
    - 当两个或以上的虚继承的类作为父类被子类继承时，就与正常继承的类会有一些区别，如果父类都虚继承了同一个爷爷类，那么在子类中就不会同一般的正常继承一样存在两个及以上的爷爷类的成员属性，我们通过不同的父类的作用域访问也只能访问到同一个成员属性，而不会出现正常继承下的多份不同的成员属性的情况。
    - 使用虚继承能够很好地解决菱形继承类似问题时的内存空间浪费问题

- **虚继承的原理**

  - **使用虚拟继承的类会==维护一个指针（指向表）以及一个表（保存继承的类）*==，这可以让他们知道他们继承了哪些类**，而当我们有孙子类继承我们这些使用了虚拟继承的类时，编译器就会通过这些指针判断这些被继承的类是否继承了同样的类，如果是继承了同样的父类，那么编译器就会自动地进行处理，**使得我们的孙子类在继承时==不会重复继承多份相同的成员属性==**，而是会在内存分配时让我们的被继承的每一个父类的作用域下这些相同的成员属性共享同一个内存地址，这样就让我们能够在使用不同的作用域进行访问时访问到的是同一个内存地址的数值，而不是正常情况下的每个父类作用域下的每个属性占据一篇内存空间。从而避免了内存空间的浪费。

  - 应用场景

  - ```C++
    #include<iostream>
    #include<string>
    using namespace std;
    class Base {
    public:
    	int age;
    	string name;
    };
    class user1:virtual public Base{};
    class user2:virtual public Base{};
    class client:public user1,public user2{};
    int main(void) {
    	user1 use1;
    	user2 use2;
    	use1.age = 100;
    	use2.age = 50;
        //这里表明虚拟继承对于类本身的功能而言与普通继承没有区别
    	cout << "use1.age=" << use1.age << endl;
    	cout << "use2.age=" << use2.age << endl;
    	client clien;
    	clien.age = 10;
    	clien.user1::age = 15;
    	clien.user2::age = 20;
        //这里表明虚拟继承让多个作用域下的同一成员属性共享同一片内存
    	cout << "clien.age" << clien.age << endl;
    	cout << "clien.user1::age" << clien.user1::age << endl;
    	cout << "clien.user2::age" << clien.user2::age << endl;
    	return 0;
    }
    ```

    

- <img src="C:\Users\Administrator\Desktop\Typora文档\C++.assets\image-20220430142019753.png" alt="image-20220430142019753" style="zoom:50%;" />
- <img src="C:\Users\Administrator\Desktop\Typora文档\C++.assets\image-20220430143113618.png" alt="image-20220430143113618" style="zoom: 60%;" />

### :japanese_ogre:==多态==

- > 注意：C++是允许子类被强制转化为父类的

- 多态的两种类型

  - 静态多态（地址早绑定）
    - 特点:在编译阶段便分配好了函数的内存地址
    - 函数多态
    - 运算符重载
  - 动态多态（地址晚绑定）
    - 特点:在程序运行阶段才会分配函数的内存地址
    - 派生类
    - 虚函数

- **早绑定与晚绑定的区别**

  - 早绑定：在编译阶段就已经分配好了内存
  - 晚绑定:

- **虚函数指针与虚函数表**

  - 虚函数指针：我们可以在类中使用`virtual 函数名(参数){语句体}`的方式定义类中的虚函数,只要我们的类中存在虚函数就会产生一个虚指针(无论多少个虚函数都只有一个虚指针)指向一张虚函数表,这张表中存储着对应类中所有的虚函数,每当我们要调用对应类的虚函数时,就会通过类的虚指针找到虚函数表,查表得到对应的虚函数的入口地址.
  - ![image-20220502200118265](C:\Users\Administrator\Desktop\Typora文档\C++.assets\image-20220502200118265.png)
  - 虚函数表：我们每一个具备虚函数的类都会有一张虚函数表(无论实例化多少次虚函数表都只有一个,虚指针也是)这个表中按顺序存储着对应类中每一个虚函数的入口地址。
  - ![image-20220502200301061](C:\Users\Administrator\Desktop\Typora文档\C++.assets\image-20220502200301061.png)

- **==虚函数表与与虚函数指针对于动态多态的作用:japanese_goblin:==**

  - > **动态多态发生的条件**：:red_circle:父类建立了虚函数,子类重写了这一虚函数,那么此时通过传引用或指针接收时会发生多态:blue_heart:`传入子类用父类& 别名接收`:blue_heart: `传入子类地址用父类* 指针名接收 `,:red_circle:传值时直接发生强制转换不会发生多态

  - **作用**:当我们在父类中定义了虚函数,并且子类重写了这一虚函数.那么当我们发生传引用或指针的情形时,便不会再发生调用的函数是父类的函数而不是子类的函数的问题(当然,这仅仅限于传引用或传指针后调用虚函数的情形,如果是调用非虚函数,那么虽然我们传入的是子类实例,但是调用的会是父类的同名函数:red_circle:如果我们调用父类中没有的函数,那么则会报错:red_circle:)

  - **原因**:这一切都是由于早绑定与晚绑定的区别引发的,我们在定义全局函数时我们的编译器就已经笃定了将要传入我们这个函数的是什么类型的值,因此对于传值问题,就会发生强制转换,而对于传引用与传指针问题(传引用其实某种程度上也是传指针),我们的编译器也会直接使用编译阶段就已经形成的指针类型进行指向,而当我们传入的是子类时,**由于指针是父类类型的指针**,因此该指针**只能**访问到子类中父类作用域下的那些成员属性与成员函数(在子类继承父类问题中,所有的父类成员属性与成员函数都会被继承到我们的子类中,并且即便子类重写某些成员函数或成员属性,他们依旧还是保存在子类中我们可以通过施加作用域来进行访问).:blue_book: 而对于父类中使用了虚函数的情形则不同,当父类使用了虚函数子类进行继承时,子类中继承到的并非父类中的虚函数而是一个指向一张虚函数表的虚指针(这个虚指针与虚函数都是属于子类的与父类不是同一份,但是子类继承到的虚指针是从父类继承得到的,因此可以被父类的指针访问到),并且此时我们重载父类中的虚函数后,会直接通过虚指针到虚函数表中覆盖被重载的虚函数的地址.对于使用了虚函数的类,访问其虚函数则不再是往常一样直接通过函数入口地址进行访问,而是先访问内部的虚指针,得到虚函数表的位置,然后再从虚函数表中查找到对应的函数的入口地址进行调用.而正是由于这个原因,当我们使用父类指针接受子类地址时,由于子类内部的虚指针是通过父类继承得到的,因此我们的父类指针是可以访问到的,因此当我们发起虚函数的调用请求时,就会通过父类指针调用子类的虚函数指针,找到子类的虚函数表(值得注意的时,由于子类重写虚函数会直接覆盖原虚函数,因此虚函数表中父类作用域下的虚函数已经被覆盖了)由于指针是不管作用域,而只管调用同名函数的,因此其就会直接在我们的虚函数表中找到对应的函数进行调用.这也就是使用虚函数与不使用虚函数的区别.

  - **使用虚函数**:子类只会继承父类的虚指针,而不会直接继承虚函数(这也就导致了我们在子类中没有办法通过`子类`.父类名::虚函数名的方式调用父类作用域下的虚函数,因为我们的编译器根本就没有把任何虚函数传递给我们的子类),访问虚函数是通过虚函数指针得到虚函数表从而查表得到.并且子类重写虚函数不是在虚函数表中添加一个数据项,而是把被重写的虚函数直接覆盖掉.而这个继承得到的虚函数指针是可以被父类类型的指针访问到,而虚函数的访问是通过虚指针进行的,这一系列的实现搭配在一起就造成了,多态的现象

  - **不使用虚函数**:在不使用虚函数的情况下,我们的子类会直接继承所有的父类中的成员函数与成员属性,并且即便我们的子类重写了父类的函数的情况下,从父类中继承的成员函数与成员变量也不会被覆盖掉,虽然直接使用函数名访问到的是子类的函数,但是我们可以通过加上作用域的方式访问到父类的该函数.而当我们通过父类指针接收子类地址时,我们的父类指针只能访问到子类中属于父类的部分,而不属于父类的部分是没有访问权限的,但是这也就导致了,即便我们的子类重写了函数,但是当传引用与传地址的情况下调用的不是子类中的函数而是父类中的函数的情况.

  - **虚函数表与虚指针**:虚函数表与虚函数指针在类与类之间是不共享的(子类继承父类的虚函数指针与虚函数表,但其实现是深拷贝的,而非直接共享地址使用)同一个类下的不同实例共享同一个虚函数表与虚函数指针

  - **证明没有传递虚函数,我们无法通过作用域方式对虚函数进行访问**

    ```C++
    #include<iostream>
    #include<string>
    using namespace std;
    class Base {
    public:
    	int age = 18;
    	virtual void speak() {
    		cout << "Base" << endl;
    	};
    };
    class MM :public Base {
    public:
    	int a = 88;
    	int age = 188;
    	virtual void speak() {
    		cout << "MM" << endl;
    	};
    };
    int main(void) {
    	//SS S;
    	//Func(S);
    	MM M;
    	cout << M.Base::speak << endl;
    	return 0;
    }
    ```

  - **证明父类指针只能访问子类的父类作用域下的函数与属性**

    ```c++
    #include<iostream>
    #include<string>
    using namespace std;
    class Base {
    public:
    	int age = 18;
    	virtual void speak() {
    		cout << "Base" << endl;
    	};
    };
    class MM :public Base {
    public:
    	int agee = 188;
    	void speakMM() {
    		cout << "MM" << endl;
    	};
    };
    void Func(Base& B) {
    	B.speakMM();  
    	B.agee;
    	cout << B.age << endl;
    };
    int main(void) {
    	//SS S;
    	//Func(S);
    	MM M;
    	Func(M);
    	return 0;
    }
    ```

- **指针与引用获取输入与值传递获取输入的区别**

  - ```C++
    //传递子类的引用，使用Base& B = 传入的值，没有进行强制类型转换此时函数会不管传入的值，而是直接初始化一个Base类的B变量作为接受到的输入，因此无论我们怎么修改传入的引用，主函数中传入的变量不会受到任何影响。
    void Func1(Base& B) {
    	B.speak();  
    	cout << B.age << endl;
    };
    
    //值传递子类，函数在接受到值传递时使用Base B = 传入的值，进行了强制的类型转换
    void Func2(Base B) {
    	B.speak();  
    	cout << B.age << endl;
    };
    ```

- 我们考察下面的实例

  ```C++
  #include<iostream>
  #include<string>
  using namespace std;
  class OO {
  public:
  	int mm = 888;
  	void speak() {
  		cout << "OO" << endl;
  	};
  };
  class Base {
  public:
  	int age = 18;
  	void speak() {
  		cout << "Base" << endl;
  	};
  };
  class MM :public Base {
  public:
  	int age = 188;
  	void speak() {
  		cout << "MM" << endl;
  	};
  };
  class SS :public MM,public OO {
  public:
  	int age = 123;
  	void speak() {
  		cout << "SS" << endl;
  	};
  };
  //这里就发生了早绑定
  void Func(Base& B) {
  	B.speak();
  	cout << B.age << endl;
  };
  int main(void) {
  	SS S;
  	Func(S);
  	MM M;
  	Func(M);
  	return 0;
  }
  ```

  <img src="C:\Users\Administrator\Desktop\Typora文档\C++.assets\image-20220430185233144.png" alt="image-20220430185233144" style="zoom:150%;" />

### 纯虚函数与抽象类

> **前提**：在父类中定义的虚函数在子类中一般都会被重写，因此在这种情况下父类的虚函数没有必要具体定义。因此就产生了纯虚函数与抽象类的概念

- 纯虚函数
  - 基本语法：`virtual 返回值类型 函数名(参数) = 0`
- **抽象类**
  - 定义了纯虚函数的类就被称为抽象类
  - **特性**
    - ==抽象类无法通过任何方式被实例化==
    - 继承了抽象类的子类如果不重写抽象父类的纯虚函数,那么其也是无法实例化的(即也是抽象类)
- **作用**
  - 前提:前面我们知道,对于继承了父类(具备虚函数)的子类而言,我们的使用父类指针指向子类对象,是可以直接访问到子类中的虚函数的,并且如果有多个不同的子类继承了父类的情况下,可以通过修改指针指向的对象来访问不同子类下的虚函数.
  - 显然我们使用虚函数的目的就是为了可以只在父类中定义虚函数,在子类中重写虚函数的情形下,只需通过父类的指针就可以调用不同的子类的虚函数.而父类中虚函数中的语句体定义与否都是无所谓的(并且父类也是无需实例化的,父类只是用于提供一个访问指针而已,并且在这种情形下子类必须重写虚函数,不然对于目的而言没有意义),因此我们直接定义纯虚函数与抽象类,就更简便地达到我们的目的
- **最终目标**:

### :flower_playing_cards: 虚析构与纯虚析构

- **前提**:由于虚函数使用的最终目的,我们一般情况下都是直接使用`父类* 指针名 = new 子类`的方式来直接在堆区开辟内存,而这就带来了一个问题,我们使用类构造的三种正常形式都是会自动调用类的析构函数的,但是这里则不同,由于我们用父类指针指向了子类,而父类指针只能调用父类的析构函数,而不能调用子类的析构函数,因此我们使用`delete 指针名`来释放指针是只能调用到子类中继承到的父类的析构函数的,而子类自己的析构函数则不会被调用,这也就导致了内存的不完全释放.因此我们必须要通过一定方法,让父类指针能够调用子类的析构函数,而这就是我们的**虚析构机制**
- **虚析构**:`virtual ~类名(){语句体}`
  - 作用:将析构函数也用`virtual`关键字修饰,那么就可以通过虚函数指针让我们的父类指针可以访问到子类的虚函数表,从而当我们使用`delete 指针`释放父类指针时,父类指针就会通过虚函数指针找到虚函数表,**并按顺序执行表内存储的所有虚析构函数**.**值得我们注意的是,当我们的子类继承了使用了虚析构的父类时,由于父类的析构函数是通过虚函数表传递给子类的,因此当我们的子类释放时,是直接按顺序调用所有存储在子类的虚函数表中的析构函数的**.而这就保证了无论我们是使用`父类名* 指针名 = new 子类名`的方式建立子类然后通过`delete 指针名`来释放内存,还是通过`子类名 实例化对象名`的方式来实例化子类都可以借助虚函数指针与虚函数表来完完整整地释放干净所有被我们创建的内存.
- **纯虚析构**:`virtual ~类名 = 0`然后在比类高一级作用域下`父类名::~父类名(){语句体}`来具体定义纯虚析构的具体内容
  
  - 作用:与前面纯虚函数作用相同
- > **注意**:虚析构也是一种虚函数,因此使用了纯虚析构的父类是抽象类,而子类如果不重写虚析构函数,那么其也将会是抽象类

****

### 文件操作

- **前提**
  - **读写**:`include<fstream>`
  - **写**:`include<ofstream>`
  - **读**:`include<ifstream>`
- **常用打开方式**
  - `ios::in`:只读
  - `ios::out`:只写
  - `ios::ate`:打开并将光标位置置于文件末尾
  - `ios::app`:以追加的方式写文件类似于`ios::out|ios::ate`
  - `ios::trunc`:若文件已经存在,则将文件删除然后新建一个再执行操作
  - `ios::binary`:以二进制格式操作文件

#### 写文件

##### 文本文件

- 特点:以文本的ASCII码为中介存储在计算机中的文件(当然ASCII码最终也要转换成二进制存储,但是我们人可以通过ASCII码一定程度上直接读取到文本的信息)

###### 基本步骤

- 包含头文件`include<fstream>`
- 创建流对象`ofstream 对象名`
- 打开文件`流对象名.open('<文件路径>',<打开方式1>|<打开方式2>|...)`
- 写数据`流对象名 << "<写入的数据>" << endl;`
  - **注意**:这里`endl`相当于换行符号(`\n`)当然我们可以直接`流对象名 << "<写入的数据>\n";`也是相同功效的
- 关闭文件`流对象名.close()

##### 二进制文件

- 特点:直接以文本的二进制表示存储在计算机中的文件(人类显然无法直接看懂二进制文件)

###### 基本步骤

- 包含头文件`include<fstream>`
- 创建流对象`ofstream 对象名`
- 打开文件`流对象名.open('<文件路径>',ios::binary|ios::out|...)`
- 写数据`流对象名.write((const char *)&待写入对象,sizeof(待写入对象的数据类型))`
- 关闭文件`流对象名.close()`

#### 读文件

##### 文本文件

###### 基本步骤

- 包含头文件`include<fstream>`

- 创建流对象`ifstream 对象名`

- 打开文件`流对象名.open('<文件路径>',<打开方式1>|<打开方式2>|...)`

- 判断文件的打开是否成功`流对象名.is_open()`

- 读数据

  - ```c++
    char buf[1024] = {0};
    一
    while(流对象名 >> buf){};每一次循环读取文件的一行,并且每一次读取都会覆盖buf;
    
    二
    while(流对象名.getline(buf,sizeof(buf))){};每一循环读取文件的一行(最多buf长度个字节的数据),并且每一次存储都会把上一次的buf内容覆盖掉
    
    三
    string buf;
    while(getline(流对象名,buf)){};每一次读取文件的一行存储在buf中,并且每一次存储都会覆盖上一次存储的内容
    
    四
    char c;
    while((c = 流对象名.get())!=EOF){};每一个循环读取一个字符,并且每一次读取都会覆盖上一次读取的内容
    ```

  - **优先使用第三种方法**

- 关闭文件`流对象名.close()`

##### 二进制文件

###### 基本步骤

- 包含头文件`include<fstream>`
- 创建流对象`ifstream 对象名`
- 打开文件`流对象名.open('<文件路径>',ios::binary|ios::out|...)`
- 判断文件的打开是否成功`流对象名.is_open()`
- 读数据`流对象名.read((char *)&接收对象名,sizeof(接收对象的数据类型));`
- 关闭文件`流对象名.close()`

> #### 重点
>
> 使用`write`进行二进制文件的写入可以写入`C++`中的任何数据类型,并且也可以用`read`对被写入的任何数据类型进行读取,而一般的文本文件写入读取方法只能写入字符串,而我们的其他任何数据类型都是无法正确写入与读取的
>

****

## 第三部分

### 模板

> 1. **模板的概念**
>    - 模板是一个通用的框架,我们通过对一个模板进行修改便可以快速地实现一个新的功能.模板又类似于Web编程中组件的概念
>
> 2. **模板的分类**
>    - 函数模板:建立一个通用的函数,其返回值类型与参数类型都不具体指定而是用一个虚拟的类型来作为占位
>    - 类模板:
>

#### 函数模板定义

- 基本语法:`template<typename T>`

  - `template`:声明我们接下来要创建模板了
  - `typename`:声明其后面的符号代表一个虚拟的数据类型(也可以用`class`替代`template`,效果不变)
  - `T`:虚拟的数据类型,可以指定为任意名称如`template<typename SS>`

- 具体实例

  ```C++
  交换a,b的值的模板(只定义了一个虚拟数据类型)
  template<typename T>
  void SwapInt(T& a,T& b){
      T temp = a;
      a = b;
      b = temp;
  };
  
  实现任意两个数据相加的模板(定义了多个虚拟数据类型)
  template<typename T1,typename T2,typename T3>
  T3 Add(T1 a,T2 b){
      return a+b;
  };
  ```

#### 函数模板使用

- **自动类型推导**

  - :red_circle:使用方法:`模板函数名(实参)`

  - 特点:`C++`编译器会自动对参数类型进行推理,然后使用其认为合理的数据类型来接收输入的实参并使用合适的数据类型进行返回

    - **值得注意的是,编译器的推导只是将每一个虚拟数据类型推导出一个其认为合理的真实数据类型,当推导出来后,我们的函数模板中使用了对应虚拟数据类型的地方,都会使用推导出来的那个真实数据类型代替**

      ```c++
      交换a,b的值的模板(只定义了一个虚拟数据类型)
      template<typename T>
      void SwapInt(T& a,T& b){
          T temp = a;
          a = b;
          b = temp;
      };
      
      比如上面,如果我们的编译器推导出T应该为int,那么我们的函数调用就会等价于调用下面的函数
      void SwapInt(int& a,int& b){
          int temp = a;
          a = b;
          b = temp;
      };
      ```

  - **:red_circle:问题**

    - :one:对于我们的输入的实参与返回值类型,编译器可能会推导出不止一种类型,而此时就会产生二异性,我们的编译器不知道到底给我们的那个虚拟数据类型确定为这多个类型中的哪一个,此时就会报错.**这又被简单表述为,只有编译器能唯一确定每一个虚拟数据类型的真实类型时,才能保证程序正确运行**

    - :two:编译器必须要能够确定虚拟数据类型才能正确使用模板

      ```C++
      template<typename T>
      void Func(){
      	cout << "调用Func函数" << endl;
      };
      此时我们完全没有办法推导出T的数据类型,并且虽然就算不知道T是什么类型函数其实也可以成功调用我们的编译器也还是不会允许这种不确定T类型的调用操作(会报错)
      ```

      

- **显示类型指定**

  - :red_circle: **使用方法**:`模板函数名<数据类型1,数据类型2...>(实参)`
  - 特点:通过直接指定模板函数中每个虚拟数据类型的真实数据类型来调用模板函数

#### 函数模板与普通函数的区别与相同点

- 区别
  - 函数模板在使用自动类型推导的情况下由于是根据输入参数进行类型自动推导,因此只有符合输入参数类型成功执行与不能唯一推导虚拟数据类型而失败报错两种结果,不会发生对输入参数的强制类型转换
  - 普通函数由于指定了输入参数的类型,因此如果输入的参数不符合要求,那么编译器就会**自动将其强制转换**为实现规定的类型
- 相同点
  - 如果函数模板使用完全的显示类型指定(即给每一个被使用的虚拟数据类型都指定真实数据类型),那么其实际上就与普通函数的使用完全相同了.同样也会发生强制类型转换.

#### 普通函数与函数模板的调用规则

- :one:如果普通函数存在与其**同名且参数个数相同**的函数模板,当我们的**输入参数类型符合普通函数要求**时会优先调用普通函数

  ```C++
  void Func(int a,int b){
  	cout << "调用了普通函数" << endl;
  };
  template<typename T>
  void Func(T a,T b){
  	cout << "调用了函数模板" << endl;
  };
  int main(void){
      Func(10,20);
      Func(10.2,20.2);
      return 0;
  };
  ```

- :two:我们可以通过**空模板参数列表**来强制调用函数模板

  ```C++
  void Func(int a,int b){
  	cout << "调用了普通函数" << endl;
  };
  template<typename T>
  void Func(T a,T b){
  	cout << "调用了函数模板" << endl;
  };
  int main(void){
      Func<>(10,20);
      Func(10.2,20.2);
      return 0 ;
  };
  ```

- :three:函数模板也可以发生重载

  ```C++
  template<typename T>
  void Func(T a,T b){
  	cout << "调用了函数模板" << endl;
  };
  template<typename T>
  void Func(T a,T b,T c){
  	cout << "调用了重载后的函数模板" << endl;
  };
  int main(void){
      Func(10,20);
      Func(10,20,30);
  };
  ```

- :four:如果输入的实参交给普通函数需要发生强制类型转换,那么就会交给函数模板看是否能执行,如果能执行就会使用函数模板,如果不能执行才会使用普通函数

  ```C++
  void Func(int a,int b){
  	cout << "调用了普通函数" << endl;
  };
  template<typename T>
  void Func(T a,T b){
  	cout << "调用了函数模板" << endl;
  };
  int main(void){
      Func(10,20);
      Func(10.2,20.2);
      return 0;
  };
  ```

#### 函数模板的局限性

- 模板并不是通用的,如用于判断两个数是否相等的模板用于数组相等判断,自定义类相等判断时就无法正确判断,甚至会报错

  - 解决方法通过对函数模板进行重载的方法来对一些具体的数据类型定义其自己的处理方式

  - **注意**:此时我们一定要使用`template <>`的方式,这样可以避免我们使用`template<typename T>`所产生的编译器无法获知`T`的类型而产生无论如何也无法调用函数的情况

  - ```C++
    template<typename T>
    void Compare(T a,T b){语句体}
    
    通过重载Compare模板让其可以处理Person自定义数据类型的比较问题
    template<>
    void Compare(Person a,Person b){语句体}
    ```

****

#### 类模板

- **作用**:构建一个通用类,这个类中的成员属性的数据类型或成员函数的返回值以及形式参数可以通过虚拟数据类型进行定义

- **基本语法**

  ```C++
  template<typename T1,typename T2....>
  class 类名{类语句体}
  
  或
      
  template<class T1,class T2....>
  class 类名{类语句体}
  ```


##### 类模板与函数模板的不同

- 类模板是不具备自动推导机制的,由类创建对象的原理就知道原因.

- 类模板可以在定义模板时，给各个虚拟数据类型指定默认的真实数据类型

  ```C++
  template<class T1 = int,class T2 = string>
  ```

- 我们使用类模板在没有给全部虚拟数据类型指定默认数据类型的情况下，必须通过尖括号来对每一个虚拟数据类型指定其真实数据类型`类模板名<数据类型1,数据类型2...> 类对象名;`

- **注意:japanese_ogre:**:只有类模板可以由默认数据类型,函数模板是不具备的.

#####  类模板中成员函数的创建时机:red_circle:

- 普通类中的所有成员函数在程序一开始运行就会成功创建,得到内存分配
- 类模板中的成员函数只有我们使用类模板实例化了类对象并使用到了该成员函数**才会试图创建该被使用的成员函数,其他没有被使用的则不会被创建**.

##### 类模板对象做函数参数的三种方法

> - 指定传入的类型
>
> - 参数模板化
>
> - 整个类模板化

```c++
#include<iostream>
#include<string>
using namespace std;
template<class Agetype, class Nametype>
class Person {
public:
    Agetype m_age;
    Nametype m_name;
    void showPerson() {
        cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
    };
    Person(Nametype name, Agetype age) {
        this->m_age = age;
        this->m_name = name;
    };
};

//指定数据类型
void test1(Person<int, string> p) {
    p.showPerson();
};

//参数列表模板化
template<class T1, class T2>
void test2(Person<T1, T2> p) {
    p.showPerson();
};

//整个类模板化
template<class T>
void test3(T p) {
    p.showPerson();
}
int main(void) {
    Person<int, string> ZS("张三", 18);
    test1(ZS);
    test2(ZS);
    test3(ZS);
    return 0;
}
```

##### 类模板与继承

- 子类在继承类模板时,必须在定义时就指定出类模板使用的虚拟数据类型的真实数据类型

  ```c++
  #include<iostream>
  #include<string>
  using namespace std;
  template<class Agetype, class Nametype>
  class Person {
  public:
      Agetype m_age;
      Nametype m_name;
      void showPerson() {
          cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
      };
  };
  
  
  //直接指定父类的参数类别列表
  class Son1 :public Person<int ,string> {
  public:
      void sonFunc() {
          cout << "调用了子类的成员函数" << endl;
      };
  };
  
  //参数模板化
  template<class T1,class T2>
  class Son2 :public Person<T1,T2> {
  public:
      void sonFunc() {
          cout << "调用了子类的成员函数" << endl;
      };
  };
  int main(void) {
      Son1 S1;
      S1.sonFunc();
  
      Son2<int, string> S2;
      S2.sonFunc();
      return 0;
  };
  ```

##### 类模板的成员函数的类外实现

- ```C++
  #include<iostream>
  #include<string>
  using namespace std;
  template<class Agetype, class Nametype>
  class Person {
  public:
      Agetype m_age;
      Nametype m_name;
      void showPerson() {
          cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
      };
      Person(Nametype name, Agetype age) {}
  };
  template<class Agetype, class Nametype>
  Person<Agetype,Nametype>::Person(Nametype name, Agetype age) {
      this->m_age = age;
      this->m_name = name;
  };
  ```

##### ==类模板分文件编写==

- > `include<.h>`与`include<.cpp>`与`include<.hpp>`有什么区别？
  
- > 什么是类模板分文件编写？
  >

- 原始文件

  ```c++
  #include<iostream>
  #include<string>
  using namespace std;
  template<class Agetype, class Nametype>
  class Person {
  public:
      Agetype m_age;
      Nametype m_name;
      void showPerson() {
          cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
      };
      Person(Nametype name, Agetype age) {
          this->m_age = age;
          this->m_name = name;
      };
  };
  
  //指定数据类型
  void test1(Person<int, string> p) {
      p.showPerson();
  };
  int main(void) {
      Person<int, string> ZS("张三", 18);
      test1(ZS);
      test2(ZS);
      test3(ZS);
      return 0;
  }
  ```

- 拆分编写(原文件)

  ```C++
  #include<iostream>
  #include<string>
  using namespace std;
  template<class Agetype, class Nametype>
  class Person {
  public:
      Agetype m_age;
      Nametype m_name;
      void showPerson() {
          cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
      };
      Person(Nametype name, Agetype age);
  };
  template<class Agetype, class Nametype>
  Person<Agetype,Nametype>::Person(Nametype name, Agetype age) {
      this->m_age = age;
      this->m_name = name;
  };
  
  //指定数据类型
  void test1(void) {
      Person<int, string> ZS("张三", 18);
      ZS.showPerson();
  };
  int main(void) {
      test1();
      return 0;
  }
  ```

  - 第一种情形,直接包含`.h`(不会发生错误)

    ```C++
    /*******************************************.h文件**********************************************/
    #pragma once
    #include<iostream>
    #include<string>
    using namespace std;
    template<class Agetype, class Nametype>
    class Person {
    public:
        Agetype m_age;
        Nametype m_name;
        void showPerson() {
            cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
        };
        Person(Nametype name, Agetype age);
    };
    /*********************************************主函数文件******************************************/
    #include<iostream>
    #include<string>
    #include".h"
    void test1(void) {
        //Person<int, string> ZS("张三", 18);
        //ZS.showPerson();
    };
    int main(void) {
        test1();
        return 0;
    }
    ```

  - 第二种情形,直接包含`.h`(会发生错误)

    ```C++
    /*******************************************.h文件**********************************************/
    #pragma once
    #include<iostream>
    #include<string>
    using namespace std;
    template<class Agetype, class Nametype>
    class Person {
    public:
        Agetype m_age;
        Nametype m_name;
        void showPerson() {
            cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
        };
        Person(Nametype name, Agetype age);
    };
    /*********************************************主函数文件******************************************/
    #include<iostream>
    #include<string>
    #include".h"
    void test1(void) {
        Person<int, string> ZS("张三", 18);
        ZS.showPerson();
    };
    int main(void) {
        test1();
        return 0;
    }
    ```

  - 第三种情形,直接包含`.h`(不会发生错误)

    ```C++
    /*******************************************.h文件**********************************************/
    #pragma once
    #include<iostream>
    #include<string>
    using namespace std;
    template<class Agetype, class Nametype>
    class Person {
    public:
        Agetype m_age;
        Nametype m_name;
        void showPerson() {
            cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
        };
        Person(Nametype name, Agetype age){
            this->m_age = age;
        	this->m_name = name;
        };
    };
    /*********************************************主函数文件******************************************/
    #include<iostream>
    #include<string>
    #include".h"
    void test1(void) {
        Person<int, string> ZS("张三", 18);
        ZS.showPerson();
    };
    int main(void) {
        test1();
        return 0;
    }
    ```

  - 第四种情形,`.cpp`包含`.h`,主函数包含`.cpp`(不会发生错误)

    ```C++
    /*******************************************.h文件**********************************************/
    #pragma once
    #include<iostream>
    #include<string>
    using namespace std;
    template<class Agetype, class Nametype>
    class Person {
    public:
        Agetype m_age;
        Nametype m_name;
        void showPerson() {
            cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
        };
        Person(Nametype name, Agetype age);
    };
    
    /********************************************.cpp文件*******************************************/
    #include<iostream>
    #include<string>
    #include".h"
    using namespace std;
    template<class Agetype, class Nametype>
    Person<Agetype,Nametype>::Person(Nametype name, Agetype age) {
        this->m_age = age;
        this->m_name = name;
    };
    
    /*********************************************主函数文件******************************************/
    #include<iostream>
    #include<string>
    #include".cpp"
    void test1(void) {
        Person<int, string> ZS("张三", 18);
        ZS.showPerson();
    };
    int main(void) {
        test1();
        return 0;
    }
    ```

  - 第五种情形,直接包含`.h`文件(不会发生错误)

    ```C++
    /*******************************************.h文件**********************************************/
    #pragma once
    #include<iostream>
    #include<string>
    using namespace std;
    template<class Agetype, class Nametype>
    class Person {
    public:
        Agetype m_age;
        Nametype m_name;
        void showPerson() {
            cout << "m_age=" << m_age << "\n" << "m_name=" << m_name << endl;
        };
        Person(Nametype name, Agetype age);
    };
    template<class Agetype, class Nametype>
    Person<Agetype,Nametype>::Person(Nametype name, Agetype age) {
        this->m_age = age;
        this->m_name = name;
    };
    
    /*********************************************主函数文件******************************************/
    #include<iostream>
    #include<string>
    #include".h"
    void test1(void) {
        Person<int, string> ZS("张三", 18);
        ZS.showPerson();
    };
    int main(void) {
        test1();
        return 0;
    }
    ```

- > 上述代码呈现出什么问题？

##### :red_circle:==类模板与友元==:red_circle:

###### 全局函数

- 类内实现使用了虚拟数据类型的全局函数,同时附加友元

  ```C++
  #include<iostream>
  #include<string>
  using namespace std
  template<class T1,class T2>
  class Person{
      T1 m_name="张三";
      T2 m_age=18;
      friend void GlobalFunc(Person<T1,T2> p){
          cout << "调用了类内实现的全局函数," << "m_name=" << p.m_name << endl;
      };
  };
  int main(void){
      GlobalFunc();
      return 0;
  };
  ```

- 类外实现使用了虚拟数据类型的全局函数,然后类内给函数附加友元(非常复杂)

  ```C++
  #include<iostream>
  #include<string>
  using namespace std
  /**********************************************************************************************/    
  template<class T1,class T2>
  class Person  
  /**********************************************************************************************/    
  template<class T1,class T2>
  void GlobalFunc(Person<T1,T2> p){
      cout << "调用了类内实现的全局函数," << "m_name=" << p.m_name << endl;
  };
  /**********************************************************************************************/
  template<class T1,class T2>
  class Person{
      T1 m_name="张三";
      T2 m_age=18;
      //这里加<>用于告诉编译器这里我们声明的函数是一个会在未来定义的函数模板
      friend void GlobalFunc<>(Person<T1,T2> p);
  };
  /**********************************************************************************************/
  int main(void){
      Person<string,int> p;
      GlobalFunc(p);
      return 0;
  };
  ```

  ```C++
  #include<iostream>
  #include<string>
  using namespace std
  /**********************************************************************************************/    
  template<class T1,class T2>
  class Person  
  /**********************************************************************************************/    
  template<class T1,class T2>
  void GlobalFunc(Person<T1,T2> p)
  /**********************************************************************************************/
  template<class T1,class T2>
  class Person{
      T1 m_name="张三";
      T2 m_age=18;
      //这里加<>用于告诉编译器这里我们声明的函数是一个会在未来定义的函数模板
      friend void GlobalFunc<>(Person<T1,T2> p);
  };
  /**********************************************************************************************/
  template<class T1,class T2>
  void GlobalFunc(Person<T1,T2> p){
      cout << "调用了类内实现的全局函数," << "m_name=" << p.m_name << endl;
  };
  /**********************************************************************************************/
  int main(void){
      Person<string,int> p;
      GlobalFunc(p);
      return 0;
  };
  ```

  

- 类内实现不使用虚拟数据类型的全局函数,同时附加友元

- 类外实现不适用虚拟数据类型的全局函数,同时附加友元

****

### :small_red_triangle: C++中子类构造函数与父类构造函数的关系

- 若是子类没有自定义构造方法(此时编译器会自动生成有参,无参,拷贝三种构造函数)，在建立子类的对象的时候,首先调用父类的无参数的构造方法,然后根据用户建立子类对象的方式调用由编译器自行生成的三种构造方法之一.
- 若是子类只自定义了带参数的构造方法)(此时编译器不会自动创建有参,午餐构造,但是会自动创建拷贝构造)，在建立子类的对象的时候,首先执行父类的无参数的构造方法，而后执行我们自定义的有参数构造方法。 
- 在建立子类对象时候，若是子类的构造函数**没有显式调用**父类的构造函数，且**父类本身没有自定义任何构造函数**则会调用父类的由**编译器自动生成的默认无参构造函数**。
- 在建立子类对象时候，若是子类的构造函数**没有显示调用**父类的构造函数且**父类本身提供自定义的了无参构造函数**，则会调用父类的**自定义的无参构造函数**。
- 在建立子类对象时候，若是子类的构造函数**没有显示调用**父类的构造函数且**父类只自定义了有参构造函数**(此时编译器**不会给父类生成默认的无参构造函数**)，此时由于**在创建子类时需要自动调用父类的无参数构造函数**,而无参数构造函数并没有存在于父类中,我们的程序便会报错.

#### 子类显式调用父类有参构造函数

- **语法**:
  - `子类名():父类构造函数名(参数){语句体}`
  - `子类名(参数):父类构造函数名(参数){语句体}`
  - `子类名(const 子类名& 别名):父类构造函数名(参数){语句体}`

- ```C++
  #include <iostream.h>
  class animal{
  public:
  	animal( int height, int weight){
          cout<< "animal construct" <<endl;
      };
  };
  
  class fish: public animal{
  public:
      fish():animal(400,300){
          cout<< "fish construct" <<endl;
      };
  };
  
  int main(){
      fish fh;
      return 0;
  };
  ```

****

## 基础知识

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

****

### ==关于声明的一些问题==

#### 函数声明问题

#### 类与结构体声明问题

- `C++`的结构体与类是可以声明的,**但是我们必须认识到,与一般的全局函数的声明不同的是,在结构体或类的内部可能会涉及到对其他的结构体或类的成员函数与成员属性的使用,但是对于我们的`C++`编译器而言,编译时在真正编译到结构体或类的定义时,我们是无法得知他们内部有什么成员属性或成员函数的,但是我们的编译器又是不可能再回头去修改已经编译的内容的**.虽然我们在他们的定义之前可以通过声明机制,让我们可以在编写时能够通过各种方式写出访问这些成员函数或成员属性的语句,但是由于我们的编译器在编译时并不知道他们在真正被定义时会保存在哪里,因此**这些操作如果在编译的阶段就要分配内存的话,那么他们都将会是错误的无效操作(如==友元的授予工作==,其在编译阶段执行到授予语句时就已经进行了给指定内存分配友元的工作,并且不管是否已经定义)**,如我们如果在类定义之前通过`new`来创建该类的一个指针那么**结果一定会是我们调用了未知的类**,因为编译器虽然知道它是一个类,但是不知道它里面有什么,自然也就无法正确的分配内存.总而言之就是,**==当我们涉及到对于类与结构体的问题时,一定要保证,对他们的成员函数与成员属性的调用一定要在该类或结构体的定义位置之后.==** **如果实在无法以常规的方法使得前面的条件成立,那么我们可以通过`返回值类型 类名::函数名(参数){语句体}`的方式在需要被调用的结构体或类后面来给我们的成员函数进行定义(如下面正确示范的提示位置的用法.)**

- 不知道要授予友元的函数的地址而导致友元授予无效

  ```C++
  #include<iostream>
  #include<string>
  using namespace std;
  class GoodGay;
  class Person {
      friend void GoodGay::Visit1();
  private:
      int num = 10;
      void Func() {
          cout << "调用私有函数" << endl;
      };
  };
  class GoodGay {
  public:
      Person* P;
      GoodGay();
      void Visit1();
  };
  GoodGay::GoodGay() {
      P = new Person;
  };
  void GoodGay::Visit1() {
      P->Func();
  };
  void main() {
      GoodGay S;
      S.Visit1();
  };
  ```

- 无法得知被调用函数的入口地址从而编译阶段无法正确链接函数入口而发生错误

  ```C++
  #include<iostream>
  #include<string>
  using namespace std;
  class GoodGay;
  class Person {
  public:
  /***************************************************************************************************/
   //由于我们的编译器不知道GoodGay里面到底时什么内容因此GoodGay G语句由于无法正确链接到类的构造函数的入口地址而发生错误 //并且由于我们此时不知道GoodgGay中的visit1函数的函数入口在哪里,因此当编译时希望链接到该函数入口地址时便发生了错误
      void Func2(GoodGay G) {
          G.Visit1();
      }
  /***************************************************************************************************/
  private:
      int num = 10;
      void Func() {
          cout << "调用私有函数" << endl;
      };
  };
  class GoodGay {
  public:
      Person* P;
      GoodGay();
      void Visit1();
  };  
  GoodGay::GoodGay() {
      P = new Person;
  };
  void GoodGay::Visit1() {
      cout << "调用了Visit1函数" << endl;
  };
  void main() {
      GoodGay S;
      S.Visit1();
  };
  ```

- 只是写一些运行阶段才执行的指令不会出错

#### ==为什么函数声明就没有类与结构体声明这么麻烦呢?==

- 具体不同的点
  - 

****

### 函数的分文件编写

- 编写用于函数声明的`.h`文件
- 编写函数语句体所在的`.cpp`文件
- 使用时先#`include "h文件名.h"`
- 然后直接调用函数

****

### `const 数据类型*`与`数据类型* const`

- **第一种定义**的指针的指向可以改变,但是不可以通过该指针修改其指向的变量的值
- **第二种定义**的指针的指向可以改变,但可以通过该指针修改其指向的变量的值

****

### cin/cout

- 作用
  - `cin`:获取用户输入
  - `cout`:向用户输出
- 字符
  - `>>`:输入流
  - '<<':输出流
  - `endl`:结束标志符号

****

### 变量,常量命名规则

- 不能是关键字
- 只能由字母,数字,下划线组成
- 区分大小写
- 必须由下划线或字母开头

****

### 三目运算符

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

****

### goto

- 作用:跳转到指定标志处执行

```C++
goto <标志名>

<标志名>:
	语句体
```

****

### include<>与include ""

****

### 常用技巧

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

****

### 常用函数

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

