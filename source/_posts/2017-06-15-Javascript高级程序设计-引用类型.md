---
title: Javascript高级程序设计-引用类型
date: 2017-06-15
categories: Javascript
tags: Javascript
keywords: Javascript
comments: true

---

# 引用类型

## Object类型
创建Object实例的方式有两种。

第一种通过new操作符后跟Object构造函数。
```
var person = new Object();
person.name = 'Tom';
```
第二种使用对象字面量使用法。
```
var person = {
    name: 'Tom'
}
```
在通过对象字面量定义对象的时候，实际上不会调用Object构造函数。

访问对象属性一般通过点表示法，也可以使用方括号表示法。
```
var person = {
    name: 'Tom'
};

console.log(person.name); // Tom
console.log(person['name']); // Tom
```
以下情况可以使用方括号表示法：

- 可以通过变量来访问属性。
```
var person = {
    name: 'Tom'
};
var propertyName = 'name';
console.log(person[propertyName]); // Tom
```
- 属性名中包含会导致语法错误的的字符，或者属性名使用的是关键字或者保留字
```
person['first name'] = 'Tom';
```

## Array类型
创建数组的基本方式有两种：第一种是通过Array构造函数。
```
var arr = new Array();
// 也可以省略new操作符
var arr = Array();
```
可以在Array()构造函数中传入参数， 有以下几种情况：

 - 传递单个数值，则会按照该数值创建给定项数的数组
 
 ```
 var arr = new Array(2);
 console.log(arr.lenght); // [undefined, undefined]
 console.log(arr.lenght); // 2 
 ```
 - 传递多个数值，则会创建包含传入数值的数组
 
 ```
 var arr = new Array(2, 3, 4);
 console.log(arr); // [2, 3, 4]
 console.log(arr.length); // 3
 ```
 - 传递其他类型的参数，则会创建包含传入参数的数组
 
 ```
 var arr = new Array("a");
 console.log(arr); // ["a"]
 console.log(arr.length); // 1
 ```
 
第二种是通过数组字面量表示法。
```
var arr = ['a', 2, 'c'];
```
可以通过length动态的为数组添加或者删除值。
```
var color = ['red', 'green', 'blue'];
// 删除值
color.length = 2;
console.log(color); //["red", "green"]
// 添加值
color[color.length] = 'black';
color[[color.length]] = 'yellow';
console.log(color); // ["red", "green", "black", "yellow"]
```

### 检测数组
检测某个对象是不是数组有以下两种方法：

 - instanceof
 
 ```
 var arr = [];
 console.log(arr instanceof Array); //true
 ```
 - Array.isArray()
 
 ```
 var arr = [];
 console.log(Array.isArray(arr)); //true
 ```
 
### 转换方法
所有的对象都具有toLocaleString()、toString()和valueOf()方法。调用toString()方法会返回由数组中每个值的字符串拼接而成的一个以逗号分割的字符串。调用valueOf()返回的是数值的本身。
```
var color = ['red', 'yellow', 'blue'];
console.log(color.toString()); // "red,yellow,blue"
console.log(color.valueOf()); // ["red", "yellow", "blue"]
```
alert()一个数组的时候，会默认在后台调用toString()方法。

### join()方法
join()方法可以接收一个参数，用作分隔符的字符串。
```
var color = ['red', 'yellow', 'blue'];
console.log(color.join('-')); // "red-yellow-blue"
console.log(color.join('+')); // "red+yellow+blue"
```

### 栈方法
栈是一种LIFO的数据结构。ECMAScript提供了push()和pop()方法，以便实现类似栈的行为。
#### push()
push()方法可以接受任意数量的参数，把他们逐步添加到末尾，返回修改后数组的长度。
```
var arr = [];
var len = arr.push('1', '2', '3');
console.log(arr); // ["1", "2", "3"]
console.log(len); // 3
```

#### pop()
pop()方法从数值末尾移除最后一项，返回移除的项。
```
var arr = ['red', 'yellow', 'blue', 'green'];
var item = arr.pop();
console.log(arr); // ["red", "yellow", "blue"]
console.log(item); // "green"
```

### 队列方法
队列数据结构访问规则是FIFO。

#### shift()
移除数组的第一项并返回该项。
```
var arr = ['red', 'yellow', 'blue', 'green'];
var item = arr.shift();
console.log(arr); //  ["yellow", "blue", "green"]
console.log(item); // "red"
```

