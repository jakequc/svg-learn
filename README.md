## About

This is the repository I used to learn svg

### SVG 文件的基本属性

- svg 文件的规则是“后来居上”，越后面的元素越可见
- web 上的 svg 文件可以直接在浏览器上展示，或者通过以下几种方法嵌入到 HTML 文件中：
  - 如果 HTML 的 XMHTL 并且声明类型为 application/xhtml+xml, 可直接把 svg 嵌入到 XML 源码中
  - 可以通过 object、iframe、img 元素中引入 svg 文件
  - svg 可以通过 js 动态常见并注入到 HTML DOM 中，这样可以对浏览器使用替换技术，在不能解析 svg 的情况下，替换创建的内容

```code
<object data="image.svg" type="image/svg+xml"/>
<iframe src="image.svg"></iframe>
```

svg 的文件格式有： .svg / .svgz （svg 允许 gzip 压缩）
⚠️： .svg 的文件如果写的时候需要使用 `<?xml version="1.0" standalone="no"?>` 然后才以 <svg> 作为根节点开始定义你的 svg

### SVG 的坐标

SVG 的坐标是以 svg 的 左上角为 原点的， y 轴垂直向下
svg 定义绝对大小的方式： 给出数字，不明确单位，输出时就会采用用户的单位（如 px,pt,cm 等）在没有规范说明的情况下 `一个用户单位等于 1 个屏幕单位`

```code
<svg  width="200" height="200" viewBox="0 0 100 100"> </svg>
/**
 svg 定义的画布尺寸时 200*200px ，但是 viewBox 属性定义了可显示区域为 从(0,0) 开始，100(width)*100（height）的区域
 */
```

### SVG 的基本形状

> （线、文本或者元素）如下成为图形，因为 svg 中的元素都可以简单的看为是一种可缩放图形

#### 大多数通用属性

-（x,y）定义图形的左上角位置

- width\height 定义图形的宽高
- rx/ry 定义 x，y 方向的圆角半径
- stroke 定义线、文本或者元素轮廓的颜色（相当于 css 中的 border-color）
- stroke-width 定义图形轮廓的宽度
- strokelinecap 定义不同类型的开放路径的终结； stroke-linecap="black" 或者 butt round square
- stroke-dasharray 用于创建虚线； stroke-dasharray="5,5,10,3" 这里的 array 体现在使用逗号分隔

#### 定义矩形 rect

```code
   <rect x="60" y="10" rx="10" ry="10" width="30" height="30" stroke="black" fill="transparent"
        stroke-width="5" />
```

#### 定义圆形 circle ⭕️

```code
    <circle cx="25" cy="75" r="20" stroke="red" fill="transparent" stroke-width="5" />
```

说明： r 为圆形的半径（radius）、cx 为圆心 x 的位置、cy 为圆心的位置

#### 定义椭圆 ellipse

```code
   <ellipse cx="75" cy="75" rx="20" ry="5" stroke="red" fill="transparent" stroke-width="5" />
```

说明： cx 设置椭圆中心的 x 位置， cy 设置椭圆中心的 y 位置；
rx 设置 椭圆的 x 半径（长轴半径）， ry 设置椭圆的 y 半径(短轴半径)

#### 定义线条 line

```code
      <line x1="10" x2="50" y1="110" y2="150" stroke="orange" fill="transparent" stroke-width="5" />
```

说明： line 是线，线是连接亮点形成的，因此 (x1，y1) 为 线的起点坐标，(x2,y2) 为线的终点坐标

#### 定义折现 polyline

```code
    <polyline points="60 110 65 120 70 115 75 130 80 125 85 140 90 135 95 150 100 145"
        stroke="orange" fill="transparent" stroke-width="5" />
```

说明：polyline 是一组连接在一起的 line 组成的，因为他有很多的点，因此折现的所有点位置都放在 points(点集数列) 中，每个点之间使用 `,` 来分割

#### 定义多边形 polygon

```code
    <polygon points="50 160 55 180 70 180 60 190 65 205 50 195 35 205 40 190 30 180 45 180"
        stroke="green" fill="transparent" stroke-width="5" />
```

说明： polygon 和 polyline 很相似，不同的是多边形 points 是闭环的，即最后一个点会自动和 第一个点相互连接 形成一个闭合图形

#### 定义路径 path

```code
   <path d="M20,230 Q40,205 50,230 T90,230" fill="none" stroke="blue" stroke-width="5" />
```

说明：路径是 svg 中最常见的 形状（也是最强大的），你可以用 path 元素绘制矩形（直角矩形或者圆角矩形）、圆形、椭圆、折线、多边形 或者其他形状（如 贝塞尔曲线等）d 为一个点集数列以及其他关于如何绘制路径的信息 `属性 d 的值 是一个 命令 + 参数 的序列`； 因为属性 d 采用的是用户坐标系统，所以不需要标明单位

