## 一、python基础语法



## 二、python函数

### 1、定义函数

你可以定义一个由自己想要功能的函数，以下是简单的规则：

- 函数代码块以 **def** 关键词开头，后接函数标识符名称和圆括号**()**。
- 任何传入参数和自变量必须放在圆括号中间。圆括号之间可以用于定义参数。
- ·
- 函数内容以冒号起始，并且缩进。
- **return [表达式]** 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None。

### 2、语法

```python
def function_name(parameters) :
    # 注释
    function_suite  # 函数体
    return [exprestion]  # 函数返回值
```

默认情况下，参数值和参数名称是按函数声明中定义的顺序匹配起来的。

### 3、实例

以下为一个简单的Python函数，它将一个字符串作为传入参数，再打印到标准显示设备上。

```
def printme(str) :
    # 打印传入的字符到标准显示设备上
    print(str)
    return str
```

### 4、函数调用

定义一个函数只给了函数一个名称，指定了函数里包含的参数，和代码块结构。

这个函数的基本结构完成以后，你可以通过另一个函数调用执行，也可以直接从Python提示符执行。

如下实例调用了printme（）函数：

```python
printme("123")
```

### 5、参数传递

在 python 中，类型属于对象，变量是没有类型的：

```python
a = [1, 2, 3]
a = "Runoob"
```

以上代码中，[1,2,3] 是 List 类型，**"Runoob"** 是 String 类型，而变量 a 是没有类型，她仅仅是一个对象的引用（一个指针），可以是 List 类型对象，也可以指向 String 类型对象。

### 6、可更改(mutable)与不可更改(immutable)对象

在 python 中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。

- **不可变类型：**变量赋值 **a=5** 后再赋值 **a=10**，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变a的值，相当于新生成了a。
- **可变类型：**变量赋值 **la=[1,2,3,4]** 后再赋值 **la[2]=5** 则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了。

python 函数的参数传递：

- **不可变类型：**类似 c++ 的值传递，如 整数、字符串、元组。如fun（a），传递的只是a的值，没有影响a对象本身。比如在 fun（a）内部修改 a 的值，只是修改另一个复制的对象，不会影响 a 本身。
- **可变类型：**类似 c++ 的引用传递，如 列表，字典。如 fun（la），则是将 la 真正的传过去，修改后fun外部的la也会受影响

python 中一切都是对象，严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

### 7、python 传不可变对象实例

```python
def change_int(a) :
    a = 10

b = 2
change_int(b)
print(b)  # 结果是2
```

实例中有 int 对象 2，指向它的变量是 b，在传递给 change_int 函数时，按传值的方式复制了变量 b，a 和 b 都指向了同一个 Int 对象，在 a=10 时，则新生成一个 int 值对象 10，并让 a 指向它。

### 8、传递可变对象实例

```python
def change_list(myList) :
    # 修改传入的列表
    myList.append([1, 2, 3, 4])
    print(myList)
    return


# 调用change_list
myList = [10, 20, 30]
change_list(myList)
print(myList)  # 输出 [10, 20, 30, [1, 2, 3, 4]]
```

### 9、参数

以下是调用函数时可使用的正式参数类型：

- 必备参数
- 关键字参数
- 默认参数
- 不定长参数

必备参数

必备参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。

调用printme()函数，你必须传入一个参数，不然会出现语法错误：

关键字参数

关键字参数和函数调用关系紧密，函数调用使用关键字参数来确定传入的参数值。

使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值。

以下实例在函数 printme() 调用时使用参数名：

```python
def print_info(name, age) :
    print("name = %s" % name)
    print("age = %d" % age)
    return


# 调用pring_info 函数
print_info(age=50, name="zhangsan")
```

默认参数

调用函数时，默认参数的值如果没有传入，则被认为是默认值。下例会打印默认的age，如果age没有被传入：

