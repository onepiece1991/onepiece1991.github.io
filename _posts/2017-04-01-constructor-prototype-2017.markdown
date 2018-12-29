---
layout:     post
title:      "JS重写构造函数后原型prototype中的constructor的指向问题"
subtitle:   ""
date:       2017-04-01 11:00:00
author:     "Jxx"
header-img: "img/post-bg-180006.jpg"
tags:
    - javascript
---
 
- 前言 

构造函数的prototype属性指向它的prototype对象，也就是原型对象，在原型对象中有一个constructor属性，指向该构造函数。但是我们在使用构造函数时，一般会重写它的原型，会导致constructor指向出问题。
```javascript
function Animal() {}
Animal.prototype.say = function () {
    console.log('wang');
}
var dog = new Animal();
Animal.prototype = {
    say : function () {
        console.log('miao');
    }
}
var cat = new Animal();
```
步骤：  
1. 一个构造函数Animal()，在重写原型之前给原型添加了一个方法say(),并且实例化一个对象dog；
2. 重写原型，实例化对象cat。
```javascript
console.log(dog.constructor); //ƒ Animal() {}
console.log(cat.constructor); //ƒ Object() { [native code] }
```

我们可以看到，重写原型之前实例的对象的dog.constructor依然指向构造函数，而cat.constructor却指向了Object`??`

我们重写原型的代码如下
```javascript
Animal.prototype = {
    say : function () {
        console.log('miao');
    }
}
```
这样写，是在改变构造函数的prototype属性的指向，让其指向我们写的一个对象，但是并没有覆盖原来的原型对象，原来的原型对象依然保留在内存中
```javascript
dog.say(); //wang
cat.say(); //miao
```
执行dog.say()输出animalLanguage说明重写之前的原型对象依然存在，只是Animal构造函数的prototype属性不再是它的引用。其实dog.--proto--依然指向它。

当我们调用cat.constructor时，cat对象在Animal的原型中(此时的原型是我们写的那个对象)没有找到constructor，而在我们写的那个对象中的--proto--中倒是有一个constructor，它当然指向的是创建它的Object，所以最后在原型链中找到了constructor的值并给我们返回的是一个function Object()，**所以我们在重写原型时，应该去指定它的constructor属性**
```javascript
Animal.prototype = {
     constructor : Animal, 
     say : function () {
         console.log('miao');
     }
 }
```