#### unshift()
unshift()方法可以接受任意数量的参数，把他们逐步添加数组的前端，返回修改后数组的长度。
```
var arr = ['red', 'yellow'];
var item = arr.unshift('black', 'pink');
console.log(arr); //["black", "pink", "red", "yellow"]
console.log(item); // 4
```
### 重排序方法
reverse()方法能够反转数组项的顺序。
```
var arr = [1, 2, 3, 4];
var newArr = arr.reverse();
console.log(arr); // [4, 3, 2, 1]
console.log(newArr); // [4, 3, 2, 1]
```
sort()方法默认按照升序排列，排序时会调用每个数组项的toString()转型方法，然后比较得到的字符串。
```
var arr1 = [4, 2, 6, 3];
var arr2 = [5, 15, 10, 1];
console.log(arr1.sort()); // [2, 3, 4, 6]
console.log(arr2.sort()); // [1, 10, 15, 5]
```
sort()可以接收一个比较函数作为参数，比较函数有两个参数，如果第一个参数应该位于第二个参数之前返回负数，两个参数相等返回0， 第一个参数应该位于第二个参数后面返回正数。
```
let arr = [1, 10, 15, 5];
console.log(arr.sort((v1, v2) => v1 - v2)); // [1, 5, 10, 15]
console.log(arr.sort((v1, v2) => v2 - v1)); // [15, 10, 5, 1]
```
**reverse()和sort()的返回值都是经过排序后的数组。**

### 操作方法
#### concat()
concat()方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。如果传递的值不是数组，这些值就会简单的被添加到数组的末尾。
```
var let = ['red', 'yellow'];
console.log(arr.concat([1, 2], ['hello', 'hi'])); // ["red", "yellow", 1, 2, "hello", "hi"]
console.log(arr.concat('green', 'black')); // ["red", "yellow", "green", "black"]
```
#### slice()
slice() 方法返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象。原始数组不会被修改。
```
var let = ['red', 'yellow', 'green', 'black'];
console.log(arr.slice(1)); // ["yellow", "green", "black"]
console.log(arr.slice(1, 3)); // ["yellow", "green"]
```
如果该参数为负数，则用数组长度加上该数来确认位置，如果结束位置小于起始位置，则返回空数组。
```
var let = ['red', 'yellow', 'green', 'black'];
// 相当于arr.slice(0, 2)
console.log(arr.slice(-4, -2)); // ["red", "yellow"]
```
slice 方法可以用来将一个类数组（Array-like）对象/集合转换成一个数组。
```
function list(){
     // return Array.prototype.slice.call(arguments);
     return [].slice.call(arguments);
 }
 var let = list(1, 2, 3);
 console.log(arr); // [1, 2, 3]
```

#### splice()
splice() 方法通过删除现有元素和/或添加新元素来更改一个数组的内容。有以下几种用法：

 - 删除： 删除任意数量的项，只需要提供两个参数，要删除的第一项和要删除的项数。
 
 ```
 let arr1 = [1, 2, 3, 4, 5];
 let arr2 = arr1.splice(2, 2);
 console.log(arr1); // [1, 2, 5]
 console.log(arr2); // [3, 4]
 ```
 
 - 插入：可以向指定位置插入任意数量的项，只需提供三个参数，起始位置和0（要删除的项数）和要插入的项，如果要插入多项，只需在后面继续添加要插入的参数即可。
 
 ```
 let arr1 = [1, 2, 3, 4, 5];
 let arr2 = arr1.splice(2, 0, 6, 7);
 console.log(arr1); // [1, 2, 6, 7, 3, 4, 5]
 console.log(arr2); // []
 ```
 - 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需提供三个参数，起始位置，要删除的项数和要插入任意数量的项。
 
 ```
 let arr1 = [1, 2, 3, 4, 5];
 let arr2 = arr1.splice(2, 2, 6, 7);
 console.log(arr1); // [1, 2, 6, 7, 5]
 console.log(arr2); // [3, 4]
 ```
 
 splice()方法始终都会返回一个新数组，包含从原始数组中删除的项。
 
### 位置方法
indexOf()和lastIndexOf()都可以接收两个参数，要查找的项和表示查找起点位置的索引。两个方法都返回要查找的项在数组中的位置，没找到返回-1。在比较第一个参数与数组中每一项时，会使用全等操作符。
```
 var let = [1, 2, 3, 4, 5, 4, 3, 2, 1];
 console.log(arr.indexOf(4)); // 3
 console.log(arr.indexOf(4, 4)); // 5
 console.log(arr.indexOf(6)); // -1
 console.log(arr.lastIndexOf(4)); // 5
```

