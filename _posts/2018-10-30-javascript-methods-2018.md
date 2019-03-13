---
layout:     post
title:      "javascript methods"
subtitle:   "Record some javascript methods"
date:       2018-10-29 20:25:00
author:     "Jxx"
header-img: "img/post-bg-180006.jpg"
tags:
    - javascript
---

### 通过ajax获取页面中指定容器内的代码块
```javascript
//方法一
success: function(result) {
            var Obj = $("<code></code>").append(result);
            var $html = $(".page-wrapper", Obj);
            var value = $html.html();
            $("#mainPage").html(value);
        }
        
//方法二
success: function(result) {
            var reg = /[\s\S]*<\/body>/g;
            var html = reg.exec(result)[0];
            //然后用filter来筛选对应块对象，如：class='page-wrapper'
            var page = $(html).filter(".page-wrapper");
            var value = page.html();
            $("#mainPage").html(value);
        }
```




### 截取页面中载入的js文件
- result 是通过ajax获取的html页面
- trimSpace()和getMultiScripts()函数用法如下所述

```javascript
// 第一步：匹配加载的页面中是否含有js
var regDetectJs = /<script(.|\n)*?>(.|\n|\r\n)*?<\/script>/ig;
var jsContained = result.match(regDetectJs);

// 第二步：如果包含js，则一段一段的取出js再加载执行
if (jsContained) {
    // 分段取出js正则(这一步感觉没什么用，删了)
    // var regGetJS = /<script(.|\n)*?>((.|\n|\r\n)*)?<\/script>/im;

    // 按顺序分段执行js
    var jsNums = jsContained.length;
    var sname = [];
    for (var i = 0; i < jsNums; i++) {
        //(同上，删了)
        // var jsSection = jsContained[i].match(regGetJS);
        // var script = jsSection[0];
        var script = jsContained[i];
        var arr = script.split("../../");
        if (arr.length != 1) {
            var sarr = arr[1].split('"></script>');
            sname[i] = sarr[0];
        }
    }
    trimSpace(sname);
    getMultiScripts(sname,'./');
}
```

### 如果array中的元素存在空字符串''、undefined、null和empty,则去掉该元素
```javascript
function trimSpace(array) {
    for (var i = 0; i < array.length; i++) {
        if (array[i] == " " || array[i] == null || typeof(array[i]) == "undefined") {
            array.splice(i, 1);
            i = i - 1;
        }
    }
    return array;
}
```

### jQuery载入多个js文件
 - @param arr js路径集合{Array}
 - @param path js父级路径{String}

```javascript
function getMultiScripts(arr, path) {
    var _arr = $.map(arr, function(scr) {
        return $.getScript((path || "") + scr);
    });

    _arr.push($.Deferred(function(deferred) {
        $(deferred.resolve);
    }));

    return $.when.apply($, _arr);
}
```

### 每隔两行tr添加背景颜色
```javascript
var trAddBg = function (){
    var trA =  $('tbody').children("tr");
    trA.each(function(i){
        if((i % 4 == 2) || (i % 4 == 3)) {
            $(trA[i]).addClass("bg");
        }
    })
}
```

### 逐字显示效果
```javascript
<p id="aa"></p>
<div style="display:none" id="w">今天的雨跟依萍找他爸要钱的那天一样大！</div>
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

### 获取键码
```javascript
$("body").keyup(function(event) {
    //处理键盘事件
    var myEvent = event || window.event;
    var keyCode = myEvent.which || myEvent.keyCode || myEvent.charCode;
    console.log(keyCode);
})
```

### XMLHttpRequest 对象的三个重要的属性
![XMLHttpRequest 对象的三个重要的属性](https://onepiece1991.github.io/img/in-post/post-js-version/xmlHttpRequest.png)
当 readyState 等于 4 且状态status为 200 时，表示响应已就绪
```javascript
var test;  
if(window.XMLHttpRequest){  
    test = new XMLHttpRequest();  
}else if(window.ActiveXObject){  
    test = new window.ActiveXObject();  
}else{  
    alert("请升级至最新版本的浏览器");  
}  
if(test !=null){  
    test.open("GET","week.json",true);  
    test.send(null);  
    test.onreadystatechange=function(){  
        if(test.readyState==4&&test.status==200){  
            console.log("页面已就绪");
            var obj = JSON.parse(test.responseText);  
            console.log("json数据存放在responseText里")  
        }  
    };  
  
}
```

