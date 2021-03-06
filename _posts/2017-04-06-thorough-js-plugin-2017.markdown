---
layout:     post
title:      "js写插件教程---深入"
subtitle:   ""
date:       2017-04-06 16:00:00
author:     "Jxx"
header-img: "img/post-bg-180006.jpg"
tags:
    - javascript
---
 
### 1. 具有安全作用域的构造函数

当不是用new调用构造函数的时候(如直接使用Fns(1))，程序会报错
```javascript
;(function(win){
    var Fns = function(name) {
        //instanceof运算符用于测试构造函数的prototype属性是否出现在对象的原型链中的任何位置
        if(!(this instanceof Fns)){
            //只要不是new的，用Fn()直接调用的，这里的this绝对不指向Fn；
            //让它重新new一下，return后继续执行代码就会执行else里的内容，故把this.a 放到了else里
            return new Fns(name)
        }else{
            this.a = name;
            this.init();
        }
    }
    Fns.prototype = {
        constructor:Fns,
        init: function(){
            var _self = this;
            _self.getFns();
        },
        getFns: function() {
            console.log('getFns');
        }
    }
    win.Fns = Fns;
})(window)
```

### 2. 默认参数
给插件配置默认的参数，当插件调用的时候没有传入参数，则使用默认的值
```javascript
;(function(win){
    function Fn(params){
        var defaults = {
            width: 100,
            color: "#000"
        };
        var params = params || {}; 
        for (var x in defaults) {
            if (typeof params[x] === 'undefined') {
                //对于使用时，没有设置的参数；用默认参数代替
                params[x] = defaults[x];
            }  
        }
        this.params= params;//得到的this.params,在方法中调用；
    }
    Fn.prototype = {
        constructor: Fn
    }
    win.Fn = Fn;
})(window)
var str = {
    width: 200,
    height: 100
};
new Fn(str);
```

### 3. 方法到底写到this里还是prototype里
从内存说起:   
写到原型上，每执行一个实例，getC不需要开辟新的内存.故，可以把一些`纯计算`的方法，写原型上;   
如果`方法和实例本身有关`，应该写道this中  

```javascript
//①
function Fn(){
    this.getC = function(){
        //...
    }
}
//or...
//②
function Fn(){}
Fn.prototype.getC = function(){}
```

### 4. 方法名防止冲突处理
如果在引入你的插件之前，window下已经有Fn的变量；此时应该把Fn的控制权交出，自己用Fn2输出
```javascript
;(function(win){
    function Fn(){}
    
    if(win.Fn){
        win.Fn2 = Fn ;
    }
    Fn.prototype = {
        constructor: Fn
    }
    win.Fn = Fn;
})(window)
new Fn();
```

### 5. 对外输出的规范化export 、AMD 完整写法
给插件配置默认的参数，当插件调用的时候没有传入参数，则使用默认的值
```javascript
;(function(global) {
 
    //开启严格模式，规范代码，提高浏览器运行效率
    "use strict";
 
    //定义一个类，通常首字母大写
    var MyPlugin = function(options) {
        this.name  = name;
        this.init();
    };
 
    //覆写原型链，给继承者提供方法
    MyPlugin.prototype = {
        constructor:MyPlugin,
        init: function() {
            
        }
    };
 
    //兼容CommonJs规范
    if (typeof module !== 'undefined' && module.exports) module.exports = MyPlugin;
 
    //兼容AMD/CMD规范
    if (typeof define === 'function') define(function() { return MyPlugin; });
 
    //注册全局变量，兼容直接使用script标签引入该插件
    global.MyPlugin = MyPlugin;
 
//this，在浏览器环境指window，在nodejs环境指global
//使用this而不直接用window/global是为了兼容浏览器端和服务端
//将this传进函数体，使全局变量变为局部变量，可缩短函数访问全局变量的时间
})(this);

new MyPlugin();
```