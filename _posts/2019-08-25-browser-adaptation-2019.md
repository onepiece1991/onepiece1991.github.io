---
layout:     post
title:      "浏览器适配总结"
subtitle:   ""
date:       2019-08-25 14:30:00
author:     "Jxx"
header-img: "img/post-bg-180004.jpg"
tags:
    - CSS
---

### 前言
由于IE8还有很大一部分用户，而IE8对HTML5和CSS3的支持并不理想。因此想让网站浏览者正常浏览网页，一般会有两种处理方法：  
1. 通过navigator.userAgent判断浏览器版本从而显示不同的网页
2. 使用Javascript来使不支持HTML5的浏览器支持HTML标签。

下面就来总结一下：

## 一、 根据浏览器版本显示不同网页
```javascript
var DEFAULT_VERSION = "9.0";
var ua = navigator.userAgent.toLowerCase();
var isIE = ua.indexOf("msie") > -1;
var safariVersion;
if (isIE) {
  safariVersion = ua.match(/msie ([\d.]+)/)[1];   //获取浏览器版本号
}
if (safariVersion*1 < DEFAULT_VERSION*1) {   //若版本号低于IE9，则跳转到如下页面
  window.location.href = "https://www.baidu.com/";    //提示页面（修改路径）
}
```

## 二、 html5shiv.js
**html5shiv 是一个针对 IE 浏览器的 HTML5 JavaScript 补丁，目的是让 IE9以下浏览器 识别并支持 HTML5 元素。**

使用方法：

```html
<!--[if lt IE 9]>
<script src="https://cdn.bootcss.com/html5shiv/3.7.3/html5shiv.js"></script>
<![endif]-->
```
### 2.1 关键词
> `lte`：就是Less than or equalto的简写，也就是小于或等于的意思。  
> `lt` ：就是Less than的简写，也就是小于的意思。  
> `gte`：就是Greater than orequal to的简写，也就是大于或等于的意思。  
> `gt` ：就是Greater than的简写，也就是大于的意思。  
> `!` ：就是不等于的意思，跟javascript里的不等于判断符相同

写法：
```html
<!--[if IE]>此处内容只有IE可见<![endif]-->

<!--[if IE 6]>此处内容只有IE6.0可见<![endif]-->

<!--[if IE 7]>此处内容只有IE7.0可见<![endif]-->

<!--[if lt IE 6]> IE6以及IE6以下版本可识别<![endif]-->

<!--[if gte IE 6]> IE6以及IE6以上版本可识别<![endif]-->

<!--[if lt IE 7]> IE7以及IE7以下版本可识别<![endif]-->

<!--[if gte IE 7]> IE7以及IE7以上版本可识别<![endif]-->

<!--[if !IE]>此处内容只非IE可见<![endif]-->
```


