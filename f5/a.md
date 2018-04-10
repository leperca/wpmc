现在我们要画第三幅图。
![want](https://github.com/leperca/wpmc/blob/master/f4/want_1_3.png)

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

新增加了复数的乘法和计算长度和幅角公式，
当然，这里的幅角公式应该调整一下，但是这里先不改了。下次再说
```js
this.mult = function (comp) {
    return new Complex(this.real * comp.real - this.img * comp.img,
        this.real * comp.img + this.img * comp.real, this.svg)
}

this.calc_len = function(){
    return Math.sqrt(this.real**2+this.img**2) 
}
this.calc_arg = function(){
    return Math.atan(this.img/this.real) 
}
```
结果
## 见网页index.html
## https://leperca.github.io/wpmc/f5/index.html