```python
# 调用pring_info 函数
print_info(age=50, name="zhangsan")
print_info("zhangsan", 50)


def print_info_1(name, age=30) :
    print("name = %s" % name)
    print("age = %d" % age)
    return


# 调用pring_info 函数
print_info_1(name="zhangsan")
```

不定长参数

你可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述2种参数不同，声明时不会命名。基本语法如下：

```python
def print_info_2([formal_args, ] *var_args_tuple) :
    # 函数体
    return 
```

加了星号（*）的变量名会存放所有未命名的变量参数。不定长参数实例如下：

```python
def print_info_2(arg1, *var_tuple) :
    print(arg1)
    for var in var_tuple:
        print(var)
    return


# 调用函数
print_info_2(10) # 10
print(10, 20, False, "zhangsan", [10, 20, 30]) # 10 20 False zhangsan [10, 20, 30]
```

### 10、匿名函数

python 使用 lambda 来创建匿名函数。

- lambda只是一个表达式，函数体比def简单很多。
- lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
- lambda函数拥有自己的命名空间，且不能访问自有参数列表之外或全局命名空间里的参数。
- 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。

语法

lambda函数的语法只包含一个语句，如下：

```python
lambda [arg1 [,arg2,.....argn]]:expression
```

```
sum = lambda arg1, arg2: arg1 + arg2
print(sum(10, 20))  # 30
```

### 11、return 语句

return语句[表达式]退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None。之前的例子都没有示范如何返回数值，下例便告诉你怎么做：

```python
def return_sum(arg1, arg2) :
    # 返回两个参数的和
    total = arg1 + arg2
    print("%d 和 %d 的和为 %d" % (arg1, arg2, total))
    return total;


# 调用函数
total = return_sum(10, 20)
print(total)
```

### 12、变量作用域

一个程序的所有的变量并不是在哪个位置都可以访问的。访问权限决定于这个变量是在哪里赋值的。

变量的作用域决定了在哪一部分程序你可以访问哪个特定的变量名称。两种最基本的变量作用域如下：

- 全局变量
- 局部变量

### 13、全局变量和局部变量

定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。

局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。如下实例：

```python
total = 0  # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
    # 返回2个参数的和."
    total = arg1 + arg2  # total在这里是局部变量.
    print("函数内是局部变量 : %d" % total)
    return total


# 调用sum函数
sum(10, 20)
print("函数外是全局变量 : %d" % total)
```



## 三、python模块

Python 模块(Module)，是一个 Python 文件，以 .py 结尾，包含了 Python 对象定义和Python语句。

模块让你能够有逻辑地组织你的 Python 代码段。

把相关的代码分配到一个模块里能让你的代码更好用，更易懂。

模块能定义函数，类和变量，模块里也能包含可执行的代码。

### 1、import 语句

模块定义好后，我们可以使用 import 语句来引入模块，语法如下：

```python
import module1[, module2[,... moduleN]]
```

比如要引用模块 math，就可以在文件最开始的地方用 **import math** 来引入。在调用 math 模块中的函数时，必须这样引用：

```python
模块名.函数名
```

当解释器遇到 import 语句，如果模块在当前的搜索路径就会被导入。

搜索路径是一个解释器会先进行搜索的所有目录的列表。如想要导入模块 support.py，需要把命令放在脚本的顶端：

```python
import math

print(math.floor(3.5))
```

一个模块只会被导入一次，不管你执行了多少次import。这样可以防止导入模块被一遍又一遍地执行。

### 2、from…import 语句

Python 的 from 语句让你从模块中导入一个指定的部分到当前命名空间中。语法如下：

```python
from math import floor
print(floor(3.5))
```

把一个模块的所有内容全都导入到当前的命名空间也是可行的，只需使用如下声明：

```python
from math import *
```

### 3、搜索路径

当你导入一个模块，Python 解析器对模块位置的搜索顺序是：

