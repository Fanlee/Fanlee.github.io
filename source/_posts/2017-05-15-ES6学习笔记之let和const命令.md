---
title: ES6学习笔记之let和const命令
date: 2017-05-15
categories: 学习笔记
tags: javascript
keywords: ES6
description: ES6
comments: true
---

最近一直在看阮大神的[ECMAScript 6入门][1]这本书，边学边记，加深印象。


----------


# <center>let和const命令</center>
ES5只有两种声明变量的方法：var命令和function命令。而ES6新增了四种声明变量的方法:let、const、import和class，所以ES6有6种声明变量的方法。
## let命令


----------
### 基本用法
let用来声明变量， 用法类似于var，但是只在let命令所在的代码块有效。
```
for(var i  = 0; i < 10; i++){};
console.log(i) // 10
for(let i = 0; i < 10; i++){}
console.log(i) // 报错
```
### 不存在变量提升
var声明的变量的能在变量声明之前访问，值为undefined，因为在脚本运行之前，变量foo已经存在，但是没有值，所以会输出undefined。
```
console.log(a); // undefined
var a = 'hello';
```
如果用let来声明的变量在没有声明之前就进行访问的话，会抛出异常。
```
console.log(a); // Uncaught ReferenceError: a is not defined
var let = 'hello';
```
### 暂时性死区
在代码块内，使用let命令声明变量之前，该变量都是不可用的。
```
if(true){
  tmp = "hello";
  console.log(tmp); // ReferenceError
  let tmp;
  console.log(tmp); // undefined
  tmp = "abc";
  console.log(tmp)
}
```
上图说明在第四行用let声明变量tmp之前，都属于tmp的死区。
**注意**
**如果在let声明变量之前用typeof检测数据类型的话，会抛出异常。**
```
console.log(typeof x); // ReferenceError
let x = 1;
```
### 不允许重复声明
let不允许在相同的作用域内重复声明同一个变量
```
function fn(arg){
  let arg;
}
```
上面代码会抛出异常，改成下面这样就不会了。
```
function fn(arg){
  {
    let arg;
  }
}
```


----------

## 块级作用域
ES5只有全局作用域和函数作用域,而ES6新增了块级作用域。
1. ES6允许块级作用于任意嵌套
```
{{{{{let a = 0}}}}}
```
上图使用了5五层块级作用域。
2. 外层作用域无法读取内层作用域的变量
```
{{{{
  {let a = 0}
  console.log(a); // ReferenceError
}}}}
```
3. 不同作用域可以定义同名变量
```
{{{{
  let a = 1;
  {
    let a = 0;
    console.log(a) // 0
  }
  console.log(a) // 1
}}}}
```


----------
## const命令
### 基本用法
const声明一个只读的常量，一旦声明，值就不可以改变。
```
const PI = 3.1415;
PI = 3.14;
console.log(PI) // Assignment to constant variable
```
const一旦声明变量，必须立即初始化。
```
const foo ;
foo = "hello";
console.log(foo); // Missing initializer in const declaration
```
const和let相同，声明的变量只有在其所在的块级作用于有效，变量不会提升，同时也存在“暂时性死区”。
### 本质
>const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。

意思就是说用const声明的基本数据的变量是不可以改变值，而引用数据类型（主要是对象和数值）,const只能保证指针是固定的，而指向的数据是可以进行变化的。
```
const foo = {};
foo.name = "Tom";
console.log(foo); // {name: "Tom"}

foo = {}; // 当把foo指向另一个对象的时候就会报错
```
```
const arr = [];
arr.push("hello");
console.log(arr); ["hello"]

arr = []; // Assignment to constant variable.
```

如果要冻结一个对象，应该使用*Object.freeze*方法。
```
const foo = Object.freeze({});
foo.name = "Tom";
console.log(foo) // {} 
```
常规模式下，添加属性不起作用
```
"use strict";
const foo = Object.freeze({});
foo.name = "Tom";
console.log(foo) // Cannot add property name, object is not extensible
```
除了将对象本身冻结，对象的属性也应该冻结。下面是一个将对象彻底冻结的函数 - . -暂时没有理解到这个方法，先直接拷过来用。
```
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```
在严格模式下会抛出异常
  [1]: http://es6.ruanyifeng.com/