---
layout:     post
title:      "SVG基础——使用Curve指令创建路径"
subtitle:   ""
date:       2016-05-09 10:20:00
author:     "Jxx"
header-img: "img/post-bg-180008.jpg"
tags:
    - SVG
---

## 曲线指令
曲线指令一共有三组，和直线指令一样，**指令字母大写表示坐标位置是绝对坐标，指令字母小写表示坐标位置是相对坐标**。

- 三次贝塞尔曲线指令 (C, c, S, s)
- 二次贝塞尔曲线指令 (Q, q, T, t)
- 椭圆弧线 (A, a)

## 三次贝塞尔曲线指令
通过定义一个起点和终点，以及两个控制点（一个控制起点，一个控制终点），绘制一条贝塞尔曲线。

三次贝塞尔曲线指令的格式为：
> C (or c) x1,y1 x2,y2 x,y

其中

- `x1`,`y1` 是曲线起点的控制点坐标
- `x2`,`y2` 是曲线终点的控制点坐标
- `x`,`y` 是曲线的终点坐标
- 曲线的起点是上一条指令的终点，所以不需要再次设置

示例1：
```SVG
<svg width="600" height="300">
    <path d="M100,200 C100,100 400,100 400,200" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```

路径从点(100,200)开始，这也是曲线的起点。三次贝塞尔曲线指令(C)绘制曲线，第一个控制点的坐标是(100,100)，第二个控制点是(400,100)。终点坐标是(400,200)。

![效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180002.png)

曲线从第一个控制点的方向开始，然后转弯，再根据第二个控制点的方向到达曲线终点。

示例2：添加第二条贝塞尔曲线来连接第一条,可以看到他们之间的衔接不是很平滑
```SVG
<svg width="1000" height="600">
    <path d="M100,200 C100,100 400,100 400,200 c100,200 400,100 300,0" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```

![效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180003.png)

通常，在连接曲线时，第二条曲线的起点控制点是第一条曲线终点控制点的对称点。S 和 s 指令是让浏览器帮你计算对称点的快捷方式，使用(S s)指令创建对称的控制点很容易。
```SVG
<svg width="1000" height="600">
    <path d="M100,200 C100,100 400,100 400,200 s100,100 300,0" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```
用一个相对定位的指令(s)快捷地创建了第二条曲线，来匹配使用相对坐标的曲线，两条曲线连接的交点处相比之前平滑了很多。

![效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180004.png)

示例3：使用绝对定位的指令(S)的快捷方式
```SVG
<svg width="1000" height="600">
    <path d="M100,200 C100,100 400,100 400,200 S400,100 300,0" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```

![效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180005.png)

## 二次贝塞尔曲线指令
二次贝塞尔曲线指令只需要一个控制点，同时作为起点控制点和终点控制点。
```SVG
<svg width="600" height="300">
    <path d="M100,200 Q250,100 400,200" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```
使用moveto指令作为开始，绘制二次贝塞尔曲线。曲线的起点坐标是(100,200)，终点坐标是(400,200)，控制点坐标是(250，100)

![效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180006.png)

二次贝塞尔曲线指令也有一个快捷方法，(T 或 t)指令可以用来平滑多条曲线之间的连接。它和三次贝塞尔曲线指令中的快捷方式(S 或 s)是同样的效果，也有助于通过对称的控制点来平滑地开始绘制第二条曲线。

```SVG
<svg width="600" height="300">
    <path d="M100,200 Q250,100 400,200 T600 200" fill="none" stroke="#000" stroke-width="2px" />
</svg>
```
![效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180007.png)
二次贝塞尔曲线指令是比较容易使用的，因为它只需要一个控制点。但是三次贝塞尔曲线指令有额外的控制点，所以可以提供更多的控制。

## 椭圆弧线
三种曲线指令中的最后一种是椭圆弧线(A 或 a)。下面这个弧线指令绘制了一段椭圆，它是曲线指令里边需要最多参数的，格式如下：
> A rx ry x-axis-rotation large-arc-flag sweep-flag x y

其中：
- x,y 是弧线的终点
- rx 是弧线在x轴方向的半径
- ry 是弧线在y轴方向的半径
- x-axis-rotation 是与x轴夹角的度数
椭圆弧绘制出的弧线是椭圆的一个片段。`若rx和ry的值相同，则绘制出圆。`

给定起点和终点坐标，以及rx ry x-axis-rotation的值，绘制出的弧线有四种可能，用下边的示例看起来比较清晰。

```SVG
<svg width="600" height="300">
    <path d="M250,100 A120,80 0 0,0 250,200" fill="none" stroke="red" stroke-width="5" />
    <path d="M250,100 A120,80 0 1,1 250,200" fill="none" stroke="green" stroke-width="5" />
    <path d="M250,100 A120,80 0 1,0 250,200" fill="none" stroke="purple" stroke-width="5" />
    <path d="M250,100 A120,80 0 0,1 250,200" fill="none" stroke="blue" stroke-width="5" />
</svg>
```
示例中，看起来好像有两个相同尺寸的彩色椭圆，但是这其实是四段不同的弧线，具体参考上方的代码。

![效果](https://onepiece1991.github.io/img/in-post/post-svg/svg-180008.png)

对照代码及图可知：
1.large-arc-flag 值为0表示使用较小的弧线，值为1表示使用较大的弧线。
2.sweep-flag 的值决定了是使用弧线(值为0)还是使用它的轴对称弧线(值为1)。

> 本文转自：[w3cplus](https://www.w3cplus.com/svg-tutorial)