- 1、当前目录
- 2、如果不在当前目录，Python 则搜索在 shell 变量 PYTHONPATH 下的每个目录。
- 3、如果都找不到，Python会察看默认路径。UNIX下，默认路径一般为/usr/local/lib/python/。

模块搜索路径存储在 system 模块的 sys.path 变量中。变量里包含当前目录，PYTHONPATH和由安装过程决定的默认目录。

### 4、PYTHONPATH 变量

作为环境变量，PYTHONPATH 由装在一个列表里的许多目录组成。PYTHONPATH 的语法和 shell 变量 PATH 的一样。

在 Windows 系统，典型的 PYTHONPATH 如下：

```
set PYTHONPATH=c:\python27\lib;
```

在 UNIX 系统，典型的 PYTHONPATH 如下：

```
set PYTHONPATH=/usr/local/lib/python
```

### 5、命名空间和作用域

变量是拥有匹配对象的名字（标识符）。命名空间是一个包含了变量名称们（键）和它们各自相应的对象们（值）的字典。

一个 Python 表达式可以访问局部命名空间和全局命名空间里的变量。如果一个局部变量和一个全局变量重名，则局部变量会覆盖全局变量。

每个函数都有自己的命名空间。类的方法的作用域规则和通常函数的一样。

Python 会智能地猜测一个变量是局部的还是全局的，它假设任何在函数内赋值的变量都是局部的。

因此，如果要给函数内的全局变量赋值，必须使用 global 语句。

global VarName 的表达式会告诉 Python， VarName 是一个全局变量，这样 Python 就不会在局部命名空间里寻找这个变量了。

例如，我们在全局命名空间里定义一个变量 Money。我们再在函数内给变量 Money 赋值，然后 Python 会假定 Money 是一个局部变量。然而，我们并没有在访问前声明一个局部变量 Money，结果就是会出现一个 UnboundLocalError 的错误。取消 global 语句前的注释符就能解决这个问题。

### 6、dir()函数

dir() 函数一个排好序的字符串列表，内容是一个模块里定义过的名字。

返回的列表容纳了在一个模块里定义的所有模块，变量和函数。如下一个简单的实例：

```python
# 导入内置math模块
import math
content = dir(math)
print(content);
```

在这里，特殊字符串变量__name__指向模块的名字，__file__指向该模块的导入文件名。

### 7、globals() 和 locals() 函数

根据调用地方的不同，globals() 和 locals() 函数可被用来返回全局和局部命名空间里的名字。

如果在函数内部调用 locals()，返回的是所有能在该函数里访问的命名。

如果在函数内部调用 globals()，返回的是所有在该函数里能访问的全局名字。

两个函数的返回类型都是字典。所以名字们能用 keys() 函数摘取。

### 8、reload() 函数

当一个模块被导入到一个脚本，模块顶层部分的代码只会被执行一次。

因此，如果你想重新执行模块里顶层部分的代码，可以用 reload() 函数。该函数会重新导入之前导入过的模块。语法如下：

```
reload(module_name)
```

在这里，module_name要直接放模块的名字，而不是一个字符串形式。比如想重载 hello 模块，如下：

```
reload(hello)
```

### 9、Python中的包

包是一个分层次的文件目录结构，它定义了一个由模块及子包，和子包下的子包等组成的 Python 的应用环境。

简单来说，包就是文件夹，但该文件夹下必须存在 __init__.py 文件, 该文件的内容可以为空。**__init__.py** 用于标识当前文件夹是一个包。

考虑一个在 **package_runoob** 目录下的 **runoob1.py、runoob2.py、__init__.py** 文件，test.py 为测试调用包的代码，目录结构如下：

```
test.py
package_runoob
|-- __init__.py
|-- runoob1.py
|-- runoob2.py
```



## 四、python面向对象

### 1、面向对象技术简介

