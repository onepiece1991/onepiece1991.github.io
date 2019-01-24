---
layout:     post
title:      "理解SVG的viewport,viewBox,preserveAspectRatio"
subtitle:   ""
date:       2016-05-12 10:04:00
author:     "Jxx"
header-img: "img/post-bg-180008.jpg"
tags:
    - SVG
---

## viewport
表示SVG可见区域的大小，或者可以想象成舞台大小，画布大小。
```XML
<svg width="500" height="300"></svg>
```
上面的SVG代码定义了一个视区，宽500单位，高300单位。

注意这里的措辞是“单位”，不是“像素”。虽然说，`width`/`height`如果是纯数字，使用的就是“像素”作为单位的。

也就是说，上面SVG的视区大小就是`500px * 300px`.

当然，故弄“单位”这个措辞，潜台词就是你可以使用其他类型的单位，涵盖常见CSS单位：

|单位	|含义	|
|--	|--	|
|em	|相对于父元素的字体大小	|
|ex	|相对于小写字母"x"的高度	|
|px	|相对于屏幕分辨率而不是视窗大小：通常为1个点或1/72英寸	|
|in	|inch, 表英寸	|
|cm	|centimeter, 表厘米	|
|mm	|millimeter, 表毫米	|
|pt	|1/72英寸	|
|pc	|12点活字，或1/12点	|
|%	|相对于父元素。正常情况下是通过属性定义自身或其他元素	|

除了SVG本身，其他一些元素，例如`<rect>`的`width`/`height`属性也可以使用上面的这些单位，也是默认单位是像素。

## viewBox属性
这个是本文的重点，也是难点。

先看一个活蹦乱跳的例子，如下HTML代码：
```XML
<svg width="400" height="300" viewBox="0,0,40,30" style="border:1px solid #cd0000;">
    <rect x="10" y="5" width="20" height="15" fill="#cd0000"/>
</svg>
```
结果如下：

![效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180015.png)

