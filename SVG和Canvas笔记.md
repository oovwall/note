# SVG和Canvas笔记

## SVG
```html
<svg width="500px"
  height="500px"
  viewBox="0 0 100 100"
  style="background: white;"
>
    <rect x="10" y="10" width="20" height="10"/>
    <rect x="10" y="70" width="20" height="10"
    fill="transparent" stroke="black" stroke-width="0.1"
    stroke-opacity="0.4" stroke-dasharray="1" stroke-dashoffset="1"
    transform="translate(-10, 0)scale(1.1)"/>
    <polygon points="0,0, 10,0 20,5 5,5" />
    <circle cx="20" cy="30" r="5" />
    <ellipse cx="40" cy="30" rx="10" ry="5" />
    <line x1="40" y1="0" x2="40" y2="10" stroke="black" stroke-width="0.1" />
    <polyline points="45,0 50,0 50,15 40,15 45,0" fill="transparent" stroke="black" stroke-width="0.1"/>
    <text x="10" y="50">hello</text>
</svg>
```
### SVG形状标签
- rect 矩形
    - x 矩形左上角的x位置
    - y 矩形左上角的y位置
    - width 矩形的宽度
    - height 矩形的高度
    - rx 圆角的x方位的半径
    - ry 圆角的y方位的半径
- circle 圆形
    - r 圆的半径
    - cx 圆心的x位置
    - cy 圆心的y位置
- ellipse 椭圆形
    - rx 椭圆的x半径
    - ry 椭圆的y半径
    - cx 椭圆中心的x位置
    - cy 椭圆中心的y位置
- line 直线
    - x1 起点的x位置
    - y1 起点的y位置
    - x2 终点的x位置
    - y2 终点的y位置
- polyline 折线
    -points 点集数列。每个数字用空白、逗号、终止命令符或者换行符分隔开。每个点必须包含2个数字，一个是x坐标，一个是y坐标。所以点列表 (0,0), (1,1) 和(2,2)可以写成这样：“0 0, 1 1, 2 2”。
- polygon 多边形
- text 文字

### SVG的引用方式
1. HTML5：直接写入<svg>标签
1. 以通过 object 元素引用SVG文件：
    ```html
    <object data="image.svg" type="image/svg+xml" />
    ```
   
1. 类似的也可以使用 iframe 元素引用SVG文件：
    ```html
    <iframe src="image.svg"></iframe>
    ```
   
1. 用img元素引入
1. 最后SVG可以通过JavaScript动态创建并注入到HTML DOM中。
