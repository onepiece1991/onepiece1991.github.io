---
layout:     post
title:      "javascript methods"
subtitle:   "link rel=alternate网站换肤功能最佳实现"
date:       2019-03-14 18:41:00
author:     "Jxx"
header-img: "img/post-bg-180006.jpg"
tags:
    - javascript
---

# 一、大多数开发人员的实现
大多数前端开发人员实现网站皮肤更换功能大致下面两种方法：

1. 一个全局class控制样式切换；
2. 改变皮肤link元素的href地址。例如：

```html
<link id="skinLink" href="skin-default.css" rel="stylesheet" type="text/css">
```

换皮肤的时候JS改变href属性值：

```javascript
skinLink.href = 'skin-red.css';
```

例如我10年前模拟腾讯网首页的换肤功能做的[demo](https://www.zhangxinxu.com/study/200912/qq-home-page-skin-jquery.html)页面就是这么实现的。

都不完美。全局class控制样式提高了样式优先级，如果换肤样式很多，代码会非常啰嗦，不利于维护；使用JS改变href属性会带来加载延迟，样式切换不流畅，体验不佳。

实际上，浏览器有原生特性，非常适合实现网站换肤功能。

# 二、原生HTML特性下的网站换肤
此方法借助HTML rel属性的`alternate`属性值实现。示意HTML如下：

```html
<link href="reset.css" rel="stylesheet" type="text/css">
                
<link href="default.css" rel="stylesheet" type="text/css" title="默认">
<link href="red.css" rel="alternate stylesheet" type="text/css" title="红色">
<link href="green.css" rel="alternate stylesheet" type="text/css" title="绿色">
```

上面4个`<link>`元素，共出现了3中不同性质的CSS样式文件加载：

- 没有title属性，rel属性值仅仅是stylesheet的<link>无论如何都会加载并渲染，如reset.css；
- 有title属性，rel属性值仅仅是stylesheet的<link>作为默认样式CSS文件加载并渲染，如default.css；
- 有title属性，rel属性值同时包含alternate stylesheet的<link>作为备选样式CSS文件加载，默认不渲染，如red.css和green.css；

这里有个非常有趣的特性，那就是`rel="stylesheet"`的<link>如果有title属性并有值，性质上就变成了一个可以控制其渲染或者不渲染的特殊元素了。

## 如何控制呢？

一种说是浏览器自己会有样式切换菜单，查看→页面，选择title属性值对应的样式。我猜这是10年前的说法了吧，现在浏览器根本没有这些菜单选项，至少我找了好久，各个浏览器都找了个遍没找到。

另外一种就是使用JS进行控制了，使用JavaScript代码修改`<link>`元素`DOM`对象的`disabled`值为`false`，可以让默认不渲染的CSS开始渲染。**注意，必须是DOM元素对象的disabled属性，而不是HTML元素的disabled属性**，`<link>`元素是没有`disabled`属性的。

例如：

```javascript
// 渲染red.css这个皮肤
document.querySelector('link[href="red.css"]').disabled = false;
```

因此，要实现换肤功能，只要在页面上方几个换肤按钮，点击的时候改变对应`<link>`元素`DOM`对象的`disabled`值就可以了。

眼见为实，我专门做了demo页面，您可以狠狠地点击这里： [link rel alternate实现网站换肤功能demo](https://www.zhangxinxu.com/study/201902/rel-alternate-switch-skin-demo.php)

结果如下GIF截图所示（截自IE浏览器）：

![link rel alternate](https://onepiece1991.github.io/img/in-post/post-js-version/alternate-skin.png)

demo页面是使用单选框模拟实现的，HTML和JS代码如下：

```html
<input id="default" type="radio" name="skin" value="default.css" checked>
<input id="red" type="radio" name="skin" value="red.css">
<input id="green" type="radio" name="skin" value="green.css">
```

```javascript
var eleLinks = document.querySelectorAll('link[title]');
var eleRadios = document.querySelectorAll('[type="radio"]');
[].slice.call(eleRadios).forEach(function (radio) {
    radio.addEventListener('click', function () {
        var value = this.value;
        [].slice.call(eleLinks).forEach(function (link) {
            link.disabled = true;
            if (link.getAttribute('href') == value) {
                // 该样式CSS文件生效
                link.disabled = false;
            }
        });
    });
});
```

# 三、link rel=alternate方法实现优点

3大优点：

1. 兼容性非常好。IE9+（IE8我没测，理论也支持），Chrome和Firefox均支持这种更原生的换肤效果实现。
2. 语义非常好。用户，开发者，尤其搜索引擎或者其他辅助阅读设备能够准确识别网站还有其他替换CSS样式。（alternate的语义就是可替换的）
3. 交互体验更好。rel=alternate方法实现的换肤功能在网站样式变换的时候是瞬间切换，完全无感知。因为浏览器已经把换肤的CSS文件预加载好了，比JS改变href地址的体验要更好。配合http2.0，几乎可以说是完美无瑕的解决方案了。

> 本文转自[link rel=alternate网站换肤功能最佳实现](https://www.zhangxinxu.com/wordpress/2019/02/link-rel-alternate-website-skin/)