- 命令简称说明 （path 元素中的命令）

  - M/m 移动命令；比如 M 20 230 表示： Move to (20,230) 这个坐标，指明何处开始画，移动画笔位置，但是不画线
  - L/l 画线命令；
  - V/v 绘制垂直线命令； 比如 V 90 表示 y 轴移动到 90 画一条垂直的线
  - H/h 绘制水平线命令； 比如 H 90 表示 y 轴移动到 90 画一条水平线
  - Z/z 闭合路径命令，Z 命令会从当前点画一条直线到路径的起点，经常被放在路径的最后
  - C/c 三次比赛尔曲线 C x1 y1, x2 y2, endPoint endPoint； (endPoint,endPoint) 曲线的终点，（x1,y1） 是起点的控制点，（x2,y2） 是终点的控制点
  - S/s 简写的 贝塞尔曲线命令 S/s x2 y2, x y
  - Q/q 二次贝塞尔曲线命令 Q/q cx c2, x y ；（cx,c2）作为控制点，用来确定起点和终点的曲线斜率。因此它需要两组参数，控制点和终点坐标(x，y)
  - T 可以通过更简短的参数，延长二次贝塞尔曲线 T x y
  - A 弧线命令 A/a rx ry x-axis-rotation large-arc-flag sweep-falg arcx arcy；
    rx 是 x 轴的半径，ry 是 y 轴的半径 ；x-axis-rotation 表示 x 轴的旋转情况;
    large-arc-flag(角度大小)决定弧线是大于还是小于 180 度，0 表示 小角弧，1
    表示 大角弧； sweep-flag 表示弧线的方向，
    0 表示从起点开始到终点沿逆时针画弧，1 表示从 起点到终点沿顺时针画弧;(arcx,arcy)定义的是弧线的最终位置

- S/s 可以和其之前 C 三次贝塞尔曲线的最后终点自然连接

- T/t 会通过前一个控制点，推断出一个新的控制点，这意味着在第一个控制点后边，可以只定义终点，就可以创造出一条曲线，需要注意的是，T 命令前面必须是一个 Q 命令，或者是另一个 T 命令；如果 T 单独使用，那么控制点会被认为和终点是同一个点，所以画出来将是一条曲线

#### 填充和边框

- fill 定义图形的填充颜色、fill-opcity 定义填充颜色的不透明度
- stroke 定义图形的线条颜色、stroke-width 定义线条的宽度、stroke-opacity 定义线条颜色的不透明度
- 描边 stroke-linecap 用来控制描边的方式，控制边框终点的形状，其值有三：
  - butt 用直边结束线段，线段边界 90 度垂直于描边的方向、贯穿他的终点（粗图线条）
  - square 和 butt 差不多，但是比 butt 要长得多
  - round 边框是圆角的，圆角的半径有 stroke-width 控制
- stroke-linejoin 控制两条描边线段之间的连接方式
  - miter 默认就是它，表示斜接连接（会形成一个直角）
  - round 圆角连接，看起来平滑
  - bevel 斜面连接（连接处会形成一条线）
- stroke-dasharray="areaLen1 areaLen2 areaLen3" 可以将虚线类型应用在锚边上。
  - ⚠️： stroke-dasharray 属性的参数，使用一组用逗号分隔的数字组成的数列，和 path 不一样，这里的数字必须用逗号分隔（空格会被忽略）。每一组数字，奇数位置的数字表示填色区域的长度，偶数位置用来表示非填色区域的长度，如果指定的长度不够，这个将会进行循环模式渲染
- 其他填充和边框属性
  - fill-rule 定义如何给图形重叠的区域上色；
  - stroke-miterlimit 定义什么情况下绘制或者不绘制边框连接的 miter 效果
  - stroke-dashoffset 定义虚线开始位置

#### 使用 CSS

除了定义对象的属性外，你也可以通过 CSS 来样式化 fill 和 stroke， 语法和在 HTML 里使用 CSS 一样，只是需要你把 background-color、border 改成 fill 和 stroke
⚠️： 不是所有的属性都可以用 css 来设置， 上色和填充的部分可以用 css 来设置（比如 fill, stroke,stroke-dasharray 等，但是不包括渐变和图案等功能。另外，width,height 以及路径的命令不能用 CSS 来设置

`SVG 规范将属性区分为 properties 和其他的 attributes， properties 是可以使用 css 来设置的，attributes 不可以使用 css 来设置`

##### svg style 的行内式

```code
 <rect
    x="10"
    height="180"
    y="10"
    width="180"
    style="stroke: black; fill: red"
  />
```

##### svg style 的内嵌式

主要是在 svg 的 <defs> 标签中定义一些不会在 SVG 图形中出现，但是可以被其他元素使用的元素

```code
 <svg
    width="200"
    height="150"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1"
  >
    <defs>
      <style>
        #myRectSvg {
          stroke: black;
          fill: red;
        }


        #myRectSvg:hover {
          stroke: black;
          fill: blue;
        }
      </style>
    </defs>

    <rect x="10" height="180" y="10" width="180" id="myRectSvg" />
  </svg>
```

#### 渐变

##### 线性渐变

线性渐变沿着直线改变颜色，要插入一个线性渐变，需要在 svg 文件的 defs
元素内部创建 linearGradient 节点

