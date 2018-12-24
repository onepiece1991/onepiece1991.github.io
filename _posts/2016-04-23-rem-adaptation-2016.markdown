---
layout:     post
title:      "rem灵活移动端适配"
subtitle:   "用js或者vm来适配"
date:       2016-04-23 10:30:00
author:     "Jxx"
header-img: "img/post-bg-180004.jpg"
tags:
    - CSS3
---


### 什么是rem?
rem（font size of the root element）是指相对于根元素的字体大小的单位。简单的说它就是一个相对单位。

如下代码，相信很多做移动端的都见过，这段js代码可以有效适配任何移动端设备没有问题。缺点是页面需要加载js代码
```javascript
!function(win){
    var e=win.document,
        t=e.documentElement,
        i=375,
        d=i/100,
        o="orientationchange" in win ? "orientationchange" : "resize",
        a=function(){
            var n=t.clientWidth||320;
            t.style.fontSize=n/d+"px";
        };
    e.addEventListener&&(win.addEventListener(o,a,false),e.addEventListener("DOMContentLoaded",a,false))
}(window); 
```

若只通过css来适配则需要使用CSS3中的一个单位**vm**:   
**vw： 1vw 等于视口宽度的1%**

以750*1334 iphone6的设计稿为例：
1VW = 750px * 1％ = 7.5px
添加如下代码
```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```

为了方便计算，我们用 100px为例 100/7.5px = 13.333333333333334。给重置common.css设置
```css
html {
    font-size: 13.333333333333334vw;
}
```
接下来我们使用单位就直接可以按照100px = 1rem来做页面了。

**总结： rem是根据html标签来计算的， 再者vw将视口分成100份，每一份代表当前1％。计算公式：100/(deviceWidth*1%)，单位是vm。**
