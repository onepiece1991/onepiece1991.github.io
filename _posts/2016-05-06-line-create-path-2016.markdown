---
layout:     post
title:      "SVG基础——使用Line指令创建路径"
subtitle:   ""
date:       2016-05-06 10:00:00
author:     "Jxx"
header-img: "img/post-bg-180008.jpg"
tags:
    - SVG
---

`SVG` 意为可缩放矢量图形（Scalable Vector Graphics）。
`SVG` 使用 XML 格式定义图像。

### SVG的常用属性
- `fill`:元素的填充颜色
- `fill-opacity`:元素的填充颜色透明度
- `stroke`:元素的笔画颜色
- `stroke-width`:元素的笔画宽度
- `stroke-opacity`:元素的笔画颜色透明度

SVG的[属性参考](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute)和[元素参照](https://developer.mozilla.org/en-US/docs/Web/SVG/Element)

## 路径元素
使用命名合适的路径元素创建一条路径，在路径元素内，通过一系列的指令和坐标定义了如下的一条路径。  
```XML
<svg>
    <path d="path data" />
</svg>
```
定义的路径(`d`)使用了不同的指令，移动到一个新的点，然后绘制不同的直线和曲线。

```XML
<svg>
    <path d="path data" pathLength="non-negative-number" />
</svg>
```
路径元素可以带有一个`pathLength`属性。这个属性接收一个`非负数`作为值，并使用这个值来覆盖浏览器计算的路径长度。

## 直线指令
有`五种不同的直线指令`:
- `moveto(M 或 m)`:移动到新的位置
- `lineto(L 或 l)`:从当前坐标画一条直线到一个新坐标
- `horizontal lineto(H 或 h)`:画一条水平线到新坐标
- `vertical lineto(V 或 v)`:画一条垂直线到新坐标
- `closepath(Z 或 z)`:关闭当前路径

不需要将指令拼写出来，使用指令字母即可。指令字母大写表示坐标位置是绝对位置，指令字母小写表示坐标位置时相对位置。

## moveto和lineto指令
示例:
```SVG
<svg width="600" height="400">
    <path d="M 50 50 l 0 300 l 200 0 l 0 -300 l -200 0" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```
1. 创建一个SVG元素，并设置了一个高度和宽度的值。另外设置填充fill为none，并为路径加了一个描边stroke-width。这样我们就可以看到实际的直线路径，而不是一个填充图形。
2. 路径的数据以移动到一个坐标为 x=50 y=50 (M 50 50)的点为开始，每条路径都需要以一个moveto指令为开始。
3. 接下来是一个lineto指令，这里设置一个小写字母 l (l 0 300)。这个指令指的是从当前坐标（50 50）画一条直线到相对坐标 (0 300)。也就是说，直线会在水平方向绘制0px，在垂直方向绘制300px。
4. 接着，是另一个lineto指令 (l 200 0)，绘制一条200px的水平线。后面的两条指令是 (l 0 -300)和 (l -200 0)，这两条指令绘制了另外两条线，一条水平线和一条垂直线，和初始的水平线和垂直线在相反的方向上。
5. 默认的单位是px（像素）
6. 路径数据是不使用逗号隔开的指令列表，这些指令是一个跟着一个

下面是绘制出来的完整的路径：
![200*300矩形](https://onepiece1991.github.io/img/in-post/post-svg/svg-180001.png)

该路径首先移动到一个起点上，然后垂直向下绘制一条直线，再一条往右，一条往上，最后一条往左，回到起点结束路径。

## closepath指令
```SVG
<svg width="600" height="400">
    <path d="M 50 50 l 0 300 l 200 0 l 0 -300 Z" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```
用一个closepath指令(Z)替换了最后一个lineto指令(l)。因为没有指定坐标，所以使用 Z 和 z 效果是一样的。示例效果完全一样

## horizontal lineto 和 vertical lineto指令
```SVG
<svg width="600" height="400">
    <path d="M 50 50 v 300 h 200 v -300 Z" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```

这里使用了vertical lineto (v) 和horizontal lineto (h) 指令替换了中间的三个lineto指令。这两个指令只需要x和y两个坐标值中的一个，另一个值它自己会确定为0。

## 样式、空格和逗号
- 通过移动或者添加空格，使得各个指令以及整个路径的数据阅读起来更简明
```SVG
<svg width="600" height="400">
    <path d="M50 50  v300 h200 v-300 Z" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```
- 逗号不能在指令之间使用，但是可以在坐标x和y之间使用
```SVG
<svg width="600" height="400">
    <path d="M50,50  v300 h200 v-300 Z" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```
- 也可以每一个指令占据一行（但是这样会占据大部分空间）。在WordPress中内联SVG的唯一解决方案是，所有的代码都要写在一行上。
```SVG
<svg width="600" height="400">
    <path d="M50 50 
            v300
            h200
            v-300
            Z" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```

> 本文转自：[w3cplus](https://www.w3cplus.com/svg-tutorial)