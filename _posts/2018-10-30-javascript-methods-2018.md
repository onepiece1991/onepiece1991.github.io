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