- **类(Class):** 用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。对象是类的实例。
- **类变量：**类变量在整个实例化的对象中是公用的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用。
- **数据成员：**类变量或者实例变量, 用于处理类及其实例对象的相关的数据。
- **方法重写：**如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。
- **局部变量：**定义在方法中的变量，只作用于当前实例的类。
- **实例变量：**在类的声明中，属性是用变量来表示的。这种变量就称为实例变量，是在类声明的内部但是在类的其他成员方法之外声明的。
- **继承：**即一个派生类（derived class）继承基类（base class）的字段和方法。继承也允许把一个派生类的对象作为一个基类对象对待。例如，有这样一个设计：一个Dog类型的对象派生自Animal类，这是模拟"是一个（is-a）"关系（例图，Dog是一个Animal）。
- **实例化：**创建一个类的实例，类的具体对象。
- **方法：**类中定义的函数。
- **对象：**通过类定义的数据结构实例。对象包括两个数据成员（类变量和实例变量）和方法。

### 2、创建类

使用 class 语句来创建一个新类，class 之后为类的名称并以冒号结尾:

```python
class className:
	# 类的主体
```

类的帮助信息可以通过ClassName.__doc__查看。

class_suite 由类成员，方法，数据属性组成。

实例

以下是一个简单的 Python 类的例子:

```python
class Employee:
    empCount = 0
    
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.empCount += 1

    def display_count(self) :
        print("Total Employee %d" % Employee.empCount)

    def display_employee(self) :
        print("Name : %s, Salary : %f" % (self.name, self.salary))
```

- empCount 变量是一个类变量，它的值将在这个类的所有实例之间共享。你可以在内部类或外部类使用 Employee.empCount 访问。
- 第一种方法__init__()方法是一种特殊的方法，被称为类的构造函数或初始化方法，当创建了这个类的实例时就会调用该方法
- self 代表类的实例，self 在定义类的方法时是必须有的，虽然在调用时不必传入相应的参数。

self代表类的实例，而非类

类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的**第一个参数名称**, 按照惯例它的名称是 self。

```python
class Test:
    def prt(self):
        print(self)
        print(self.__class__)
 
t = Test()
t.prt()
```

以上实例执行结果为：

```
<__main__.Test instance at 0x10d066878>
__main__.Test
```

从执行结果可以很明显的看出，self 代表的是类的实例，代表当前对象的地址，而 **self.__class__** 则指向类。

self 不是 python 关键字，我们把他换成 runoob 也是可以正常执行的:

### 2、创建实例对象

实例化类其他编程语言中一般用关键字 new，但是在 Python 中并没有这个关键字，类的实例化类似函数调用方式。

以下使用类的名称 Employee 来实例化，并通过 __init__ 方法接收参数。

```python
"创建 Employee 类的第一个对象"
emp1 = Employee("Zara", 2000)
"创建 Employee 类的第二个对象"
emp2 = Employee("Manni", 5000)
```

### 3、访问属性

您可以使用点号 **.** 来访问对象的属性。使用如下类的名称访问类变量:

```python
emp1.display_employee()
emp2.display_employee()
print "Total Employee %d" % Employee.empCount
```

完整实例

```python
class Employee:
    empCount = 0

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.empCount += 1

    def display_count(self):
        print("Total Employee %d" % Employee.empCount)

    def display_employee(self):
        print("Name : %s, Salary : %f" % (self.name, self.salary))

    def prt(self):
        print(self)
        print(self.__class__)

# 创建Employee类的第一个对象
emp1 = Employee("zhangsan", 1000)
# 创建Employee的第二个对象
emp2 = Employee("lisi", 2000)
emp1.display_employee()
emp2.display_employee()
print("Total Employee %d" %Employee.empCount)
```

执行以上代码输出结果如下：

```python
Name : zhangsan, Salary : 1000.000000
Name : lisi, Salary : 2000.000000
Total Employee 2
```

你可以添加，删除，修改类的属性，如下所示：

