---
layout:     post
title:      "截取字符串"
subtitle:   "substring() substr()"
date:       2018-10-29 20:25:00
author:     "Jxx"
header-img: "img/post-bg-2015.jpg"
tags:
    - javascript
---


## substring()  
[substring()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substring)
方法返回一个字符串在开始索引到结束索引之间的一个子集, 或从开始索引直到字符串的末尾的一个子集。  

##### 语法:

```javascript
str.substring(indexStart[, indexEnd])  
```
##### 例子:

```javascript
var anyString = 'Mozilla';
var anyString4 = anyString.substring(anyString.length - 4);
console.log(anyString4); //illa  

//逐字输入效果
<p id="aa"></p>
<div style="display:none" id="w">今天的雨跟依萍找他爸要钱的那天一样大！</div>
<script src="jquery.min.js"></script>
<script type="text/javascript">
window.onload = type;
var index = 0;
var word = $("#w").html();
function type(){
    $("#aa").html(word.substring(0,index++));
    if(index > word.length) {
        return;
    } else {
        setTimeout(type,50);
    };
}
</script>
```


## substr()  
[substr()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr)
方法返回一个字符串中从指定位置开始到指定字符数的字符。

##### 语法:

```javascript
str.substr(start[, length]) 
```
##### 例子:

```javascript
var str = "abcdefghij";
console.log(str.substr(1,2));   // (1,2): bc
console.log(str.substr(-3,2));  // (-3,2): hi
console.log(str.substr(-3));    // (-3): hij
console.log(str.substr(1));     // (1): bcdefghij
console.log(str.substr(-20,2)); // (-20, 2): ab
console.log(str.substr(20,2));  // (20, 2):

```