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
