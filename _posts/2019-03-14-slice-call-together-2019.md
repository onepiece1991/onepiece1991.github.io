---
layout:     post
title:      "[].slice.call()方法"
subtitle:   "javascript methods"
date:       2019-03-14 18:41:00
author:     "Jxx"
header-img: "img/post-bg-180006.jpg"
tags:
    - javascript
---


**前言：**

前些天看到[link rel=alternate网站换肤功能最佳实现](https://www.zhangxinxu.com/wordpress/2019/02/link-rel-alternate-website-skin/)
里面有用到[].slice.call()方法，于是去查了下

先摆上结论：**`[].slice.call(arguments)`能将具有`length`属性的对象转成数组**。

# 1、基础知识
## 1.1 `slice()`方法

### 定义和用法
  `slice(start, end) `方法可提取数组的某个部分，并以新的数组返回被提取的部分(浅拷贝)。原始数组不会被改变。   
  使用`start`（包含） 和 `end`（不包含） 参数来指定提取数组开始和结束的部分。

- 如果未指定`start`和`end`，则返回整个数组。
- 如果指指定一个参数，该参数作为`start`使用，返回包括`start`位置之后的全部数组。
- 如果是负数，则该参数规定的是从数组的尾部开始算起的位置。也就是说，-1 指数组的最后一项，-2 指倒数第二个项，以此类推。

### 实例

```javascript
//在数组中读取元素：
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(-2,-1);
citrus 结果输出:
["Apple"]
```

## 1.2  call()方法

### 定义和用法
`call()`函数用于调用当前函数`functionObject`，并可同时使用指定对象`thisObj`作为本次执行时`functionObject`函数内部的`this`指针引用。

### 实例

```javascript
var obj = {name: "李四", age: 20};
function foo(a, b){
    document.writeln(this.name);    
    document.writeln(a);    
    document.writeln(b);    
}
// 改变this引用为obj，同时传递两个参数
foo.call(obj, 12, true); // 李四 12 true
```

# 2、进入正题
`arguments`是一个对象而不是数组，与数组类似，最多算是一个伪数组，而且自身的原型链上也没有`slice`这个方法。

`Array.prototype.slice.call(arguments, 1)`可以理解成是让`arguments`转换成一个数组对象，让`arguments`具有`slice()`方法。要是直接写`arguments.slice(1)`会报错。

```javascript
(function(a,b,c){
   console.log(arguments.length);           //3
   console.log(typeof arguments);           //Object
   console.log(arguments instanceof Object);    //true

}(1,2,3))
```

 [ ]自身也是也是一个对象.而数组原型链上有这个slice这个方法。

```javascript
 /*此处的返回值是true*/
   [].slice === Array.prototype.slice;
```

- 通过call显式绑定来实现`arguments`变相有`slice`这个方法。
- 这就是说：`Array.prototype.slice.call(arguments)`这句里，就是把 arguments 当做当前对象，要调用的是 `arguments` 的`slice` 方法

### 补充：
将函数的实际参数转换成数组的方法

方法一：
```javascript
var args = Array.prototype.slice.call(arguments);
```

方法二：
```javascript
var args = [].slice.call(arguments, 0);
```

方法三：
```javascript
var args = []; 
for (var i = 1; i < arguments.length; i++) { 
    args.push(arguments[i]);
}
```

# 3.最后，附个**转成数组的通用函数**
```javascript
var toArray = function(s){
    //try语句允许我们定义在执行时进行错误测试的代码块。
   //catch 语句允许我们定义当 try 代码块发生错误时，所执行的代码块。
    try{
        return Array.prototype.slice.call(s);
    } catch(e){
        var arr = [];
        for(var i = 0,len = s.length; i < len; i++){
            //arr.push(s[i]);
               arr[i] = s[i];  //据说这样比push快
        }
         return arr;
    }
}
```

参考资料：
[Array.prototype.slice.call()方法讲解](https://www.cnblogs.com/dingxiaoyue/p/4948166.html)

[Array.prototype.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

[JavaScript arguments对象](http://www.cnblogs.com/lwbqqyumidi/archive/2012/12/03/2799833.html)

[Function.prototype.call()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)