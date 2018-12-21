---
layout:     post
title:      "clientHeight offsetHeight scrollHeight区别"
subtitle:   ""
date:       2016-04-19 17:00:00
author:     "Jxx"
header-img: "img/post-bg-180006.jpg"
tags:
    - javascript
---
 
### 普通div
**区别图如下**
![结果](https://onepiece1991.github.io/img/in-post/post-js-version/CSO-180001.png)

### documentElement 和 body 相关说明： 

> * body是DOM对象里的body子节点，即 <body> 标签；  
> * documentElement 是整个节点树的根节点root，即<html> 标签；

**区别图如下**
![结果](https://onepiece1991.github.io/img/in-post/post-js-version/CSO-180002.png)
`document.documentElement.clientHeight显示的是浏览器可视区域的高度`
