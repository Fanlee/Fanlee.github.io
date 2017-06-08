---
title: Javascript高级程序设计-变量、作用域和内存问题
date: 2017-06-06
categories: Javascript
tags: Javascript
keywords: Javascript
comments: true

---
Javascript变量松散类型的本质，决定了它只是在特定时间用于保存特定值的一个名字而已。变量的值及其数据类型可以在脚本的生命周期内改变。
## 基本类型和引用类型的值
ECMAScript包含两种不同数据类型的值：基本类型值和引用类型值。Javascript不允许直接访问内存中的位置，在操作对象时，实际上是操作对象的引用而不是实际的对象。

- 基本类型值保存在栈内存中
- 引用类型值保存在堆内存中

### 动态的属性
定义一个基本类型值和引用类型值是一样的：创建一个变量并为该变量赋值。对于引用类型的值，我们可以为其添加属性和方法。

### 复制变量值
从一个变量向另一个变量复制基本类型值的时候，会在变量对象上创建一个新值，两个变量可以参与任何操作而不相互影响。
```
var num1 = 5;
var num2 = num1;
console.log(num1) // 5
console.log(num2) // 5
```
当从一个变量向另一个变量复制引用类型值的时候，其实复制的是对象的引用，复制操作结束后，两个变量的引用指向同一个对象。修改其中一个变量会影响到另一个变量。
```
var obj1 = {name: 'Tom'};
var obj2 = obj1;
obj2.name = 'joy';
console.log(obj1.name); //joy
```

### 传递参数
ECMAScript中所有参数都是按值传递的。
```
function setName(obj){
    obj.name = 'Tom';
    obj = new Object();
    obj.name = 'Joy';
}
var person = new Object();
setName(obj);
console.log(person.name); // 'Tom'
```
当传入的参数是对象的时候，也是按值传递，如果是按引用传递的话，当obj被赋予新的对象的时候，person也会跟着变化。

### 检测类型
检测一个变量是不是基本类型值的时候可以通过typeof操作符，注意当值为null的时候，使用typeof操作符会返回object。
```
console.log(typeof null); // object
```
检测是哪一种引用类型值的时候用instanceof
```
var a = [];
var b = new RegExp();

console.log(a instanceof Array) // true
console.log(b instanceof RegExp) //true
```
所有引用类型的值都是Object的实例，所以在检测引用类型值和Object构造函数的时候，始终返回true。
```
var a = [];
var b = new RegExp();

console.log(a instanceof Object) // true
console.log(b instanceof Object) //true
```

## 执行环境及作用域
　执行环境定义了变量或函数有权访问的其他数据。每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。
　全局执行环境是最外围的一个执行环境，在Web浏览器中，全局执行环境被认为是window对象，因此所有的全局变量和函数都是作为window对象的属性和方法创建的。某个执行环境所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数也随之被销毁。
　每个函数都有自己的执行环境，当执行流进入一个函数时，函数的环境就被推入一个环境栈中。而在函数执行之后，栈将环境弹出，把控制权交给之前的执行环境。
　当代码在一个环境中执行时，会创建变量对象和一个作用域链。作用域链的用途是保证对执行环境的有权访问和所有函数和变量的有序访问。作用域链的前端始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象作为变量对象，活动对象在最开始只包含一个变量，即arguments对象。全局执行环境的变量对象始终是作用域链中的最后一个对象。
　标识符的解析是沿着作用域链一级一级搜索标识符的过程。搜索过程始终从作用域链的前端开始。然后逐级的向后回溯。
```
var color = "red";
function changeColor(){
    var anotherColor = "blue";
    function swapColor(){
        var tempColor = anotherColor;
    };
    swapColor();
}
changeColor();
```
　上面代码有三个执行环境：全局环境、changeColor()的局部环境和swapColor()的局部环境。全局环境中有变量color和函数changeColor()，changeColor()的局部环境中包含变量anotherColor和函数swapColor()，swapColor()局部环境中包含变量tempColor。swapColor()局部环境能访问他的包含环境的所有变量，而changeColor()局部环境和全局环境就不能访问swapColor()局部环境中的变量。
　内部环境能够通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。

### 延长作用域链
下面两个语句能够在作用域链的前端增加一个变量对象。该变量对象会在代码执行后被移除出。

 - try-catch语句的catch块
 - with语句
 
with会将指定的对象添加到作用域链中，catch会创建一个新的变量，其中包含的是被抛出的错误对象的声明。
```
function bulidUrl(){
  var qs = "?debug=true";
  with(location) {
    var url = href + qs;
  }
  return url;
}
console.log(bulidUrl()) // http://localhost:63342/test/93.html?_ijt=butdjev1qf9q4jjb9qetp365sp?debug=true
```
with语句接收的是location对象，因此变量对象中就包含了所有location对象的属性和方法。其中的href就是location.href。

### 没有块级作用域
Javascript没有块级作用域，for语句初始化变量的表达式所定义的变量，在循环外部的执行环境也能访问到。
```
for(var i = 0; i < 10; i++){};
console.log(i); // 10
```

#### 声明变量
使用var声明的变量会自动被添加到最接近的环境中。如果初始化变量没有用var声明，该变量会自动被添加到全局环境。
```
function add(num1, num2){
    num = num1 + num2;
    return num;
}
console.log(add(10, 20)); // 30
console.log(num); // 30
```

#### 查询标识符
搜索过程从作用域链的前端开始，向上逐级查询与给的名字匹配的标识符。
如果在局部环境没有找到标识符，则沿着作用域链向上搜索。
```
var color = "red";
function sayColor(){
    return color;
};
console.log(sayColor()); //color
```
如果在局部环境找到标识符，则停止搜索。
```
var color = "red";
function sayColor(){
    var color = "blue"
    return color;
};
console.log(sayColor()); //blue
```

## 垃圾收集
Javascript具有自动垃圾收集机制。执行环境会负责管理代码执行过程中使用的内存。离开作用域的值将被自动标记为可回收，因此将在垃圾收集期间被删除。
### 标记清除
Javascript最常用的垃圾收集方式是标记清除。这种算法的思想是给当前不用的值加上标记，然后再回收其内存。
### 引用计数
这种算法的思想是跟踪所有的值被引用的次数。Javascript引擎目前都不再使用这种算法。

### 管理内存
优化内存占用的最佳方式是解除引用，就是一旦数据不再使用，手动将值设置为null来释放引用。

```
function createPerson(name){
    var localPerson = new Object();
    localPerson.name = name;
    return localPerson;
}
var globalPerson = createPerson('Tom');

globalPerson = null;
```
解除一个值的引用并不意味着自动回收该值所占用的内存，解除引用的正在目的是让值脱离执行环境，以便垃圾收集器下次运行时将其回收。
