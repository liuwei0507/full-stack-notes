## 一、JavaScript简介



## 二、JavaScript基本语法

6、流程控制语句

1）if...else...

2）switch

​	在Java中switch可以接收的数据类型：byte，in t，short，char，枚举（1.5）string（1.7）

​	在JavaScript中，switch可以接收任意类型的数据

​	

```js
var a = "abc";
        switch(a) {
            case 1:
                alert("number");
                break;
            case "abc":
                alert("string");
                break;
            case true:
                alert("boolean");
                break;
            case null:
                alert("null");
                break;
            case undefined:
                alert("undefined");
                break;
      }
```

3）for

```javascript
for(var i = 1;i<=100;i++) {
   sum += num;
}
```

4）while

```javascript
while(num<=100) {
   sum += num;
   num++;
}
```

5）do...while



7、例子，完成99 乘法表

```js
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>99乘法表</title>
        <style>
            td {
                border: 1px solid;
            }
        </style>
        <script>
            document.write("<table border = '1' align = 'center'>");
            for(var i = 1; i <= 9; i++) {
                document.write("<tr>")
                for(var j = 1; j <= i; j++) {
                    document.write("<td>")
                    document.write(i + "*" + j + "=" + i * j + "&nbsp");
                    document.write("</td>")
                }
                document.write("<br>");
                document.write("</tr>")
            }

            document.write("</table>");
        </script>
        
    </head>
</html>
```

![image-20191121141133947](/Users/wei.liu/Library/Application Support/typora-user-images/image-20191121141133947.png)

## 三、JavaScript对象

1、Array

1.1、创建

​          方式1: var arr = new Array(元素列表)；

​          方式2: var arr = new Array(默认长度);

​          方式3: var = [元素列表];

1.2、方法：

​            1）join(参数) :将数组中的元素按照指定的分隔符拼接为字符串,不传参数的话，默认逗号分割

​            2）push(): 往数组中添加一个元素



1.3、属性：

​            

1. 4、特点：

​            1）js中，数组元素的类型是可变的。数组可以放多种类型的数组

​            2）js中，数组的长度是可变的，数组下表越界之后，值是undefined

演示程序

```javascript
var arr1 = new Array(1,2,3);
    var arr2 = new Array(5);
    var arr3 = [1,2,3,4];

    document.write(arr1 + "<br>");
    document.write(arr2 + "<br>");
    document.write(arr3 + "<br>");

    arr3.push(true)
    document.write(arr3.join("--") + "<br>");
```

2、Boolean

基本数据类型boolean的包装类

3、Date

3.1、创建

​          方式1: var date = new Date()；

3.2、方法：

​            1）：toLocaleString() ,返回当前date对象对应的时间的本地字符串格式

​            2)：getTime(),  获取毫秒值，返回当前日期对象时间到1970年1月1日零点的毫秒值差

演示代码

```javascript
var date = new Date();

    document.write(date + "<br>");
    document.write(date.toLocaleString() + "<br>");
    document.write(date.getTime() + "<br>");
```

4、Math

4.1、创建：

​            \* 特点：Math对象不用创建，直接使用。Math.方法名称

4.2、方法：

​            1）random()：返回 0～1之间的随机数

​            2）ceil(x)：对数进行向上舍入

​            3）floor(x)：对数进行向下舍入

​            4）round(x)：把数四舍五入为最接近的整数

4.3、属性：

​            PI

演示代码

```javascript
 document.write(Math.PI + "<br>");
    document.write(Math.random() + "<br>");
    document.write(Math.ceil(3.12) + "<br>");
    document.write(Math.floor(4.2) + "<br>");
    document.write(Math.round(4.5) + "<br>");

    /**
     * 取 1 ～ 100 之间的随机整数
     *  1、 Math.random() 产生随机数，范围(0,1]小数
     *  2、乘以100  -- > [0，99.99999] 小数
     *  3、舍取小数部分 ， floor -- [0,99]
     *  4、加 1
     */
    var number = Math.floor(Math.random()*100) +  1;
    document.write(number);
```

5、Number



6、String



7、RegExp ：正则表达式对象

7.1 正则表达式：定义字符串组成的规则

​	1）单个字符：[ ] .如[a]，[ab] 表示a或者b,  [a-zA-Z0_9_]

​			特殊符号表示特殊含义的字符：

​				\d：单个数字字符 [0-9]

​				\w：单个单词字符 [a-zA-Z0-9_]

​	2）量词符号：

​			? ：表示出现0次或1次

​			* ：表示粗线0次或多次

​			+：表示出现1次或者多次

​			例如：\w*表示出现0个或者多个单词字符

​			{m,n}:表示数量大于等于m，小于等于n

​					* m如果缺省 {,n}：最多n次，n如果缺省 {m,}: 最少m次

​	3）开始结束符号：

​			^：开始符号

​			$：结束符号		

7.2 正则对象创建

方式1:  var reg  = new RegExp("正则表达式");

方式2:  var reg = /正则表达式/;

7.3 正则对象的方法

​		test(参数)：验证指定的字符串是否符合正则定义的规范

演示代码

```javascript
 //字符串中的反斜杠有转义作用，所以需要用\\
    var reg = new RegExp("^\\w{6,12}$");

    var reg2 = /^\w{6,12}$/;

    var username = "zhangsanerewgrwerwe"
    
    var flag = reg.test(username);

    alert(flag);
```

8、Global

8.1  特点：全局对象，这个Global中封装的方法不需要创建对象，就可以直接调用。 方法名称(参数)

8.2 方法

1）encodeURI()：url编码

2）decodeURI()：url解码

3）encodeURIComponent()：url编码

4）decodeURIComponent(()：url解码

5）parseInt()：将字符串转为数字

		* 逐一判断每一个字符是否是数字，直到不是数字为止，将前面数字部分转为number

6）isNaN()：判断一个值是否是NaN

	* NaN 六亲不认，连自己都不认，NaN参与的==比较都为false

7）：eval()：将JavaScript的字符串转化为脚本执行

9、Function：方法（函数）对象

9.1、创建

​          方式1: var func = new Function(形式参数列表,方法体);  --- 不常用

```javascript
var func1 = new Function("a","b","alert(a);")
//调用
func1(3,4);
```

​          方式2: function 方法名称(形式参数列表）{ 

​              方法体

​             }

```javascript
function func2(a,b) {
    alert(a+b);
}
//调用
func2(3,4);
```

​          方式3: var 方法名 = function(形式参数列表) {

​              方法体

​            }

```javascript
var func3 = function(a,b) {
   alert(a+b);
}
//调用
func3(3,4);
```

9.2、方法：



9.3、属性：

​            length:形参的个数

​        4、特点：

​            1）方法定义时，形参的类型不用写; 返回值类型也可以不用写

​            2) 方法是一个对象，如果定义名称相同的方法，会覆盖掉前面的方法

​            3) 在js中，方法的调用只与方法的名称有关，和方法参数列表无关

```javascript
function add (a , b) {
   return a + b;
}
 var sum = add(1,2，3);
```

​            4）在方法声明中有一个隐藏的内置对象（数组） arguments，封装了所有的参数

​			例如：需要求任意个数整数的和

```javascript
function add1() {
            var sum = 0;
            for(var i = 0; i < arguments.length ; i++) {
                sum += arguments[i];
            }
            return sum;
        }
        var sum = add1(10,11,12);
```

​        5、调用：

​            方法名称(实际参数列表);

## 四、DOM对象





## 五、BOM对象



## 六、JavaScript事件