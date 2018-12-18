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