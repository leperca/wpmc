将f1的内容重新拿出来整理一下。

当然这里也只是适度整理，我觉得之后通过一定的学习了解和摸索之后，应该会有不同的看法。暂时阶段可以做如下的整理，不奢求一次到位，以不过多的考虑今后（例如面向对象的方式），只做眼前需要的设计。

## 定义一些常量
```js
const svgns = "http://www.w3.org/2000/svg";
const version = "f2"
// 以后将会涉及到的一些量
```

## 只要以下
```js
//svg画板
function create_svg(w = 500, h = 500) {
    let svg = document.createElementNS(svgns, "svg");
    svg.setAttribute("xmlns", svgns);
    svg.setAttribute("width", w);
    svg.setAttribute("height", h);
    svg.setAttribute("style", "border:1px solid green");
    document.body.appendChild(svg)
    return svg;
}

//画线
function create_line(x1, y1, x2, y2, dasharray=0) {
    let line = document.createElementNS(svgns, "line");
    line.setAttribute("x1", x1);
    line.setAttribute("y1", y1);
    line.setAttribute("x2", x2);
    line.setAttribute("y2", y2);
    line.setAttribute("style", "stroke:black;stroke-width:1");
    line.setAttribute("stroke-dasharray",dasharray);
    return line
}

//画点
function create_point(cx, cy, r = 3, color = "yellow", sw = 2) {
    let o = document.createElementNS(svgns, "circle");
    //注意需要改成“circle”
    o.setAttribute("cx", cx);
    o.setAttribute("cy", cy);
    o.setAttribute("r", r);
    o.setAttribute("stroke", "black");
    o.setAttribute("fill", color);
    o.setAttribute("stroke-width", sw);
    return o
}

//插入默认箭头定义
function insert_arrowdefs(svg, idname) {
    // 定义标签
    let my_defs = document.createElementNS(svgns, "defs");
    let my_marker = document.createElementNS(svgns, "marker");
    my_marker.setAttribute("id", idname); //注意设置id 
    my_marker.setAttribute("viewBox", "0 0 10 10");
    my_marker.setAttribute("refX", "8");
    my_marker.setAttribute("refY", "5");
    my_marker.setAttribute("markerWidth", "6")
    my_marker.setAttribute("markerHeight", "6")
    my_marker.setAttribute("orient", "auto");
    // arrow
    let arrow = document.createElementNS(svgns, "path");
    arrow.setAttribute("d", "M 0 0 L 10 5 L 0 10 L 5 5 Z");
    arrow.setAttribute("fill", "#000000");
    // 标签添加到defs图上
    my_marker.appendChild(arrow);
    my_defs.appendChild(my_marker);
    svg.appendChild(my_defs)
}

// 画箭头
function create_arrow(x1, y1, x2, y2, idname) {
    let line = create_line(x1, y1, x2, y2)
    line.setAttribute("style", "stroke:#000;stroke-width:2");
    line.setAttribute("marker-end", `url(#${idname})`)
    return line
}

function create_text(text, x, y, dx = 10, dy = 10) {
    let t = document.createElementNS(svgns, "text");
    t.setAttribute("x", x)
    t.setAttribute("y", y)
    t.setAttribute("dx", dx)
    t.setAttribute("dy", dy)
    t.innerHTML = text;
    return t
}
```

## 平移加变换
例如将在[m,n]中的点线性映射到[p,q]中的点。
如果没法快速的想象出来，直接考虑以下两个求解方程组：
```
p = k m + b
q = k n + b
```
于是有以下的生成函数：
```js
// 把[m,n]中的点变成[p,q]中的点。
function linear([m,n],[p,q]){
    return function(x){
        k = (p - q)/(m-n);
        b = p-k*m;
        return k*x+b
    }
}
```
例如原来的rsx = linear([-9,9],[0,800]),
而原来的rsy = linear([4,-4],[0,400])。
这里稍微注意下y需要颠倒。

## 使用
```js
svg = create_svg(800, 400);
insert_arrowdefs(svg, "default")
rsx = linear([-9, 9], [0, 800])
rsy = linear([4, -4], [0, 400])

//画x=0 y=0
svg.appendChild(create_line(rsx(-9), rsy(0), rsx(9),rsy(0)))
svg.appendChild(create_line(rsx(0), rsy(-4), rsx(0),rsy(4)))

//画箭头
l1 = create_arrow(rsx(0), rsy(0), rsx(4), rsy(0));
a1 = create_point(rsx(4), rsy(0), 4,"white");
svg.appendChild(a1)
svg.appendChild(l1)

//发现模式，组合一下
function draw(x, y) {
    let l1 = create_arrow(rsx(0), rsy(0), rsx(x), rsy(y));
    let a1 = create_point(rsx(x), rsy(y), 4,"white");
    svg.append(a1, l1)
}
draw(-7, 1);draw(0, 3);draw(-2, -3);draw(2, -2);draw(4, 3)

svg.appendChild(create_point(rsx(0), rsy(0), 5, "white"))
svg.appendChild(create_text("The Complex Plane",rsx(-6), rsy(-1)))

// 画网格，可以按照需求画。
function drawHdash(x){
    svg.appendChild(create_line(rsx(-9), rsy(x), rsx(9),rsy(x),3))
}
function drawVdash(y){
    svg.appendChild(create_line(rsx(y), rsy(-4), rsx(y),rsy(4),3))
}
drawHdash(3);drawHdash(-3);drawHdash(-2);drawHdash(1);
drawVdash(-7);drawVdash(4);drawVdash(2)
```
结果见图。
