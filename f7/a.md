现在我们来画needham
图1-5，结果应该这样
![want](https://github.com/leperca/wpmc/blob/master/f7/want.png)

复用上次的代码，并增加polygon 函数
```js
function create_polygon(xs) {
    // xs是数组，例如： [[10,20],[22,2],[23,32]]
    let ts = "";
    for (let i = 0; i < xs.length; i++) {
        ts += xs[i].join(',') + ' '
    }
    let poly = document.createElementNS(svgns, "polygon")
    poly.setAttribute("points", ts)
    poly.setAttribute("style", "fill:none;stroke:black;stroke-width=2")
    return poly;
}
```

画图的代码重新组织了一下逻辑。
```js
let xmin = -4, xmax = 6;
let ymin = -4, ymax = 3;

let svg_width = 500, svg_height = 250;
let mysvg = create_svg(svg_width, svg_height)
insert_arrowdefs(mysvg);

// scale function
let rsx = linear([xmin, xmax], [0, svg_width])
let rsy = linear([ymin, ymax], [svg_height, 0])
// inverse scale function
let isx = linear([0, svg_width], [-5, 5])
let isy = linear([svg_height, 0], [-5, 5])

// opoint
let svg_opoint = create_point(rsx(0), rsy(0), 3, "white")
svg_opoint.style["stroke-width"] = "1"

// xline
let xline = create_arrow(rsx(xmin + 0.2), rsy(0), rsx(xmax - 0.4), rsy(0))
xline.style["stroke-width"] = "1.5"

// yline
let yline = create_arrow(rsx(0), rsy(ymin + 0.1), rsx(0), rsy(ymax - 0.1))
yline.style["stroke-width"] = "1.5"

// parallelogram point
let opoint = [0, 0]
let para_AC = [3, 2], para_AB = [2, -0.5]
let plus2d = ([x, y], [m, n]) => [x + m, n + y]
let para_ABC = plus2d(para_AC, para_AB)
let para = [opoint, para_AC, para_ABC, para_AB]
// for svg
para = para.map(([x, y]) => [rsx(x), rsy(y)])
svg_para = create_polygon(para)
svg_para.style.fill = "#000000"
svg_para.style["fill-opacity"]="0.5"

// circle
let cx = rsx(-1.5), cy=rsy(1.5), radius = 30;
let svg_circle = create_point(cx,cy,radius)
svg_circle.style.fill="#999999"
svg_circle.style.strokeWidth="1"

// text 
let svg_text = create_text("AC",rsx(para_AC[0]),rsy(para_AC[1]),0,-5)

// append all the elements into the svg
let svg_elements = [xline, yline, svg_para, svg_opoint,svg_circle,svg_text]
svg_elements.map(x => mysvg.appendChild(x))
```
见图
![want](https://github.com/leperca/wpmc/blob/master/f7/result.svg)
## 最终结果见网页index.html
## https://leperca.github.io/wpmc/f7/index.html
