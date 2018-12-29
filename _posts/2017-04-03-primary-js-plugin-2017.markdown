---
layout:     post
title:      "js写插件教程---入门"
subtitle:   "在输入框中输入色值来改变body的颜色并添加修改记录"
date:       2017-04-03 13:00:00
author:     "Jxx"
header-img: "img/post-bg-180006.jpg"
tags:
    - javascript
---
 
页面html
```html
<div id="test1">
    <input  type="" name="" id="" value="red" />    
    <button id="btn1">Apply</button> 
</div>
```

## 前言
- 整个插件写在一个立即执行函数里，即 (function(){})()， 函数自执行，保证里面的变量不会与外界互相影响；
- function前的 ;(分号) 是为了防止别人的js结束没有分号，而可能发生语法上的混淆报错;
- 头部的win,doc和$是底部的window,document,jQuery的映射，方便内部直接调用；（如果不引用jq的话头部的$和底部的jQuery可以不要；如果还需要别的依赖可以在后面添加）
- 最后面的undefined可写可不写，最好写上保证里面再出现的undefined是未定义的意思，不被其他东西赋值。

框架如下：
```javascript
;(function(win,doc,$,undefined){
 
}(window,document,jQuery))
```

插件完整代码
```javascript
    ;(function(win,doc,undefined){
        //AddBox是插件名，构造函数，一般首字母大写
        //调用的时候直接new一下插件名并传参数或者对象就行了
        var AddBox = function(demo,btn) {
            //this指向这个函数（实例）
            this.div = doc.getElementById(demo);
            this.btn = doc.getElementById(btn);
            this.Input = this.div.getElementsByTagName('input')[0];
            //可以写一写默认的设置，方便下面调用
            //这里写了什么都不会报错；只是有用没用的问题
            this.author = 'Jinxq';
            //执行下面写的函数，调用最开始执行的函数
            this.init();
        };
        //给构造函数AddBox原型里添加属性（方法）
        AddBox.prototype = {
            //构造器指向构造函数，防止构造器指向Object
            constructor: AddBox,
            init: function() {
                //把this保存下来防止在局部函数内部取不到（局部函数内部取得this可能是别的值） 
                var _self = this;
                this.btn.onclick = function(){
                    //取得实时时间
                    //扩展的方法之间都可以用this.方法名 互相调用
                    var timeStamp = _self.getTimeStamp();
                    var _val = _self.Input.value;
                    //创建临时div存放修改记录
                    var tempdiv = doc.createElement('div');
                    tempdiv.innerHTML = timeStamp + ' 修改成 ' + _val;
                    tempdiv.style.color = '#000';
                    _self.div.appendChild(tempdiv);
                    _self.setColor();
                }
            },
            //根据input框里的色值来色值body的颜色
            setColor: function() {
                doc.body.style.backgroundColor = this.Input.value;
            },
            //获取时间戳
            getTimeStamp: function() {
                var myDate = new Date();
                var year = myDate.getFullYear();
                var month = myDate.getMonth() + 1;
                var dateT = myDate.getDate();
                var day = myDate.getDay();
                var hour = myDate.getHours();
                var minute = myDate.getMinutes();
                var second = myDate.getSeconds();
                switch(day) {
                    case 0:
                        day = '日';
                        break;
                    case 1:
                        day = '一';
                        break;
                    case 2:
                        day = '二';
                        break;
                    case 3:
                        day = '三';
                        break;
                    case 4:
                        day = '四';
                        break;
                    case 5:
                        day = '五';
                        break;
                    case 6:
                        day = '六';
                        break;
                };
                if(hour < 10) {
                    hour = '0' + hour;
                }
                if(minute < 10) {
                    minute = '0' + minute;
                }
                if(second < 10) {
                    second = '0' + second;
                }
                return year + '-' + month + '-' + dateT + ' 星期' + day + ' ' + hour + ':' + minute + ':' + second;
            }
        };
        //把这个对象附给window底下的 名字叫AddBox的对象；要不你调用的时候new AddBox() 怕在window的环境下找不到；
        win.AddBox = AddBox;
    })(window,document)
    //调用插件
    new AddBox('test1','btn1');
```
