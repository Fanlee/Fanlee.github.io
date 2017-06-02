---
title: Javascript高级程序设计-基础概念
date: 2017-06-02
categories: 学习笔记
tags: Javascript
keywords: Javascript
comments: true

---

## 语法
### 区分大小写
ECMAScript是区分大小写的，包括变量，函数名和操作符。
### 标识符
标识符指的是函数，变量，属性的名字。必须要遵循以下规则：

 1. 第一个字符必须是字母、下划线、或者美元符号
 2. 其他字符可以是字符、下划线、美元符号或者数字

标识符推荐采用驼峰大小写格式。
### 注释
单行注释
```
// 单行注释
```
块级注释
```
/*
 * 块级注释
 */
```
### 严格模式
除了正常运行模式，ECMAScript5添加了第二种正常运行模式：“严格模式”。
设立严格模式的目的：
 * 消除Javascript语法一些不合理、不严谨的地方，减少一些怪异的行为
 * 消除代码运行的一些不安全之处，保证代码运行的安全
 * 提高编译器效率，增加运行速度
 * 为未来的新版本Javascript做好铺垫

严格模式有两种调用方法：

 1. 整个脚本启用严格模式
 
 ```
 "use strict";
 ```
 2. 函数内部启用严格模式
 
 ```
 (function(){
    "use strict";
 }())
 ```
不管在什么情况下启用严格模式都必须放在第一行，否则不会生效。
### 语句
ECMAScript中的语句以分号结尾，虽然可以不写，但是不推荐。分号加与不加完全取决于个人习惯，但为了代码稳定（解析出错）还是建议使用分号断句。
Javascript自动加分号规则：
 
 1. 当有换行符（包括含有换行符的多行注释），并且下一个token没法跟前面的语法匹配时，会自动补分号。
 2. 当有}时，如果缺少分号，会补分号。
 3. 当程序源代码结束时，如果缺少分号，会补分号。
