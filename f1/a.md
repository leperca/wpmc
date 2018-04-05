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

接下来既然是画图，就有两种选择：一种是<svg>，一种是<canvas>。就我所知，前者矢量后者像素图。然后矢量有矢量的好处，像素有像素的好处。具体的好处百度一下就有了...
我们先选择 svg。 
然后第一件事情就是把svg放入到html中，注意这里，将一切都放在js中进行。代码如下：
```js
const svgns = "http://www.w3.org/2000/svg";
let svg = document.createElementNS(svgns, "svg");
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