```python
emp1.age = 7  # 添加一个 'age' 属性
emp1.age = 8  # 修改 'age' 属性
del emp1.age  # 删除 'age' 属性
```

你也可以使用以下函数的方式来访问属性：

- getattr(obj, name[, default]) : 访问对象的属性。
- hasattr(obj,name) : 检查是否存在一个属性。
- setattr(obj,name,value) : 设置一个属性。如果属性不存在，会创建一个新属性。
- delattr(obj, name) : 删除属性。

```python
hasattr(emp1, 'age')    # 如果存在 'age' 属性返回 True。
getattr(emp1, 'age')    # 返回 'age' 属性的值
setattr(emp1, 'age', 8) # 添加属性 'age' 值为 8
delattr(emp1, 'age')    # 删除属性 'age'
```

### 4、Python内置类属性

- __dict__ : 类的属性（包含一个字典，由类的数据属性组成） 
- __doc__ :类的文档字符串 
- __name__: 类名 
- __module__: 类定义所在的模块（类的全名是'__main__.className'，如果类位于一个导入模块mymod中，那么className.__module__ 等于 mymod） 
- __bases__ : 类的所有父类构成元素（包含了一个由所有父类组成的元组） 

Python内置类属性调用实例如下：

```python
print(Employee.__doc__)
print(Employee.__name__)
print(Employee.__module__)
print(Employee.__bases__)
print(Employee.__dict__)
```

### 5、python对象销毁(垃圾回收)

Python 使用了引用计数这一简单技术来跟踪和回收垃圾。

在 Python 内部记录着所有使用中的对象各有多少引用。

一个内部跟踪变量，称为一个引用计数器。

当对象被创建时， 就创建了一个引用计数， 当这个对象不再需要时， 也就是说， 这个对象的引用计数变为0 时， 它被垃圾回收。但是回收不是"立即"的， 由解释器在适当的时机，将垃圾对象占用的内存空间回收。

```
a = 40      # 创建对象  <40>
b = a       # 增加引用， <40> 的计数
c = [b]     # 增加引用.  <40> 的计数

del a       # 减少引用 <40> 的计数
b = 100     # 减少引用 <40> 的计数
c[0] = -1   # 减少引用 <40> 的计数
```

垃圾回收机制不仅针对引用计数为0的对象，同样也可以处理循环引用的情况。循环引用指的是，两个对象相互引用，但是没有其他变量引用他们。这种情况下，仅使用引用计数是不够的。Python 的垃圾收集器实际上是一个引用计数器和一个循环垃圾收集器。作为引用计数的补充， 垃圾收集器也会留心被分配的总量很大（及未通过引用计数销毁的那些）的对象。 在这种情况下， 解释器会暂停下来， 试图清理所有未引用的循环。

实例

析构函数 __del__ ，__del__在对象销毁的时候被调用，当对象不再被使用时，__del__方法运行：

```python
class Point:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def __del__(self):
        class_name = self.__class__.__name__
        print(class_name+"被销毁")


pt1 = Point()
pt2 = pt1
pt3 = pt1
print(id(pt1))
print(id(pt2))
print(id(pt3))

del pt1
del pt2
del pt3
```

以上实例运行结果如下：

```
3083401324 3083401324 3083401324
Point 销毁
```

**注意：**通常你需要在单独的文件中定义一个类，

### 6、类的继承

面向对象的编程带来的主要好处之一是代码的重用，实现这种重用的方法之一是通过继承机制。

通过继承创建的新类称为**子类**或**派生类**，被继承的类称为**基类**、**父类**或**超类**。

**继承语法**

```python
class 派生类名(基类名)
    ...
```

在python中继承中的一些特点：

