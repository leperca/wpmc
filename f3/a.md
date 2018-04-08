下面开始添加交互式的功能。
我想这个功能才是所有做这件事情的核心所在。
以needham的第二幅图为例，
![want](https://github.com/leperca/wpmc/blob/master/f3/want.jpg)

## 添加交互之前的工作

就着上次整理好的函数，我们把它们写在script标签里面，这样方便待会后面的修整，如果需要的话。

我们开始画，但是由于没有确切的坐标系。
这里我们自己给定义一个坐标系，就假设这是x轴为[-1,5]，y轴为[-1,4]

另外我们还找到一个错误的地方。
少了两个let，导致后面使用了参数就出错了。
```js
        // 把[m,n]中的点变成[p,q]中的点。
        function linear([m, n], [p, q]) {
            return function (x) {
                let k = (p - q) / (m - n); //查到一个错误 少了个let
                let b = p - k * m; // 这里也少了个 let
                return k * x + b
            }
        }

///
```

回到正题。画图。
我们另外写了一个构造函数 Complex；
它包含有draw，moveTo功能。
```js
///
        //x轴为[-0.5,5.5]，y轴为[-0.5,3.5]
        const svgns = "http://www.w3.org/2000/svg"
        let svg_width = 500, svg_height = 400;
        let s1 = create_svg(svg_width, svg_height)
        insert_arrowdefs(s1);
        let rsx = linear([-0.5, 5.5], [0, svg_width])
        let rsy = linear([-0.5, 3.5], [svg_height, 0])

        let isx = linear([0, svg_width], [-0.5, 5.5])
        let isy = linear([svg_height, 0], [-0.5, 3.5])

        // 画原点
        s1.appendChild(create_point(rsx(0), rsy(0), 3, "black"))

        // complex number
        function Complex(x, y,_svg_="none") {
            this.real = x;
            this.img = y;
            this.point = create_point(rsx(x),rsy(y));
            this.arrow = create_arrow(rsx(0),rsy(0),rsx(x),rsy(y));
            this.svg = _svg_;
            // 复数的加法
            this.plus = function (comp) {
                let [real, img] = comp.data // comp ==> 一个复数
                return new Complex(this.real + real, this.img + this.img)
            }
            // 画上
            this.draw = function (svg=this.svg) {
                svg.appendChild(this.arrow)
                svg.appendChild(this.point)
            }

            this.moveTo = function(x,y,svg=this.svg){
                this.point.setAttribute("cx",rsx(x))
                this.point.setAttribute("cy",rsy(y))
                this.arrow.setAttribute("x2",rsx(x))
                this.arrow.setAttribute("y2",rsy(y))
            }
        }

        a = new Complex(2,0.5,s1)
        b = new Complex(4,3,s1)

        a.draw();
        b.draw();
        // 见图s1.svg
        a.moveTo(3,3)
        b.moveTo(3,1)
        // 见图s2.svg
```
两个图分别是：
![want](https://github.com/leperca/wpmc/blob/master/f3/s1.svg)

![want](https://github.com/leperca/wpmc/blob/master/f3/s2.svg)

好了，下面来构造拖动。
这个函数看上去有点复杂。其实就是需要有个拖动功能，fn是回调函数。
当然也可以使用其他库。这里就先这样放这里，以后再整理干净，能用就行。
## 添加拖拽功能能
```js
this.drag = function (fn = () => { }) {
    let comp = this;
    comp.point.setAttribute("fill", "white")
    comp.point.setAttribute("stroke", "green")
    comp.point.setAttribute("r", "5")
    comp.point.addEventListener("mousedown", moveStart)
    function moveStart(event) {
        console.log("dragStart")
        let tx = parseInt(event.target.getAttribute("cx")),
            ty = parseInt(event.target.getAttribute("cy"))
        let x0 = parseInt(event.clientX), y0 = parseInt(event.clientY)
        document.addEventListener("mousemove", moving)
        function moving(e) {
            let sx = isx(e.clientX - x0 + tx)
            let sy = isy(e.clientY - y0 + ty)
            comp.moveTo(sx, sy)
            fn();
            document.addEventListener("mouseup", () => document.removeEventListener("mousemove", moving))
        }
    }
}
```
```js
a = new Complex(1, 0.5, s1)
b = new Complex(1, 2, s1)
c = a.plus(b)
a.draw(); b.draw(); c.draw();

a.drag(dragingfn);
b.drag(dragingfn);

function dragingfn() {
    let newC = a.plus(b);
    c.moveTo(newC.real, newC.img)
}


function download(filename, text) {
    var pom = document.createElement('a');
    pom.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(text));
    pom.setAttribute('download', filename);
    var event = document.createEvent('MouseEvents');
    event.initEvent('click', true, true);
    pom.dispatchEvent(event);
}

// download("a.svg",s1.outerHTML)
// 帮助下载svg文件
```

![want](https://github.com/leperca/wpmc/blob/master/f3/result.gif)

## 具体结果见网页 index.html

## https://leperca.github.io/wpmc/f3/index.html
以上我们实现了两个复数加法的可视化。

