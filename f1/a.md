今天我要画的是Needham书里的第一章的第一幅图（原版的Figure[1]）。

![want](https://github.com/leperca/wpmc/blob/master/f1/want.png)

所以，我应该随便打开一个文件，就命名为figure1_1.html 开始写代码了。
所有的文件我都写成html文件吧，也就是这样子的。
```html
<html>
<body>
    <script>
           // 上帝说要有光。
           // 这里是开始的地方。
           // .......
           // 以上是所有的代码。
    </script>
</body>
</html>
```

接下来既然是画图，就有两种选择：一种是svg，一种是canvas。其实我知道的也不是特别多，前者是矢量图，后者是像素图。然后矢量有矢量的好处，像素有像素的好处。具体的好处百度一下就有了...
我们先选择 svg。 
然后第一件事情就是把svg放入到html中，注意这里，将一切都放在js中进行。代码如下：
```js
const svgns = "http://www.w3.org/2000/svg";
let svg = document.createElementNS(svgns, "svg");
svg.setAttribute("xmlns", svgns);
svg.setAttribute("width", 800);
svg.setAttribute("height", 400);
svg.setAttribute("style", "border:1px solid green");
document.body.appendChild(svg)
```

以上const 是常量命名，给svgns定义一个常量，以便后面方便使用。然后是createElementNS创建svg标签，设置其宽度为800，高度为400，并且我们设置了一个边界为绿色方便识别。于是我们将得到如下。
![want](https://github.com/leperca/wpmc/blob/master/f1/s1.png)

接着是我们要做的第一件non-trivial(不平凡)的事情了。画第一条直线。当然了，我是不会的，我就去找了个进了w3c网站关于svg的介绍看看，例子给出的是，我们可以这样子画线。

```html
<line x1="0" y1="0" x2="300" y2="300" style="stroke:rgb(99,99,99);stroke-width:2"/>
```

好的，那么我们就来画这条线。
follow上面的步骤，我们先创建元素，然后修改里面的属性，再添加到图片里。当然没明白我说什么也没关系，反正就是固定的套路。就像这样：
```js
let line = document.createElementNS(svgns, "line");
line.setAttribute("x1", 0);
line.setAttribute("y1", 0);
line.setAttribute("x2", 300);
line.setAttribute("y2", 300);
line.setAttribute("style", "stroke:rgb(99,99,99);stroke-width:1");
svg.appendChild(line);
```
然后结果就是这样子的。
![want](https://github.com/leperca/wpmc/blob/master/f1/s2.png)

当然，原因是我没有调整上面的参数。这个w3c上的参数到我们这里的参数套路就是上面这样子的，待会见到更多的。其中两点定义一条直线：第一个点(x1,y1)以及第二个点(x2,y2)。
注意这里的原点是左上方，坐标轴x没变，y轴从上到下为正方向。

好啦，我们准备调个参数，先把第一条直线先画对了，然后再是第二条。当然了，可能会觉得上面的代码太繁杂了，不简洁，没事，我们不着急，一点点来，待会再弄整洁。
```js
let line = document.createElementNS(svgns, "line");
line.setAttribute("x1", 0);
line.setAttribute("y1", 0);
line.setAttribute("x2", 00);
line.setAttribute("y2", 300);
line.setAttribute("style", "stroke:rgb(99,99,99);stroke-width:1");
svg.appendChild(line);
```
多试几次就会出来具体的结果了，就是调整两个点。当然，其实没有弄明白坐标轴啊，各个参数的意思搞混了也没有关系，调个参数看看结果，就知道大概往哪个方向调整了。没错，我就是没有经过简单的计算得到以下数字的，改了好几次数字，明明可以算出来的。哈哈哈，没事，不打紧。结果修改为红色部分的代码：
```js
let line = document.createElementNS(svgns, "line");
line.setAttribute("x1", 0);
line.setAttribute("y1", 200);
line.setAttribute("x2", 800);
line.setAttribute("y2", 200);
line.setAttribute("style", "stroke:rgb(99,99,99);stroke-width:1");
svg.appendChild(line);
```

结果为：
![want](https://github.com/leperca/wpmc/blob/master/f1/s3.png)

好的，那么再画第二条线。此时我抑制了一下整理代码的冲动，以及给前面的线条改名字的冲动。代码竖线代码如下。
```js
let line2 = document.createElementNS(svgns, "line");
line2.setAttribute("x1", 400);
line2.setAttribute("y1", 0);
line2.setAttribute("x2", 400);
line2.setAttribute("y2", 400);
line2.setAttribute("style", "stroke:rgb(99,99,99);stroke-width:1");
svg.appendChild(line2);
```
好了，这个结果如下：
![want](https://github.com/leperca/wpmc/blob/master/f1/s4.svg)
上面这个图片svg格式的，github上显示的时候出现了点问题--没有了下边和右边的绿色边框不见了。算了，不管它继续重要的事情。

看到了上面的内容，我们学会了画线的方法。于是可以把上头的画线方法包装成一个函数。我想了想，算了，暂时还是不做这样的事情，先把所有事情都处理完，再去包装整理代码吧。
下面开始要话真实的点了。先不管三七二十一，先上w3c上面看看怎么画个点。好吧，没有看到点，那就画个圈也行，如下
```html
<circle cx="100" cy="50" r="40" stroke="black"
stroke-width="2" fill="red"/>
```
好的，那么我们画个点，其他都不变，照猫画虎。
```js
let origin = document.createElementNS(svgns, "circle"); 
//注意需要改成“circle”
origin.setAttribute("cx", 100); 
origin.setAttribute("cy", 0);
origin.setAttribute("r", 40);
origin.setAttribute("stroke", "black");
origin.setAttribute("stroke-width", "2");
origin.setAttribute("fill", "red");
svg.appendChild(origin);
```
好的，虽然复制粘贴了一下代码，并且命名上也显得有点随意，但是无妨，简陋一点呗，结果如下。
![want](https://github.com/leperca/wpmc/blob/master/f1/s5.svg)

结果其实挺丑的，哈哈哈。改参数，边调整，边看结果，
此时可以借助chrome的调试工具，就可以改成如下结果。

```js
let origin = document.createElementNS(svgns, "circle"); 
//注意需要改成“circle”
origin.setAttribute("cx", 400); 
origin.setAttribute("cy", 200);
origin.setAttribute("r", 5);
origin.setAttribute("stroke", "black");
origin.setAttribute("stroke-width", "1");
origin.setAttribute("fill", "white");
svg.appendChild(origin);
```
![want](https://github.com/leperca/wpmc/blob/master/f1/s6.png)

由上面的内容我们可以知道，
其实有两套的数字，一个是真实的点的坐标，另外一个是图像上的坐标。显然这两个不是一致的，我们需要做一些调整。这就是我们可能需要花时间的地方。为了方便，构造一个函数来帮助我们做坐标转化的事情。

这里涉及到两个正逆过程，一个是把真实的坐标转化成为svg图像上的坐标【正过程】，
另一个是把svg图像上的坐标转化成为真实坐标【逆过程】。这样说起来有点含糊，一边画点，一边来使问题更加清晰。
例如我想画个点，真实的坐标是（0，0）的原点，然而在svg图像上它的坐标是(400，200)
这个是我们上边调整得来的结论。意思也很明白。
那么我们就写这样一个函数吧，就是把（0，0）变成为（400，200）的函数：
```js
function real2svg_funny(x, y) {
    if (x == 0 && y == 0) {
        return [400,200]
    }
}
```
哈哈，显然，上面这个函数只能做那么一件事情。就是放入一个（0，0），输出一个（400，200）。非常完美。当然，这有点搞笑了，它不能解决一个问题，我们希望放入一个(4,3)它可以得到一个其他数字，例如（某数，某数）。很糟糕，需要稍微算个数字。

好吧，那就计算一下吧。我们整理一下思路。大概要这样一件事情，目的是将真实的坐标转化成为svg图像上的坐标（逆过程现在先不管，到时候再来重新照猫画虎）。
分为两个步骤，一个是放缩盒子大小；第二个是把坐标系的摆正（待会细说）。
而我们拥有的是svg图像的盒子大小为：宽度800，高度400；数一下needham那个图片的格子，我们知道那里面是，x轴为-9到9，y轴是-4到4。

所以很明白的一点是要做这样的事情，例如对于x轴，给定一个-9到9的数字，它应该给出一个[0,800]之间的数字，当然，我们还需要将原点的0点变到400去。
我们知道这个是放大过程加平移过程,也就是线性变换过程：
即如下两个过程 [-9,9] => [-400,400] => [0,800]
1）即把长度为18的东西，变成长度为800的东西，放大了800/18；
2）把[-400,400] 平移称为 [0,800]
所以写个这样的函数：
```js
function real2svg_X_step1(x) {
    return x*800/18
}
```
以及
```js
function real2svg_X_step2(x) {
    return x+400
}
```
再稍微思考一下， 这里出现了挺讨人厌的数字，我们把数字抹掉，变成参数传进去。也就是svg_width的宽度和真实坐标的盒子宽度（real_width）。
```js
function real2svg_X_step1(x,svg_width,real_width) {
    return x*svg_width/real_width
}
```
以及平移原点x坐标。
```js
function real2svg_X_step2(x,origin_shiftX) {
    return x+origin_shiftX
}
```
二者组装起来，就是这样子的函数：
```js
function real2svg_X(x,svg_width,real_width,origin_shiftX) {
    return x*svg_width/real_width+origin_shiftX
}
```
如果很早之前就知道是线性变换 x'=k x + b 形式就不需要拆开来这么细节去写。
但是，既然是写程序，必须要暴露细节出来，自己才能清楚。
于是，我们可以把y方向的坐标也这样一写。
```js
function real2svg_Y(y,svg_height,real_height,origin_shiftY) {
    return y*svg_height/real_height+origin_shiftY
}
```
在我们的问题里面，svg_height = 400, real_height = 8, 
以及 origin_shiftY = svg_height/2 （即为svg_height的一半）。
但是呢，由于这个svg坐标系统和我们常用的坐标系统在y轴的正方向上有差别，
所以我们必须要把这个方向给转过来。想不清楚也没有关系，就是哪里把正号边成负的。
例如：y轴上为4的点变到svg坐标y上为0，即 4 ===> 0。
于是根据上头的式子（real2svg_Y）应该有 4*400/8+200=400,
但是呢，我们真实的是需要是0，所以，给y上面打个负号就成了即：-4*400/8+200 = 0;
代码就是这样子的：
```js
function real2svg_Y(y,svg_height,real_height,origin_shiftY) {
    return -y*svg_height/real_height+origin_shiftY
}
```
为了防止出错，我们再代入个其它值试试就可以了。
所以我们就得到了两个函数，
```js
function real2svg_X(x,svg_width,real_width,origin_shiftX) {
    return x*svg_width/real_width+origin_shiftX
}
function real2svg_Y(y,svg_height,real_height,origin_shiftY) {
    return -y*svg_height/real_height+origin_shiftY
}
```

接着我们来试试看，画个坐标为(4,3)这个点的圆圈。
```js
let point1x = real2svg_X(4,800,18,400)
let point1y = real2svg_Y(3,400,8,200)

let point1 = document.createElementNS(svgns, "circle");
//注意需要改成“circle”
point1.setAttribute("cx", point1x);
point1.setAttribute("cy", point1y);
point1.setAttribute("r", 5); //半径变小了
point1.setAttribute("stroke", "black");
point1.setAttribute("stroke-width", "1");
point1.setAttribute("fill", "black"); //改了填充色
svg.appendChild(point1);
```
![want](https://github.com/leperca/wpmc/blob/master/f1/s7.svg)

由于我们还想画点(-7,1)，（-2，-3），（2，-2）还有（0，3）。
所以我们最终还是要包装一下上面的函数了。不然写起来太麻烦了。
包装一下上面的函数为create_point(x,y,svg)。这里的svg就是我们的图像，
里面的x和y是svg上的坐标。如下：
```js
function create_point(x,y,svg){
    let point1 = document.createElementNS(svgns, "circle");
    //注意需要改成“circle”
    point1.setAttribute("cx", x);
    point1.setAttribute("cy", y);
    point1.setAttribute("r", 5); //半径变小了
    point1.setAttribute("stroke", "black");
    point1.setAttribute("stroke-width", "1");
    point1.setAttribute("fill", "black"); //改了填充色
    svg.appendChild(point1);
}
```
其他的一股脑儿都包进去，也就不暴露设置样式的接口了，不重要。

再者为了后文的使用方便，我们给坐标变化那里的函数设置默认值，
并给了个新的名字，rsx以及rsy且给它们设置了默认值，
于是我们画出点(-7,1)，（-2，-3），（2，-2）还有（0，3）,（0，4）。
```js
function rsx(x,svg_width=800,real_width=18,origin_shiftX=400) {
    return x*svg_width/real_width+origin_shiftX
}
function rsy(y,svg_height=400,real_height=8,origin_shiftY=200) {
    return -y*svg_height/real_height+origin_shiftY
}
create_point(rsx(-7),rsy(1),svg)
create_point(rsx(-2),rsy(-3),svg)
create_point(rsx(2),rsy(-2),svg)
create_point(rsx(0),rsy(3),svg)
create_point(rsx(4),rsy(0),svg)
```
![want](https://github.com/leperca/wpmc/blob/master/f1/s8.svg)

好了，下面我们就要开始画箭头了。箭头怎么画，我不知道，先查了下别人怎么画的。
如果不想自己写网上就有一些代码可以现场拷贝过来。但是呢，我还是自己动手自己写个。哈哈哈。
我就画了个这样的箭头。
![want](https://github.com/leperca/wpmc/blob/master/f1/s9.jpg)
那么我们就是要画这样的东西（上面这个图片好大好丑，哈哈哈，没事不管）。

我们可以看看如何用path画上面那个图，百度下，svg path。
一般都是w3c, 或者像 runoob 这个网站也成。
或者是mdn上的例子https://developer.mozilla.org/en-US/docs/Web/SVG/Element/marker，
```html
<svg width="120" height="120" viewBox="0 0 120 120"
    xmlns="http://www.w3.org/2000/svg" version="1.1">

  <defs>
    <marker id="Triangle" viewBox="0 0 10 10" refX="1" refY="5"
        markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" />
    </marker>
  </defs>

  <polyline points="10,90 50,80 90,20" fill="none" stroke="black" 
      stroke-width="2" marker-end="url(#Triangle)" />
</svg>
```
我大概需要变成：
```html
<path d="M 0 0 L 10 5 L 0 10 L 5 5 Z" />
```
关于M L Z 这些网站上有介绍，具体的样子我们稍微做了改动。
![want](https://github.com/leperca/wpmc/blob/master/f1/s9.jpg)

于是在我们的svg上的的js代码应该是这样的：
```js
// 定义标签
let my_defs = document.createElementNS(svgns, "defs");
let my_marker = document.createElementNS(svgns, "marker");
my_marker.setAttribute("id", "Triangle"); //注意设置id 
my_marker.setAttribute("viewBox","0 0 10 10");
my_marker.setAttribute("refX", "12"); 
my_marker.setAttribute("refY", "5");
my_marker.setAttribute("markerWidth","6")
my_marker.setAttribute("markerHeight","6")
my_marker.setAttribute("orient", "auto");
// 三角形的arrow
let arrow = document.createElementNS(svgns, "path");
arrow.setAttribute("d", "M 0 0 L 10 5 L 0 10 L 5 5 Z");
arrow.setAttribute("fill", "#000000");
// 标签添加到svg图上
my_marker.appendChild(arrow);
my_defs.appendChild(my_marker);
svg.appendChild(my_defs);

let aline2 = document.createElementNS(svgns, "line");
aline2.setAttribute("x1", rsx(0));
aline2.setAttribute("y1", rsy(0));
aline2.setAttribute("x2", rsx(4));
aline2.setAttribute("y2", rsy(3));
aline2.setAttribute("style", "stroke:#000;stroke-width:2");
aline2.setAttribute("marker-end","url(#Triangle)") // <<<<这里引用上Triangle
svg.appendChild(aline2);
```
上面我们已经要把直线和箭头装上去了。有两种方法，一种就是每次都画一个直线，再把箭头也一起放上去，这会涉及到箭头的旋转的问题。另外一种就是使用svg中的marker。上面的例子就是如此。
最后结果如图。
![want](https://github.com/leperca/wpmc/blob/master/f1/s10.svg)

好了，包装一下函数，我们就可以画箭头了。

```js
// 包装marker
function create_marker(svg, idname) {
    // 定义标签
    let my_defs = document.createElementNS(svgns, "defs");
    let my_marker = document.createElementNS(svgns, "marker");
    my_marker.setAttribute("id", idname); //注意设置id 
    my_marker.setAttribute("viewBox", "0 0 10 10");
    my_marker.setAttribute("refX", "12");
    my_marker.setAttribute("refY", "5");
    my_marker.setAttribute("markerWidth", "6")
    my_marker.setAttribute("markerHeight", "6")
    my_marker.setAttribute("orient", "auto");
    // arrow
    let arrow = document.createElementNS(svgns, "path");
    arrow.setAttribute("d", "M 0 0 L 10 5 L 0 10 L 5 5 Z");
    arrow.setAttribute("fill", "#000000");
    // 标签添加到svg图上
    my_marker.appendChild(arrow);
    my_defs.appendChild(my_marker);
    svg.appendChild(my_defs);
    return idname
}
// 包装箭头函数
function create_arrow(x1, y1, x2, y2, svg,idname) {
    let aline2 = document.createElementNS(svgns, "line");
    aline2.setAttribute("x1", x1);
    aline2.setAttribute("y1", y1);
    aline2.setAttribute("x2", x2);
    aline2.setAttribute("y2", y2);
    aline2.setAttribute("style", "stroke:#000;stroke-width:2");
    aline2.setAttribute("marker-end", `url(#${idname})`) //注意修改了这里
    svg.appendChild(aline2);
}

idname = create_marker(svg,"my_arrow")
create_arrow(rsx(0),rsy(0),rsx(-7),rsy(1),svg,idname)
create_arrow(rsx(0),rsy(0),rsx(-2),rsy(-3),svg,idname)
create_arrow(rsx(0),rsy(0),rsx(0),rsy(3),svg,idname)
create_arrow(rsx(0),rsy(0),rsx(2),rsy(-2),svg,idname)
create_arrow(rsx(0),rsy(0),rsx(4),rsy(0),svg,idname)
```
所以图像是这样的。
![want](https://github.com/leperca/wpmc/blob/master/f1/s11.svg)
当然这里有点小瑕疵，最后再来修改。

回头翻了翻SVG Essential的书籍，有一些事情的确有更好的处理方式。有些探索似乎没有必要，自己的方法也显得很啰嗦。但是回头一想，这应该就是学习的过程，摸索一些自己处理的方式，尔后再去学习就知道哪些东西是自己可以学习到的。而且本文的目的就是记录一些常见的弯路。很多材料都是精致整理过的东西，让人觉得一下子就应该这么做，反而阻碍了人们的前行。就跟研究一样，很多时候就是摸索着前进，摸到什么就写一份报告，迂回的道路才是一条常规的路线。不奢求一次到位。

回来，我们剩下要做的事情就是写上文字了和背景网格的事情。

着手写文字。
再次同样的方式：
```js
let t1 = document.createElementNS(svgns, "text");
t1.setAttribute("x",  rsx(4))
t1.setAttribute("y",  rsy(3))
t1.setAttribute("dx", 10)
t1.setAttribute("dy", 10)
t1.innerHTML = "4+3i";
svg.appendChild(t1);
```
结果如图：
![want](https://github.com/leperca/wpmc/blob/master/f1/s12.svg)
整合为函数：
```js
function create_text(text,x,y,svg,dx=10,dy=10) {
    let t1 = document.createElementNS(svgns, "text");
    t1.setAttribute("x", x)
    t1.setAttribute("y", y)
    t1.setAttribute("dx", dx)
    t1.setAttribute("dy", dy)
    t1.innerHTML = text;
    svg.appendChild(t1);
}
create_text("3i",rsx(0),rsy(3),svg)
create_text("4",rsx(4),rsy(0),svg,0,20)
create_text("-2-3i",rsx(-2),rsy(-3),svg)
create_text("2-2i",rsx(2),rsy(-2),svg)
create_text("0",rsx(0),rsy(0),svg,-10,-10)
create_text("-7+i",rsx(-7),rsy(1),svg,-10,-10)
```
结果如图：
![want](https://github.com/leperca/wpmc/blob/master/f1/s13.svg)

最后把虚线画上，经过查询，看到这样一个例子：
```html
<line stroke-dasharray="5, 5" x1="10" y1="10" x2="190" y2="10" />
```
那么我们就先画一条虚线：
```js
let dashed1 = document.createElementNS(svgns, "line");
dashed1.setAttribute("x1", rsx(-9))
dashed1.setAttribute("y1", rsy(1))
dashed1.setAttribute("x2", rsx(9))
dashed1.setAttribute("y2", rsy(1))
dashed1.setAttribute("stroke-dasharray","5,5")
dashed1.setAttribute("stroke", 'black')
svg.appendChild(dashed1)
```
结果如图：
![want](https://github.com/leperca/wpmc/blob/master/f1/s14.svg)

同样的方式，整合成函数再画剩余的线。
```js
function create_dash(x1,y1,x2,y2,svg) {
    let dashed1 = document.createElementNS(svgns, "line");
    dashed1.setAttribute("x1", x1)
    dashed1.setAttribute("y1", y1)
    dashed1.setAttribute("x2", x2)
    dashed1.setAttribute("y2", y2)
    dashed1.setAttribute("stroke-dasharray", "5,5")
    dashed1.setAttribute("stroke", 'rgb(155,155,155)')
    svg.appendChild(dashed1)
}
//horizontal
create_dash(rsx(-9), rsy(2), rsx(9), rsy(2),svg)
create_dash(rsx(-9), rsy(3), rsx(9), rsy(3),svg)
create_dash(rsx(-9), rsy(-1), rsx(9), rsy(-1),svg)
create_dash(rsx(-9), rsy(-2), rsx(9), rsy(-2),svg)
create_dash(rsx(-9), rsy(-3), rsx(9), rsy(-3),svg)
create_dash(rsx(-9), rsy(-4), rsx(9), rsy(-4),svg)
//vertical
create_dash(rsx(-1), rsy(-4), rsx(-1), rsy(4),svg)
create_dash(rsx(-2), rsy(-4), rsx(-2), rsy(4),svg)
// a new function help to generate dashed lines
function create_vertDash(x){
    create_dash(rsx(x), rsy(-4), rsx(x), rsy(4),svg)
}
for(let i=0;i<18;i++){
    create_vertDash(i-9)
}
```
以上发现了一定的模式之后，整合成函数帮助画线，当然一开始就想到应该用函数也可以，这里就是方便理解，先用傻傻的方式先做，再逐步变聪明。哈哈哈。上面的虚线有叠加，但不要紧，最后的结果如图（我们稍微修改了虚线的颜色，可以对比黑线和暗线的差别）
![want](https://github.com/leperca/wpmc/blob/master/f1/s15.svg)

再补上圆形的矩阵外边框：
```js
let rect = document.createElementNS(svgns, "rect");
rect.setAttribute("width", 800)
rect.setAttribute("height", 400)
rect.setAttribute("rx", 40)
rect.setAttribute("ry", 40)
rect.setAttribute("fill", 'none')
rect.setAttribute("stroke", 'blue')
svg.appendChild(rect)
```
如图：
![want](https://github.com/leperca/wpmc/blob/master/f1/s15.svg)

最最后，我们好像忘记写复平面这几个字以及双写的C。那么写上这个。
```js
let t2 = document.createElementNS(svgns, "text");
t2.setAttribute("x", rsx(-7))
t2.setAttribute("y", rsy(-1))
t2.setAttribute("dx", 10)
t2.setAttribute("dy", 10)
t2.setAttribute("font-size", 22)
t2.setAttribute("font-family","'Times New Roman'")
t2.setAttribute("fill", "black")
t2.innerHTML = "The Complex Plane";
svg.appendChild(t2);

let t3 = document.createElementNS(svgns, "text");
t3.setAttribute("x", rsx(-5.5))
t3.setAttribute("y", rsy(-2))
t3.setAttribute("dx", 10)
t3.setAttribute("dy", 10)
t3.setAttribute("font-size", 30)
t3.setAttribute("font-family","Arial Unicode MS")
t3.setAttribute("fill", "black")
t3.innerHTML = " &#x2102 ";
svg.appendChild(t3);

let t4 = document.createElementNS(svgns, "text");
t4.setAttribute("x", rsx(6))
t4.setAttribute("y", rsy(-3))
t4.setAttribute("dx", 10)
t4.setAttribute("dy", 10)
t4.setAttribute("font-size", 25)
t4.setAttribute("fill", "black")
t4.innerHTML = "复平面";
svg.appendChild(t4);

svg.setAttribute("style","")
```
其中我们把边框清掉了。另外，发现有些边框的线条多画了，循环那里需要再稍微改一改。
留点小瑕疵吧，当然我是故意留的，一是因为懒，二是，总有些东西我们会不顺心，但也需要接受。
我们下次翻过头来再回顾重新修改这些比较脏的代码。
这里就这样结束，如图
![want](https://github.com/leperca/wpmc/blob/master/f1/s16.svg)

END.