- 1、如果在子类中需要父类的构造方法就需要显示的调用父类的构造方法，或者不重写父类的构造方法。详细说明可查看：[python 子类继承父类构造函数说明](https://www.runoob.com/w3cnote/python-extends-init.html)。
- 2、在调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别在于类中调用普通函数时并不需要带上 self 参数
- 3、Python 总是首先查找对应类型的方法，如果它不能在派生类中找到对应的方法，它才开始到基类中逐个查找。（先在本类中查找调用的方法，找不到才去基类中找）。

如果在继承元组中列了一个以上的类，那么它就被称作"多重继承" 。

**语法：**

派生类的声明，与他们的父类类似，继承的基类列表跟在类名之后，如下所示：

```python
class SubClassName (ParentClass1[, ParentClass2, ...]):
    ...
```

```python
class Parent:
    parentAttr = 100

    def __init__(self):
        print("调用父类的构造函数")

    def parentMethod(self):
        print("调用父类方法")

    def setAttr(self, attr):
        Parent.parentAttr = attr

    def getAttr(self):
        print("父类属性值为：%d" % Parent.parentAttr)


class Child(Parent):
    def __init__(self):
        print("调用子类构造方法")

    def childMethod(self):
        print("调用子类方法")


c = Child()  # 实例化子类
c.childMethod()  # 调用子类的方法
c.parentMethod()  # 调用父类方法
c.setAttr(200)  # 再次调用父类的方法 - 设置属性值
c.getAttr()  # 再次调用父类的方法 - 获取属性值
```

以上代码执行结果如下：

```
调用子类构造方法
调用子类方法
调用父类方法
父类属性 : 200
```

你可以继承多个类

```python
class A:        # 定义类 A
.....

class B:         # 定义类 B
.....

class C(A, B):   # 继承类 A 和 B
```

你可以使用issubclass()或者isinstance()方法来检测。

- issubclass() - 布尔函数判断一个类是另一个类的子类或者子孙类，语法：issubclass(sub,sup)
- isinstance(obj, Class) 布尔函数如果obj是Class类的实例对象或者是一个Class子类的实例对象则返回true。

### 7、方法重写

如果你的父类方法的功能不能满足你的需求，你可以在子类重写你父类的方法：

实例：

```python
class Parent:        # 定义父类
   def myMethod(self):
      print '调用父类方法'
 
class Child(Parent): # 定义子类
   def myMethod(self):
      print '调用子类方法'
 
c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法
```



### 8、基础重载方法

下表列出了一些通用的功能，你可以在自己的类重写：

| 1    | __init__(self [, args...])  构造函数<br />简单的调用方法：obj = className(args) |
| :--- | ------------------------------------------------------------ |
| 2    | __del__(self)<br />析构方法，删除一个对象<br />简单的调用方法：del obj |
| 3    | **__repr__( self )**<br/>转化为供解释器读取的形式<br/>简单的调用方法 : *repr(obj)* |
| 4    | **__str__( self )**<br/>用于将值转化为适于人阅读的形式<br/>简单的调用方法 : *str(obj)* |
| 5    | **__cmp__ ( self, x )**<br/>对象比较<br/>简单的调用方法 : *cmp(obj, x)* |

### 9、类属性与方法

类的私有属性

**__private_attrs**：两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 **self.__private_attrs**。

### 类的方法

在类的内部，使用 **def** 关键字可以为类定义一个方法，与一般函数定义不同，类方法必须包含参数 self,且为第一个参数

### 类的私有方法

**__private_method**：两个下划线开头，声明该方法为私有方法，不能在类的外部调用。在类的内部调用 **self.__private_methods**

```python
class JustCounter:
    __secretCount = 0  # 私有变量
    publicCount = 0  # 公开变量

    def count(self):
        self.__secretCount += 1
        self.publicCount += 1
        print(self.__secretCount)


counter = JustCounter()
counter.count()
counter.count()
print(counter.publicCount)
print(counter.__secretCount)  # 报错，实例不能访问私有变量
```

Python不允许实例化的类访问私有数据，但你可以使用 **object._className__attrName**（ **对象名._类名__私有属性名** ）访问属性，参考以下实例：

