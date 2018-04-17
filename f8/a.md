这次我们要画大括号.
有几种方式，
一种是利用原来的字符“{”， 然后再加平移转动。
第二种是利用path，把路径画出来，再加平移转动。

我们这里就用第二种，先画一半。
就是先画个一半的大括号,然后把另外一半画上。
```js
function create_curly1([sx,sy],[ex,ey]){
    let svg_curl = document.createElementNS(svgns,"path")
    // the first half
    let lenx = ex-sx,
        leny = ey-ey;
    let cp1x = sx+lenx/3,
        cp1y = ey,
        cp2x = sx+lenx/3*2,
        cp2y = sy;
    let te = `M${sx} ${sy} C${cp1x} ${cp1y} ${cp2x} ${cp2y} ${ex} ${ey}`
    // the second half
    te+= `C${cp2x+lenx/3*2} ${cp2y} ${cp2x+lenx} ${cp1y} ${cp2x+lenx/3*4} ${sy}`
    svg_curl.setAttribute("d",te)
    svg_curl.style.fill="none"
    svg_curl.style.strokeWidth="1"
    svg_curl.style.stroke="black"
    return svg_curl;
}
```
以上这个函数只能在 点[sx,sy], 以及点[ex,ey]上画如下的曲线，
并且只能在一条横线上画线，这个函数写得很丑陋，但是没有关系，
随便用一个就成，然后其他的内容都是根据这个比较丑陋的函数进行变换。

在[startx,starty]到[endx,endy]这条水平线上画线，并且写上文字。
```js
function create_curly2([startx, starty], [endx, endy]) {
    let l = Math.sqrt((endx - startx) ** 2 + (endy - starty) ** 2)
    let ex = startx + l / 2;
    let ey = starty - l / 10;
    let t = document.createElementNS(svgns, "text")
    t.setAttribute("x", ex)
    t.setAttribute("y", ey)
    t.setAttribute("dy", -10)
    t.innerHTML = "default"
    let result = create_curly1([startx, starty], [ex, ey])
    result.bindText = t;
    return result
}
```

以下函数添加了旋转之后，可以使大括号随着直线转动。
```js
function create_curly3(x1, y1, x2, y2) {
    let dx = x2 - x1, dy = y2 - y1;
    let len = Math.sqrt(dx ** 2 + dy ** 2)
    let alpha = 90;
    if (dx !== 0) {
        alpha = Math.atan(dy / dx) * 180 / Math.PI;
    }
    let curl = create_curly2([x1, y1], [x1 + len, y1])
    curl.setAttribute('transform',
        `rotate(${alpha},${x1},${y1})`)
    curl.bindText.setAttribute('transform',
        `rotate(${alpha},${x1},${y1})`)
    return curl
}
```
最后可以把函数全部塞进create_cruly中，暴露一个接口即可。
如图：
![want](https://github.com/leperca/wpmc/blob/master/f7/result.svg)

结果
## 见网页index.html
## https://leperca.github.io/wpmc/f7/index.html