### 迭代方法
ECMAScript5为数组定义了5个迭代方法，每个方法都接收两个参数：要在每一项上运行的函数和运行该函数的作用域。传入这些方法的函数会接收三个参数，数组项的值，该项在数组中的位置和数组对象本身。

#### every()
对数组每一项都运行给定的函数，如果函数对每一项都返回true,则返回true。every()不会改变原数组。
```
const arr = [2, 4, 6, 8];
let everyResult = arr.every((item, index, array) => item > 1);
console.log(everyResult); // true
```

#### some()
对数组中每一项运行给的的函数，任一项返回ture，则返回true。
```
const arr = [1, 2, 3, 4];
let someResult = arr.some((item, index, array) => item > 3)
console.log(someResult); // true
```

#### filter()
对数组每一项都运行给定的函数，返回true的项组成的数组，不会改变原数组。
```
const arr = [2, 4, 6, 8];
let everyResult = arr.filter((item, index, array) => item > 5);
console.log(everyResult); // [6, 8]
```

#### forEach()
对数组中的每一项运行给定的函数，没有返回值。
```
const arr = [2, 4, 6, 8];
let pos = [];
arr.forEach((item, index, array) => {
    if (item > 5) {
        pos.push(index);
    }
});
console.log(pos); // [2, 3]
```

#### map()
对数组中的每一项运行给定的函数，返回每次函数调用结果组成的数组。
```
const arr = [1, 2, 3, 4];
let mapResult = arr.map((item, index, array) => item * 2);
console.log(mapResult); // [2, 4, 6, 8]
```

### 缩小方法
reduce()和reduceRight()这两个方法都会迭代数组所有项，然后构建一个最终返回值。

这两个方法都接收两个参数，一个在每一项上调用的函数和作为缩小基础的初始值。传给reduce()和reduceRight()的函数接收四个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。
```
const arr = [1, 2, 3, 4, 5];
let result = arr.reduce((prev, cur, index, array) => prev + cur);
console.log(result); // 15

let result = arr.reduce((prev, cur, index, array) => prev + cur, 20);
console.log(result); // 35
```
和reduceRight()与reduce()作用类似，只不过方向相反而已。

## Date类型
要创建一个日期对象，使用new操作符和Date构造函数即可，新创建的对象会自动获取当前日期和时间。
```
var now = new Date();
```
想根据特定的日期和时间创建日期对象，需要传入该日期的毫秒数。
```
console.log(new Date(600000000000)); // Thu Jan 05 1989 18:40:00 GMT+0800 (中国标准时间)
```
### Date.parse()
Date.parse() 方法解析一个表示某个日期的字符串，并返回从1970-1-1 00:00:00 UTC 到该日期对象（该日期对象的UTC时间）的毫秒数，如果该字符串无法识别，或者一些情况下，包含了不合法的日期数值（如：2015-02-31），则返回值为NaN。
```
var str = Date.parse('2017-06-15');
var str = Date.parse('June 15, 2017'); 
var str = Date.parse('6/15/2017'); 
console.log(str); //1497484800000
```
将表示日期的字符串传给Date构造函数，会在后台默认调用Date.parse()。

### Date.UTC()
Date.UTC() 方法接受的参数同日期构造函数接受最多参数时一样，返回从1970-1-1 00:00:00 UTC到指定日期的的毫秒数。
```
var utcDate = new Date(Date.UTC(96, 11, 1, 0, 0, 0));
console.log(utcDate); // Sun Dec 01 1996 08:00:00 GMT+0800 (中国标准时间)
```

### Date.now()
返回调用这个方法时的日期和时间的毫秒数。
```
 var now = Date.now();
 console.log(now); // 1497515369177
```

### valueOf()
返回日期的毫秒数。
```
 var now = new Date();
 console.log(now.valueOf()); // 1497515607459
```

### 日期/时间组件方法
日期/时间组件方法有下图这些。
![mark](http://optwq0urg.bkt.clouddn.com/blog/20170615/164010994.png?imageslim)

![mark](http://optwq0urg.bkt.clouddn.com/blog/20170615/164435910.png?imageslim)