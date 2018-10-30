---
layout:     post
title:      "javascript methods"
subtitle:   "substring() substr()"
date:       2018-10-29 20:25:00
author:     "Jxx"
header-img: "img/post-bg-2015.jpg"
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