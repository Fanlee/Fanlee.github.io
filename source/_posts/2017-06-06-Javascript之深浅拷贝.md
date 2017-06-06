---
title: Javascript之深浅拷贝
date: 2017-06-06
categories: Javascript
tags: Javascript
keywords: Javascript
comments: true

---

在JavaScript中，对对象进行拷贝的场景比较常见。但是简单的复制语句只能对对象进行浅拷贝，即复制的是一份引用，而不是它所引用的对象。而更多的时候，我们希望对对象进行深拷贝，避免原始对象被无意修改。对象的深拷贝与浅拷贝的区别如下：

 - 浅拷贝：仅仅复制对象的引用，而不是对象本身
 - 深拷贝：把复制的对象所引用的全部对象都复制一遍

## 浅拷贝的实现
### 简单的复制语句
```
function simpleClone(target) {
    var obj = {};
    for (var attr in target) {
      obj[attr] = target[attr];
    }
    return obj;
};
```
### Object.assign()
Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。但是 Object.assign() 进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。
```
var obj = { a: {a: "hello", b: 21} };
var newObj = Object.assign({}, obj);
```

## 深拷贝的实现
### JSON.parse()
最偷懒，也是浏览器层面优化比较好的方式，就是使用JSON的解析和序列化API,既简单，效率也非常高（v8层面又C++实现的解析引擎）:
```
JSON.parse(JSON.stringify(target));
```
但是，这种拷贝的方法存在以下问题:

- 一旦对象中存在函数类型（Function），此解析器会自动忽略掉Function
- 字符串化之后必须是标准类型的JSON格式
- 原型链丢失

这种拷贝方式，最适合纯数据类型的对象或者数组。

### 递归拷贝
```
function deepClone(initalObj, finalObj) {
    var obj = finalObj || {};
    for (var attr in initalObj) {
      var prop = initalObj[attr];
      // 避免相互引用对象导致死循环，如initalObj.a = initalObj的情况
      if (prop === obj) {
        continue;
      }
      if (typeof prop === 'object') {
        obj[attr] = (prop.constructor === Array) ? [] : {};
        arguments.callee(prop, obj[attr]);
      } else {
        obj[attr] = prop;
      }
    }
    return obj;
}
```

### Object.create()
```
function deepClone(initalObj, finalObj) {
    var obj = finalObj || {};
    for (var i in initalObj) {
        var prop = initalObj[i];
 
        // 避免相互引用对象导致死循环，如initalObj.a = initalObj的情况
        if(prop === obj) {
            continue;
        }
 
        if (typeof prop === 'object') {
            obj[i] = (prop.constructor === Array) ? [] : Object.create(prop);
        } else {
            obj[i] = prop;
        }
    }
    return obj;
}
```