[html5shiv.js下载地址](https://www.bootcdn.cn/html5shiv/)

## 三、 respond.js

Respond.js 是一个快速、轻量的 polyfill，用于为 IE6-8 以及其它不支持 CSS3 Media Queries 的浏览器提供媒体查询的 min-width 和 max-width 特性，实现响应式网页设计（Responsive Web Design）。

**注意事项：**

1. 由于浏览器的安全规则问题，Respond.js 不能通过 file:// 协议（打开本地HTML文件所用的协议）访问的页面上发挥正常的功能。如果需要测试IE8下面的Respond.js 响应式特性，`必须用http服务器（例如apache、nginx）托管HTML页面才可以`。
2. `Respond.js 不支持通过 @import 引入的CSS文件`。例如，Drupal 一般被配置为通过 @import 引入CSS文件，Respond.js对其将无法起到作用。 
3. IE8不能完全支持box-sizing: border-box;与min-width、max-width、min-height或max-height一同使用。由于此原因，`从Bootstrap v3.0.1版本开始，我们不再为.container使用max-width`。
4. 另一种让ie8及以下支持css3媒体查询的css3-mediaqueries.js 像上面引入也可，但是会出现闪屏 不是特别推荐使用， 上面是最常用的

[respond.js下载地址](https://www.bootcdn.cn/respond.js/)

## 四、 background-size polyfill(backgroundsize.min.htc)
`使用该文件能够让 IE7、IE8 支持 background-size 属性`。其原理是创建一个 img 元素插入到容器中，并重新计算宽度、高度、left、top 等值，模拟 background-size 的效果。

使用方法：
``` css
.selector { 
  background-size: cover; 
  -ms-behavior: url(/backgroundsize.min.htc); 
  behavior: url(/backgroundsize.min.htc); 
}
```

也可以通过`滤镜filter`实现，如下:
```css
body {
  background: url() no-repeat center;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  filter:progid:DXImageTransform.Microsoft.AlphaImageLoader( src='', sizingMethod='scale');
  -ms-filter:progid:DXImageTransform.Microsoft.AlphaImageLoader( src='', sizingMethod='scale');
}
```
**但是使用滤镜filter实现IE8下的background-size 属性效果时 ，局限性如下：**

1. 引用图片不能透明的，否则会出现双层图片的效果；
2. 不能指定任意大小background百分比，可用cover；
3. 用于单张图片不能使用图片精灵等拼图；
4. 要引用绝对路径图片；

[backgroundsize.min.htc下载地址](https://github.com/louisremi/background-size-polyfill)

## 五、 ie-css3.htc
`是一个可以让IE低版本（如IE6/7/8）浏览器支持部份CSS3属性的htc文件`，比如`盒阴影box-shadow`、`圆角属性border-radius`、`文字阴影属性text-shadow` 。

详细实现原理参见[这里](https://www.zhangxinxu.com/wordpress/2010/04/%E8%AE%A9ie6ie7ie8%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81css3%E5%B1%9E%E6%80%A7/)

使用方法：
```css
.selector {
  -ms-behavior: url(/ie-css3.htc);
  behavior: url(/ie-css3.htc);
}
```
 
**注意事项：**

1. 当前元素一定要有定位属性，position:relative或position:absolute。 z-index值一定要比周围元素的要高；
2. http协议访问；
3. 原生IE浏览器访问，兼容模式显示可能没效果。

**支持样式：**

1. border-radius: 只设置一个角落的圆角属性时无效；
2. box-shadow: 只支持#(000）黑色阴影；
3. text-shadow: IE下样式表现与Firefox/Safari/Chrome存在差异。

[ie-css3.htc下载](https://www.zhangxinxu.com/study/down/ie-css3.htc)

## 六、 pie.htc
用CSS的behavior行为，可以调用此文件，然后让IE6\7\8也能实现一些常见的CSS3效果，如`圆角(border-radius)`，`盒阴影(box-shadow)`，`背景渐变(gradient)`，`多图片背景(multiple background images)`、`透明度(rgba)`。

使用如下实现线性渐变：
```css
.demo{
    height: 100px ;
    width:200px;
    background:linear-gradient(90deg,#00FFFF,#0000FF);
    -pie-background:linear-gradient(0,#00FFFF,#0000FF);
    behavior: url(static_resources/PIE.htc);
}
```
**注意事项：**
1. 当前元素一定要有定位属性，position:relative或position:absolute。 z-index值一定要比周围元素的要高(不可为-1)；
2. border-image(IE6/7/8只会以stretch的形式进行填充，即使border-image-repeat属性是repeat和round)；
3. 使用绝对路径；
4. PIE不支持border-top-left-radius表示左上圆角的缩写方式；
5. 在服务器上提供 htc文件的MIME 配置类型

[pie.htc下载](http://css3pie.com)

## 七、 cssSandpaper库
`cssSandpaper是一个CSS3的JavaScript库`，使用特定的CSS书写规则可以让页面元素支持CSS3的一些特性，例如：`旋转`，`拉伸`，`盒阴影`，`渐变等效果`，`包括IE浏览器`。

详见[这里](https://www.zhangxinxu.com/wordpress/2010/05/csssandpaper-%E5%85%BC%E5%AE%B9ie%E7%9A%84css3-javascript%E5%BA%93/)

## 八、Normalize.css
不同浏览器的默认样式存在差异，可以使用 Normalize.css 抹平这些差异。当然，也可以使用 reset.css

使用normalize.css有下几个优点：

1. 保护有用的浏览器默认样式而不是完全去掉它们
2. 一般化的样式：为大部分HTML元素提供
3. 修复浏览器自身的bug并保证各浏览器的一致性
4. 优化CSS可用性：用一些小技巧
5. 解释代码：用注释和详细的文档来

[normalize.css下载](http://necolas.github.io/normalize.css/)

参考资料：

[CSS3新特性，兼容性，兼容方法总结](https://www.cnblogs.com/jesse131/p/5441199.html)  
[让IE6\7\8支持Html5和CSS3的各类JS和Htc归纳](https://blog.csdn.net/zhouzuoluo/article/details/93720071)