或者亲自围观[demo](https://www.zhangxinxu.com/study/201408/svg-viewbox.html)

如果不看`viewBox`, 你一定会觉得诧异——SVG尺寸明明有`400*300`像素，而小小的`<rect>`大小只有其`1/20`，但是显示出来的却占据了半壁江山！不科学啊！

OK, 之所以小小矩形大显神威就是这里的`viewBox`起了推波助澜的作用。

`viewBox`值有`4`个数字：

> viewBox="x, y, width, height"  // x:左上角横坐标，y:左上角纵坐标，width:宽度，height:高度

`viewBox`顾名思意是“视区盒子”的意思，好比在说：“SVG啊，要不你就让我铺满你吧~”

更形象的解释就是：SVG就像是我们的显示器屏幕，`viewBox`就是截屏工具选中的那个框框，最终的呈现就是把框框中的截屏内容再次在显示器中全屏显示！

更直观的解释：
1. 如果没有viewBox, 应该是长这样的：   
![没有viewBox](https://onepiece1991.github.io/img/in-post/post-svg/svg-180016.png)

`<rect>`大小只有整个SVG舞台的`1/20`.

2. `viewBox="0,0,40,30"`相当于在SVG上圈了下图左上角所示的一个框框：   
![有viewBox](https://onepiece1991.github.io/img/in-post/post-svg/svg-180017.png)

3. 然后把这个框框，连同框框里的小矩形一起放大到整个SVG大小（如下gif）:
![gif演示区域面积扩大](https://onepiece1991.github.io/img/in-post/post-svg/svg-180018.gif)

到手里的才是自己的，您可以狠狠地点击这里：[SVG viewBox属性原理分步演示demo](https://www.zhangxinxu.com/study/201408/svg-viewbox-explain.html)

![demo操作截图示意](https://onepiece1991.github.io/img/in-post/post-svg/svg-180019.png)

## preserveAspectRatio
上面的例子，SVG的宽高比正好和`viewBox`的宽高比是一样的，都是`4:3`. 显然，实际应用`viewBox`不可能一直跟`viewport`穿同一条开裆裤。此时，就需要`preserveAspectRatio`出马了，此属性也是应用在`<svg>`元素上，且作用的对象都是`viewBox`。

先看下猪是怎么跑的：  
> preserveAspectRatio="xMidYMid meet"

下面我们来吃猪肉。

`preserveAspectRatio`属性的值为空格分隔的两个值组合而成。例如，上面的`xMidYMid`和`meet`.

- 第1个值表示，viewBox如何与SVG viewport对齐
- 第2个值表示，如何维持高宽比（如果有）。

其中，第1个值又是由两部分组成的。前半部分表示`x`方向对齐，后半部分表示`y`方向对齐。家族成员如下：

|值	|含义	|
|--	|--	|
|xMin	|viewport和viewBox左边对齐	|
|xMid	|viewport和viewBox x轴中心对齐	|
|xMax	|viewport和viewBox右边对齐	|
|YMin	|viewport和viewBox上边缘对齐。注意Y是大写。	|
|YMid	|viewport和viewBox y轴中心点对齐。注意Y是大写。	|
|YMax	|viewport和viewBox下边缘对齐。注意Y是大写。	|

x, y自由合体就可以了，如：

> xMaxYMax
> xMidYMid

亲爱的小伙伴，看出啥意思没？

噔噔蹬蹬，没错，就是组合的意思：“右-下”和“中-中”对齐。恭喜你此处的知识点学习顺利毕业！

`preserveAspectRatio`属性第2部分的值支持下面3个：

|值	|含义	|
|--	|--	|
|meet	|保持纵横比缩放viewBox适应viewport，受	|
|slice	|保持纵横比同时比例小的方向放大填满viewport，攻	|
|none	|扭曲纵横比以充分适应viewport，变态	|

现在急需一个活生生的例子，让大家感受下这三个值的表现。

您可以狠狠地点击这里: [meet,slice,none功能演示demo](https://www.zhangxinxu.com/study/201408/svg-preserveaspectratio-meet-slice-none.html)

首先，看下SVG代码：

```XML
<svg width="400" height="200" viewBox="0 0 200 200" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>
```

截取SVG左边一半(200正好宽度400的一般)作为视区，里面有个`150*150`的红色矩形。

默认展示如下：

<svg width="400" height="200" viewBox="0 0 200 200" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>

如果我估计没错，默认应该是`"xMidYmid meet"`效果。

① 如果是`meet`效果，代码如下：  
```
<svg width="400" height="200" viewBox="0 0 200 200" preserveAspectRatio="xMinYMin meet" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>
```

截图效果如下：

<svg width="400" height="200" viewBox="0 0 200 200" preserveAspectRatio="xMinYMin meet" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>

表现原理为：SVG`宽400`, `高200`，`viewBox宽200`, `高200`. `x`横轴比例是`2`, `y`纵轴比例是`1`. `meet`的作用是让`viewBox`等比例的同时，完全在SVG的`viewport`中显示。这里，最小比例是纵向的`1`，所以，实际上`viewBox`并没有任何的缩放。

我们只要对`viewBox`属性值做一点小小的修改`（200→300）`，就可以感受到缩放了：
```XML
<svg width="400" height="200" viewBox="0 0 200 300" preserveAspectRatio="xMinYMin meet" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>
```

此时的显示效果为：  
<svg width="400" height="200" viewBox="0 0 200 300" preserveAspectRatio="xMinYMin meet" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>

改成`300`后，`viewBox`的高度就比`viewport`的`200`高，所以，`viewBox`要想完全适应`viewport`，就要进行缩放，所以，我们可以上到上面的矩形面积变小了，就是因为缩放的结果（缩放了`200/300`, 差不多原来的`66.7%`）。

② 如果是`slice`, `slice`本身就有剪切的意思。代码如下：
```XML
<svg width="400" height="200" viewBox="0 0 200 200" preserveAspectRatio="xMinYMin slice" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>
```

slice值下的效果：  

<svg width="400" height="200" viewBox="0 0 200 200" preserveAspectRatio="xMinYMin slice" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>

`slice`也是要保持`viewBox`的纵横比的，不过，其作用是尽量填满`viewport`. 同样，这里`viewBox`宽度`200`，SVG的`width`是`400`. 显然，要想最大化充满，`viewBox`的宽度就需要扩大为原来的两倍。于是，就有了上图`viewBox`放大两倍后的效果截图。由于`viewBox`部分区域超出了`viewport`, 视区之外内容是不可见的，于是就出现了`slice`所表意的`“剪切”`效果。

③ 如果是`none`, 则表示不关心比例，`viewBox`直接拉伸到最大填满`viewport`.

直接属性值"none"即可，例如：
```XML
<svg width="400" height="200" viewBox="0 0 200 200" preserveAspectRatio="none" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>
```

<svg width="400" height="200" viewBox="0 0 200 200" preserveAspectRatio="none" style="border:1px solid #cd0000;">
    <rect x="10" y="10" width="150" height="150" fill="#cd0000"/>
</svg>

默认状态下，以及使用工具导出的SVG图形子啊设置宽高的时候都是保持比例不拉伸的，这对于图标等尺寸控制非常有用，但如果我们希望SVG图形像图片默认行为一样，宽高设置直接拉伸，就是设置这里的`preserveAspectRatio="none"`即可！

![none时候的拉伸填充效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180022.png)

原本好好的一个正方形，现在因为viewBox的拉伸，变成了一个宽高2:1的矩形了。

## viewBox的对齐
千言万语不如一个可以自己动手体验的demo实在，您可以狠狠地点击这里：[viewBox的对齐各个名称表现感受demo](https://www.zhangxinxu.com/study/201408/svg-viewbox-alignment.html)

截两张图给大家瞅瞅：  
![viewBox对齐demo的截图](https://onepiece1991.github.io/img/in-post/post-svg/svg-180020.png)

![viewBox对齐演示demo的截图](https://onepiece1991.github.io/img/in-post/post-svg/svg-180021.png)

无论是`meet`还是`slice`，你是不可能在一种状态下同时看到`x`, `y`方向上的位移的。因为总会有一个方向是充满`viewport`的。


> 本文转自：[张鑫旭博客](http://www.zhangxinxu.com/wordpress/2014/08/svg-viewport-viewbox-preserveaspectratio/)