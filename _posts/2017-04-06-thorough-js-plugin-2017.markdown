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