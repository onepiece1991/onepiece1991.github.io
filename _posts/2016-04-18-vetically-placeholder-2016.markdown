---
layout:     post
title:      "Placeholder垂直居中"
subtitle:   ""
date:       2016-04-18 10:00:00
author:     "Jxx"
header-img: "img/post-bg-180004.jpg"
tags:
    - CSS3
---


## PC端-（Chrome浏览器）
### 设置input的font-size值大于placeholder的font-size值
```css
input.test {  
    display: block;  
    padding: 6px 0;  
    width: 100%;  
    height: 44px;  
    font-size: 32px;  
    color: #333;  
    font-weight: bold;  
    text-align: left;  
    line-height: normal;  
}  
input.test::-webkit-input-placeholder {  
    font-size: 14px;   
    color: #999;  
    font-weight: normal;
}  
```

```html
<input type="text" placeholder="测试使用" class="test" />
```

**效果图如下**
![placeholder值显示会在光标偏下位置](https://onepiece1991.github.io/img/in-post/post-css/pl-180001.png)

如果需要placeholder垂直居中，则需要设置`::-webkit-input-placeholder` 的transform

```css
::-webkit-input-placeholder {
    font-size: 14px;
    color: #999;
    font-weight: normal;
    transform: translate(0, -6px);
}
```

**效果图如下**
![placeholder垂直居中显示](https://onepiece1991.github.io/img/in-post/post-css/pl-180002.png)

**输入后的效果**
![placeholder垂直居中显示](https://onepiece1991.github.io/img/in-post/post-css/pl-180003.png)

## APP
```css
input.test {  
    display: block;  
    padding: 6px 0;  
    width: 100%;  
    height: 44px;  
    font-size: 32px;  
    color: #333;  
    font-weight: bold;  
    text-align: left;  
    line-height: normal;  
}  
input.test::-webkit-input-placeholder {  
    font-size: 14px;   
    color: #999;  
    font-weight: normal;
}  
```

```html
<input type="text" placeholder="测试使用" class="test" />
```

测试结果：   
Android:  placeholder会在光标偏下位置   **效果如下图**
![placeholder垂直居中显示](https://onepiece1991.github.io/img/in-post/post-css/pl-180004.png)


iphone:  placeholder会在光标偏上位置   **效果如下图**
![placeholder垂直居中显示](https://onepiece1991.github.io/img/in-post/post-css/pl-180005.png)

因此需要判断机型
```javascript
$(function(){
    var userSys = navigator.userAgent;
    if(userSys.indexOf("Android") > -1) {
        $('.test').removeClass('iphone')
        $('.test').addClass('android')
    } else {
        $('.test').removeClass('android')
        $('.test').addClass('iphone')
    }
})
```

```css
input.test.android::-webkit-input-placeholder {
    transform: translate(0, -.325rem);
}
input.test.iphone::-webkit-input-placeholder {
    transform: translate(0, .45rem);
}
```

处理后结果如下
**Android**
![placeholder垂直居中显示](https://onepiece1991.github.io/img/in-post/post-css/pl-180006.png)

**Iphone**
![placeholder垂直居中显示](https://onepiece1991.github.io/img/in-post/post-css/pl-180007.png)