结论：
1. 在return、break、continue、后自增、后自减五种语句中，换行符可以完全替代分号的作用。
2. var、if、do、while、for、continue、break、return、with、switch、throw、try、 debugger几种关键字开头的语句，以及空语句，上一行加不加分号影响不大。
3. 凡表达式语句和函数表达式语句，后面不加分号非常危险，情况极其复杂。
4. 凡(和[开头的语句，前面不加分号极度危险。

### 变量
ECMAScript变量是松散类型的，可以用来保存任何类型的数据。声明变量要使用var操作符后跟变量名。
```
var message;
```
上面代码是未初始化变量，会保存特殊值“undefined”。
也可以在声明的时候直接初始化变量。
```
var message = "hi";
```
使用var声明的变量会成为定义该变量作用域的局部变量。在函数中通过var声明的变量会在函数退出后销毁。
```
function test(){
  var a = 1;
};
test();
console.log(a) // Uncaught ReferenceError: a is not defined
```
上面代码如果省略var就会创建一个全局变量，在全局作用于内任何地方都能访问到。
```
function test(){
  a = 1;
};
test();
console.log(a) // 1
```
一条语句可以定义多个变量，通过逗号分割开。
```
var a = 1,
    b = 2,
    c = "hello";
```
严格模式下，定义名为eval和arguments的变量会报错。
```
"use strict";
var eval = "hello" // Uncaught SyntaxError: Unexpected eval or arguments in strict mode
```
## 数据类型
ECMAScript有5种基本数据类型：Undefined、Null、Boolean、Number、String和一种引用数据类型：Object。
### typeof操作符
用来检测变量的数据类型
```
var a;
var b = 'hi';
var c = 1;
var d = true;
var e = {};
var f = function(){};
var g = new RegExp;
var h = null;
console.log(typeof a); // undefined
console.log(typeof b); // string
console.log(typeof c); // number
console.log(typeof d); // boolean
console.log(typeof e); // object
console.log(typeof f); // function
console.log(typeof g); // object
console.log(typeof h); // object
```
typeof null会返回object,因为null被认为是一个空的对象。
### Undefined类型
使用var声明变量时未进行初始化，这个变量的值就是undefined。主要用来区分空对象指针与未经初始化的变量。
未初始化的变量和未定义的变量是不一样的，直接使用未定义的变量会报错。
```
var a;
console.log(a); // undefined
console.log(b); // Uncaught ReferenceError: b is not defined
```
### Null类型
null表示一个对象的空指针，如果定义的变量将来用于保存对象，最好将该变量初始化为null。
undefined的值派生自null，所以相等测试返回true。
```
console.log(undefined == null); // true
```
### Boolean类型
Boolean类型有两个字面值：true和false。ECMAScript所有类型的值都能通过Boolean()转型函数转换为Boolean值。
转换规则如下：

| 数据类型        | 转化为true的值 | 转化为false的值 |
| -------------   |:--------------:| :--------------:|
| Boolean         | true           | false           |
| String          | 非空字符串     | ""（空字符串）  |
| Number          | 非零数字       | 0和NaN          |
| Object          | 任何对象       | null            |
| Undefined       | 不能转化为true | Undefined       |
### Number类型
Number类型使用IEEE754来表示整数和浮点数。
整数可以通过十进制、八进制、十六进制来表示。
八进制第一位必须是0，后面是八进制数字序列（0~7），如果数值超出了范围，则省略前导的0，后面数字以十进制解析。**八进制在严格模式下无效，会抛出异常**。
```
console.log(070) // 56
console.log(079); // 79
```
十六进制前两位必须是0x开头，后面是十六进制数字序列（0~7及A~F）。
```
console.log(0xA); // 10
```
#### 浮点数值
浮点数值需要的内存空间是整数的两倍，所以ECMAScript会将小数点后没有数字的（如1.）和本身就表示一个整数的（如1.0）的浮点数值转化为整数。
可以用科学计数法表示极大或者极小的浮点数值。默认情况下ECMAScript会将小数点后带有6个零以上的浮点数在转化为e表示法。如：
```
console.log(3.12e7) // 31200000
console.log(3.12e-6) // 0.00000312
console.log(3.12e-7) // 3.12e-7
```
浮点数值在任何机器的上都不能直接判断相等，最好通过精度来判断。
#### 数值范围
`ECMAScripot`的数值范围在`Number.MIN_VALUE`和`Number.MAX_VALUE`之间，如果超出这个范围， 将会被转换成`Infinity`值，该值不能用于计算，要想确定一个值是否是有穷的，通过`isFinite()`判断，函数参数位于最大值与最小值之间返回`true`。
```
console.log(Number.MIN_VALUE); // 5e-324
console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
console.log(isFinite(Number.MIN_VALUE + Number.MIN_VALUE)); // false
```
#### NaN
这个数值表示一个本来要返回数值的操作数未返回数值的情况。

 1. 任何涉及到NaN的操作都会返回`NaN`
 2. `NaN`与任何值都不相等，包括自身
```
console.log(NaN === NaN) // false
```
`ECMAScript`定义了`isNaN()`函数，该函数接收一个参数，会尝试将这个参数转换成数值，转换成功返回`false`, 失败返回`true`。
```
console.log(isNaN(NaN)); // true
console.log(isNaN(10)); // false
console.log(isNaN('10')); // false
console.log(isNaN('color')); // true
console.log(isNaN(true)); // false
```

#### 数值转换
有三个函数可以把非数值转换为数值：`Number()`、`parseInt()`和`parseFloat()`。`Number()`可用于任何数据类型，另两个则专门用于把字符串转换成数值。

`Number()`转换规则如下：

 1. 如果是Boolean值，则把`true`和`false`转换成1和0
 2. 如果是数值，返回本身
 3. 如果是`Null`， 返回0
 4. 如果是`undefined`，返回`NaN`
 5. 如果是字符串，则有一下几种情况
    - 如果字符串中只包含数字，则转换成对应十进制
    
    ```
    console.log(Number("10")); // 10
    console.log(Number("+10")); //10
    console.log(Number("-10")); // -10
    console.log(Number("010")); // 10
    ```
    - 如果字符串中包含有效的浮点格式，则转换成对应的浮点数值。
    
    ```
    console.log(Number("10.3")); // 10.3
    console.log(Number("+10.3")); // 10.3
    console.log(Number("-10.3")); //-10.3
    console.log(Number("010.3")); // 10.3
    ```
    - 如果字符串中包含有效的十六进制格式，则转换成相同大小的十进制整数值。
    
    ```
    console.log(Number("0xF")); // 15
    ```
    - 如果字符串为空，则转换为0
    
    ```
    console.log(Number(" ")); // 0
    ```
    - 字符串中包含除上述格式之外的字符， 则转换为`NaN`
 6. 如果是对象， 则调用对象的`valueOf()`方法，然后按照前面规则转换，如果转换结果是`NaN`，则调用对象的`toString()`方法，再次依照前面规则转换。日期对象会转换成相应的毫秒数。
 
```
let now = new Date();
console.log(Number(now)); // 1495076392196
```

`parseInt()`函数在转换字符串时会忽略字符串前面的空格，直到找到第一个非空格字符串。如果第一个字符不是数字字符或者负号，就会返回`NaN`，如果第一个字符是数字字符，`parseInt()`会继续解析后面的字符，直到解析完所有字符或者遇到非数字字符。`parseInt()`解析空字符串会返回`NaN`。同样，`parseInt()`也能辨别各种整数格式。
```
console.log(parseInt("")); // NaN
console.log(parseInt("123abc")); // 123
console.log(parseInt("abc123")); // NaN
console.log(parseInt("10.2")); // 10
console.log(parseInt("070")); // 70 (ECMAScript5已经不具备解析八进制的能力，所以认为前导的0无效)
console.log(parseInt("0xFWQR")); // 15
```
`parseInt()`可以接受第二个参数来指定整数的格式。默认是转换成十进制。
```
console.log(parseInt("10", 2)); // 2
console.log(parseInt("10", 8)); // 8
console.log(parseInt("10", 10)); // 10
console.log(parseInt("10", 16)); // 16
```
`parseFloat()`函数与`parseInt()`类似，不同的是`parseFloat()`解析完成或者遇到第一个无效的浮点数字字符为止，但是第一个小数点是有效的。`parseFloat()`始终会忽略前导的零，十六进制的字符串始终会被转换成0，由于`parseFloat()`只解析十进制值，所以不能指定第二个参数，如果字符串包含的是一个可解析成整数的数值，`parseFloat()`会返回整数。
```
console.log(parseFloat("123abc")); // 123
console.log(parseFloat("0xF")); // 0
console.log(parseFloat("10.5")); // 10.5
console.log(parseFloat("10.5.5")); // 10.5
console.log(parseFloat("010.5")); // 10.5
console.log(parseFloat("10.0")); // 10
```
### String类型
`String`由零或者多个16为Unicode字符组成的字符序列，可以用双引号或者单引号表示。
#### 字符串的特点
字符串是不可变的，要改变某个变量保存的字符串首先要先销毁原来的字符串。然后用新值填充该变量。
#### 转换为字符串
可以通过`toString()`将`Number`、`Boolean`、`Object`和`String`类型的数据转换为字符串，该方法可以接收一个参数：输出数值的基数。
在不知道值是不是`Null`或`Undefined`的情况下，可以通过`String()`方法进行转换。转换规则如下：

 - 如果值有`toString()`方法，则直接调用该方法
 - 如果值为`null`，返回`null`
 - 如果值为`undefined`，返回`undefined`
 
### Object类型
ECMAScript对象就是一组数据和功能的集合。
可以通过`new`操作符后跟要创建的对象类型的名称来创建。创建`Object`类型的实例并为其添加属性和方法就可以创建自定义对象。
```
  var person = new Object();
  person.name = "Tom";
  person.sayName = function(){
    alert(this.name);
  }
  person.sayName(); // Tom
```
`Object`类型是所有它的实例的基础，它所具有的所有属性和方法都能被实例所共享。
`Object`每个实例都有一下属性和方法：
 - constructor: 保存着用于创建当前对象的函数
```
var obj = new Object();
console.log(obj.constructor === Object) // true
```
 - hasOwnProperty(): 检查给定属性是否存在当前实例中，如果存在于原型中返回true，实例本身属性返回false
```
function Person(){};
Person.prototype.name = "Tom";
var person = new Person();
person.age = 20;
console.log(Person.hasOwnProperty("name")); // true
console.log(Person.hasOwnProperty("age")); // false
```
 - isPrototypeOf(): 用于检查传入的对象是否是另一个对象的原型
```
function Foo(){};
function Bar(){};
Foo.prototype = new Bar();
var f1 = new Foo();
console.log(Bar.prototype.isPrototypeOf(f1)) // true
```
 - propertyIsEnumerable(): 检查给定的属性能否通过`for-in`来枚举
    1. 这个属性必须属于实例的，并且不属于原型。
    2. 这个属性必须是可枚举的，也就是自定义的属性。
    3. 如果对象没有指定的属性，该方法返回false
 
```
function Person(){};
Person.prototype.name = 'Tom';
var p1 = new Person();
p1.age = 20;
for(var attr in p1) {
console.log(attr) // age name
}
console.log(p1.propertyIsEnumerable('age'))  // true
console.log(p1.propertyIsEnumerable('name')); // false
console.log(p1.propertyIsEnumerable('toString')); //false
```
 - toLocaleString(): 返回对象的字符串表示法，该字符串与执行环境的地区对应
 - toString(): 返回对象的字符串表示
 - valueOf(): 返回对象的字符串、数值和布尔值表示

## 操作符
### 一元操作符
只能操作一个值的操作符叫做一元操作符。分为递增和递减操作符，递增和递减操作符又有前置型和后置型两种。
前置型：递增和递减操作在语句求值之前执行
```
var num1 = 2,
    num2 = 10;
var num3 = --num1 + num2;
var num4 = num1 + num2;
console.log(num3); // 11
console.log(num4); // 11
```
后置型：递增和递减操作在语句求值之后执行
```
var num1 = 2,
    num2 = 10;
var num3 = num1-- + num2;
var num4 = num1 + num2;
console.log(num3); // 12
console.log(num4); // 11
```
这四个操作符对任何值都适用，内部默认调用Number()进行转换。
#### 一元加和减操作符
一元加操作符以一个加号表示，放在数值前面。对数值不会产生任何影响。
```
var num = 20;
num = +num;
console.log(num) // 20
```
如果是非数值则内部调用`Number()`进行数据类型转化。

一元减操作符用于将一个数值转化为负数
```
var num = 20;
num = -num;
console.log(num) // -20 
```
如果是非数值则先内部调用`Number()`进行数据类型转化，然后再变为负数。
### 位操作符
`ECMAScript`中所有数值都以IEEE-754 64位格式存储，但位操作符不直接操作64位的值，而是将64位值转换为32位的整数，操作完成后再转换为64位。
对于有符号的整数，前31位表示整数的值，第32位表示符号：0表示正数，1表示负数。表示符号的叫符号位。正数以纯二进制格式存储，31位中的每一位都表示2的幂，没有用到的位以0填充，忽略不计。
二进制转化成十进制如下
```
10010 => (2⁴*1)+(2³*0)+(2²*0)+(2¹*1)+(2⁰*0)
            16 +  0   +   0  +   2   +  0 = 18
```
负数是用二进制的补码进行存储。计算一个数值的二进制补码需要经过下面步骤：
 1. 求这个数值绝对值的二进制码
 2. 求二进制的反码，即0替换成1，1替换成0
 3. 得到二进制反码加1

求-18的二进制码步骤如下：

```
(1)先取到18的二进制码
0000 0000 0000 0000 0000 0000 0001 0010
(2)取其反码
1111 1111 1111 1111 1111 1111 1110 1101
(3)反码加一
1111 1111 1111 1111 1111 1111 1110 1101
                                      1
----------------------------------------
1111 1111 1111 1111 1111 1111 1111 1110                         
```
#### 按位非（~）
执行按位非的结果返回数值的反码，按位非的本质是操作数的负值减1。
```
var num1 = 2;
var num2 = ~num1;
console.log(num2); // -3
```
浮点数取整
```
console.log(~~3.14); // 3
console.log(~~-3.14); //-3
```
indexOf判断字符是否存在
```
  var str = "hello";
  if(~str.indexOf('e')){
    ...
  }
```
#### 按位与（&）
按位与的操作是将两个数值的每一位对齐，两个数的值为1时，才返回1，其他为零。
```
    var result = 3 & 5;
    console.log(result) // 1
    
    0011
  & 0101
  ------
    0001
 
```
可以用一个数和1进行按位&操作来判断一个数是奇数还是偶数
```
function assert(n) {
    if (n & 1) {
      console.log("奇数");
    } else {
      console.log("偶数");
    }
}
```
因为奇数的最后一位肯定是1，而1只有最后一位为1，按位&操作之后，结果肯定只有最后一位数为1。而偶数的二进制表示的最后一位数是0，和1进行按位&操作，结果所有位数都为0。
#### 按位或（|）
只要两个数中有一个数为1，结果就为1，其他则为0。
```
  0001
| 0011
------
  0011
```
可以用来对浮点数向下求整
```
var num = 1.1 | 0; // 1
```
#### 按位异或（^）
按位异或是两个数中只有一个1时返回1，其他情况返回0。
```
    0001
  ^ 0011
  -------
    0010
```
可以用来调换两个数值的值。
```
var num1 = 1,
    num2 = 2;
num1 ^= num2;
num2 ^= num1;
num1 ^= num2;
console.log(num1); // 2
console.log(num2); // 1
```
#### 有符号左移（<<）
有符号左移会将32位二进制数的所有位向左移动指定位数。如：
```
var num = 2; // 二进制10
num = num << 5; // 二进制1000000，十进制64
```
如果要求2的n次方，可以这样：
```
function power(n) {
    return 1 << n;
}
power(5); // 32
```
#### 有符号右移（>>）
有符号右移会将32位二进制数的所有位向右移动指定位数。如：
```
var num = 64; // 二进制1000000
num = num >> 5; // 二进制10，十进制2
```
可以用来求一个数的二分之一
```
var num = 10 >> 1; // 5
```
#### 无符号右移（>>>）
正数的无符号右移与有符号右移结果是一样的。负数的无符号右移会把符号位也一起移动，而且无符号右移会把负数的二进制码当成正数的二进制码：
```
var num = -64; // 11111111111111111111111111000000
num = num >>> 5; // 134217726
```
可以利用无符号右移来判断一个数的正负：
```
function isPos(n) {
return (n === (n >>> 0)) ? true : false; 	
}

isPos(-1); // false
isPos(1); // true
```
-1>>>0虽然没有向右移动位数，但-1的二进制码已经变成了正数的二进制码,所以不会相等。

应该尽量少用位操作符进行运算，这会导致代码阅读困难。

### 布尔操作符
布尔操作符一共有三个：非（!）、与（&&）、或（||）。
#### 逻辑非（!）
逻辑非会将操作数转化为布尔值，然后对其求反。转换规则与`Boolean()`函数类似。
`!!`会将一个数转换为Boolean值。
```
console.log(!!"hello"); // true
console.log(!!0); // false
console.log(!!NaN); // false
console.log(!!""); // false
console.log(!!10); // true
```
#### 逻辑与（&&）
逻辑与有两个操作数。只要其中有一个为false，则返回false。
在有一个操作数不是布尔值的情况下，逻辑与操作不一定返回布尔值，遵循以下规则：

 - 第一个操作数是对象，则返回第二个操作数
 - 第二个操作数是对象，则在第一个操作数求值结果为true的情况下返回该对象
 - 两个操作数都是对象，则返回第二个
 - 如果有一个操作数为null,则返回null
 - 如果有一个操作数为NaN,则返回NaN
 - 如果有一个操作数为Undefined,则返回Undefined
 
逻辑与属于短路操作，第一个操作数为false，就不会对第二个操作数求值。

#### 逻辑或（||）
逻辑或有两个操作数。只要其中有一个为true，则返回true。

在有一个操作数不是布尔值的情况下，逻辑或操作不一定返回布尔值，遵循以下规则：
 - 第一个操作数是对象，则返回第一个操作数
 - 第一个操作数求值结果为false，则返回第二个操作数
 - 两个操作数都是对象，则返回第一个
 - 如果有一个操作数为null,则返回null
 - 如果有一个操作数为NaN,则返回NaN
 - 如果有一个操作数为Undefined,则返回Undefined
 
逻辑或也属于短路操作，第一个操作为true，就不会对第二个操作数求值。
可以利用这一行为避免为变量赋null和undefined值。
```
var obj = null;
var result = obj || {};
console.log(result); // object
```