```code
<svg
  width="120"
  height="240"
  version="1.1"
  xmlns="http://www.w3.org/2000/svg"
>
  <defs>
    <linearGradient id="Gradient1">
      <stop class="stop1" offset="0%" />
      <stop class="stop2" offset="50%" />
      <stop class="stop3" offset="100%" />
    </linearGradient>

    <!-- 垂直渐变 -->
    <linearGradient id="Gradient2" x1="0" x2="0" y1="0" y2="1">
      <stop offset="0%" stop-color="red" />
      <stop offset="50%" stop-color="black" stop-opacity="0.5" />
      <stop offset="100%" stop-color="blue" />
    </linearGradient>

    <style type="text/css">
      #rect1 {
        fill: url(#Gradient1);
      }
      .stop1 {
        stop-color: red;
      }
      .stop2 {
        stop-color: black;
        stop-opacity: 0;
      }
      .stop3 {
        stop-color: blue;
      }
    </style>
  </defs>

  <rect id="rect1" x="10" y="10" rx="15" ry="15" width="100" height="100" />

  <rect
    x="10"
    y="120"
    rx="15"
    ry="15"
    width="100"
    height="100"
    fill="url(#Gradient2)"
  />
</svg>
```

如上用到的节点属性说明：

- stop 为线性渐变的内部 节点，节点通过指定位置的 offset（偏移）属性和 stop-color（颜色值）属性来说明在渐变的特定位置应该是什么颜色； offset 应该从 0%（or 0）到 100% （or 1） 结束；如果 stop 设置的位置有重合，将使用 xml 树中较晚设置的值
- stop-opacity 设置某个位置的半透明度
- fill 此时的值 是 url(#gradientId) 表示引用某个渐变
- linearGradient 元素可以通过 x1,y1 x2,y2 两个点来决定 渐变的方向，渐变色默认是水平方向的
- 可以在渐变上使用 xlink:href 属性，如果使用了该属性时，一个渐变的属性和颜色中值(stop) 可以被另一个渐变包含引用

##### 径向渐变

> 径向渐变与线性渐变类似，只是它是从一个点开始发散绘制渐变。创建径向渐变需要在文档 defs 中添加一个 radialGradient 元素

```code
 <svg width="120" height="120">
  <defs>
    <radialGradient
      id="Gradient"
      cx="0.5"
      cy="0.5"
      r="0.5"
      fx="0.25"
      fy="0.25"
    >
      <stop offset="0%" stop-color="red" />
      <stop offset="100%" stop-color="blue" />
    </radialGradient>
  </defs>

  <rect
    x="10"
    y="10"
    rx="15"
    ry="15"
    width="100"
    height="100"
    fill="url(#Gradient)"
    stroke="black"
    stroke-width="2"
  />
  <circle
    cx="60"
    cy="60"
    r="50"
    fill="transparent"
    stroke="white"
    stroke-width="2"
  />
  <circle cx="35" cy="35" r="2" fill="white" stroke="white" />
  <circle cx="60" cy="60" r="2" fill="white" stroke="white" />
  <text
    x="38"
    y="40"
    fill="white"
    font-family="sans-serif"
    font-size="10pt"
  >
    (fx,fy)
  </text>
  <text
    x="63"
    y="63"
    fill="white"
    font-family="sans-serif"
    font-size="10pt"
  >
    (cx,cy)
  </text>
</svg>
```

- 径向渐变属性的说明
  - cx, cy 组成是 径向渐变的圆心点，描述渐变边缘的位置
  - fx, fy 组成是 径向渐变的焦点，定义渐变的中心
  - r 定义径向渐变的半径

##### 线性渐变 和 径向渐变 的共有属性

- spreadMethod 定义了渐变达到终点的行为，有如下三值：
  - pad 当渐变达到终点时，最终的偏移颜色会填充剩余口空间
  - reflect 从 100% 偏移位置开始渐变到 0% 位置，然后在回到 100% 偏移位置颜色
  - repeat 会让渐变颜色重复到 从 0% 开始 到 100%，然后再到 0% 的位置
- gradientUnits 渐变单元，用来描述渐变的大小和方向的单元系统，有如下值
  - userSpaceOnUse 使用绝对单元，你必须知道对象的位置，并将渐变放在同样地理位置
  - objectBoundingBox （默认就是这个值），定义了对象的渐变大小范围，需要指定从 0 到 1 的坐标值，渐变就会自动的缩放到对象相同大小

#### pattern 定义组合图案

> pattern 元素内部你可以包含其他基本形状，它的作用就是将这些基本形状根据你的设计与样式组合成一个图案

- pattern 的属性
  - patternUnits 描述使用的属性单元
    - objectBoundingBox 为其默认值
  - width、height 的百分比形式用于设置 水平和垂直距离 的次数（.25 表示重复 4 次）
  - x、y 设置 pattern 的偏移量
  - patternContentUnits 描述 pattern 元素基于形状使用的单元系统，这个属性默认值 为 userSpaceOnUse
