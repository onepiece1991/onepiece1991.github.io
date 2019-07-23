---
layout:     post
title:      "截取字符串"
subtitle:   "substring() substr() slice() splice() indexOf()"
date:       2018-10-29 20:25:00
author:     "Jxx"
header-img: "img/post-bg-180006.jpg"
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

## splice()
[splice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)  
方法通过删除现有元素和/或添加新元素来修改数组,并以数组返回原数组中被修改的内容。  

##### 语法:

```javascript
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```

##### 例子:

```javascript
var months = ['Jan', 'March', 'April', 'June'];  
months.splice(1, 0, 'Feb');  
// inserts at 1st index position  
console.log(months);  
// expected output: Array ['Jan', 'Feb', 'March', 'April', 'June']  
  
months.splice(4, 1, 'May');  
// replaces 1 element at 4th index    
console.log(months);  
// expected output: Array ['Jan', 'Feb', 'March', 'April', 'May']  
```

* 请注意，splice() 方法与 slice() 方法的作用是不同的，splice() 方法会直接对数组进行修改。 *

## slice()
[slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
方法返回一个新的数组对象，这一对象是一个由 begin和 end（不包括end）决定的原数组的浅拷贝。原始数组不会被改变。

##### 语法:

```javascript
arr.slice();  
// [0, end]

arr.slice(begin);  
// [begin, end]

arr.slice(begin, end);  
// [begin, end)
```

##### 例子:

```javascript
var animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];  

console.log(animals.slice(2));  
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));  
// expected output: Array ["camel", "duck"]

console.log(animals.slice(1, 5));  
// expected output: Array ["bison", "camel", "duck", "elephant"]
```

## indexOf()

方法可返回某个指定的字符串值在字符串中首次出现的位置。

##### 语法

```javascript
stringObject.indexOf(searchvalue,fromindex)
//如果要检索的字符串值没有出现，则该方法返回 -1。
```
|参数		|描述		|
|--	|--	|
|searchvalue	|必需。规定需检索的字符串值。	|
|fromindex	|可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是 0 到 stringObject.length - 1。如省略该参数，则将从字符串的首字符开始检索。|

## 总结
> substring(start, stop);   ----不包括stop  
> substr(start, length);   
> slice(start, end) ;  -----不包括end ----用于数组，不会修改原数组，返回一个新数组  
> splice(start, deleteCount, item);  -----deleteCount=0,不删除直接在start位置添加item,否则删除deleteCount位置的数据并在该位置添加item