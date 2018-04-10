现在我们要画第三幅图。
![want](https://github.com/leperca/wpmc/blob/master/f5/want_1_3.png)

同样复用上次的代码。

这里需要加上画函数图像的部分了。
就是给出一个函数表达式，画出它的图像来。
我们添加画线的函数。
```js
function create_poly(xs) {
    // xs是数组，例如： [[10,20],[22,2],[23,32]]
    let ts = "";
    for (let i = 0; i < xs.length; i++) {
        ts += xs[i].join(',') + ' '
    }
    let poly = document.createElementNS(svgns, "polyline")
    poly.setAttribute("points", ts)
    console.log(ts)
    poly.setAttribute("style", "fill:none;stroke:black;stroke-width=2")
    return poly;
}
```

新增加辅助函数,为了调用方便
```js
// 均匀产生start到end之间n个数字
function linspace(start, end, n) {
    let s = [];
    let len = (end - start) / (n - 1);
    for (let k = 0; k < n; k++) {
        s.push(start + k * len)
    }
    return s
}
// 将数组xs和数组ys拆开打包成点列 [ ..., [x,y], ... ]
// 例如[[1,2,3],[4,5,6]] => [[1,4],[2,5],[3,6]]
function zip(xs, ys) {
    let s = [];
    for (let i = 0; i < xs.length; i++) {
        s.push([xs[i], ys[i]])
    }
    return s
}
let log = console.log; 
// 为了调试方便。
```

以及画图：
```js
 // draw X line
let x_line = create_line(rsx(-5),rsy(0),rsx(5),rsy(0))
x_line.style["stroke"] = "#bbbbbb"
s1.appendChild(x_line.cloneNode())
s2.appendChild(x_line.cloneNode())

// draw Y line
let y_line = create_line(rsx(0),rsy(-5),rsx(0),rsy(5))
y_line.style["stroke"] = "#bbbbbb"
s1.appendChild(y_line)
s2.appendChild(y_line.cloneNode())


// figure 1
// function x**2
let x1 = linspace(-5, 5, 200)
let y1 = x1.map(x => x ** 2)
let z1 = zip(x1.map(rsx), y1.map(rsy))
let p1 = create_poly(z1)
s1.appendChild(p1)

// function m*x+2
let y2 = x1.map(x => 1/2*x + 0.6)
let z2 = zip(x1.map(rsx), y2.map(rsy))
let p2 = create_poly(z2)
s1.appendChild(p2)

// figure 2
// function x**3

let y3 = x1.map(x => x ** 3)
let z3 = zip(x1.map(rsx), y3.map(rsy))
let p3 = create_poly(z3)
s2.appendChild(p3)

let y4 = x1.map(x => -2 * x + 0.6 )
let z4 = zip(x1.map(rsx), y4.map(rsy))
let p4 = create_poly(z4)
s2.appendChild(p4)
```
结果
## 见网页index.html
## https://leperca.github.io/wpmc/f5/index.html
