现在我们给f5中的图像加入一点交互。

同样复用上次的代码。

修改画图函数为。xs为数列点，fn是xs的函数点。
```js
function draw_function(xs,fn){
        let ys = xs.map(x => fn(x))
        let zipped = zip(xs.map(rsx), ys.map(rsy))
        let poly = create_poly(zipped)
        return poly
    }
    pk2 = draw_function([1,2,3],(x)=>x*3)
    s1.appendChild(pk2)
```

加入了滑动的控件
```js
function create_slider(name, xmin, xmax, callback=()=>{console.log("required fn")}, init = xmin, xstep = (xmax - xmin) / 10) {
    let div = document.createElement("div")
    let text1 = document.createElement("label")
    let text2 = document.createElement("label")
    let shown_num = document.createElement("label")
    let slider = document.createElement("input");
    //segment1
    text1.innerText=" { "+ xmin 
    div.appendChild(text1) 
    //slider
    slider.type = "range";
    slider.min = xmin;
    slider.max = xmax;
    slider.step = xstep;
    slider.value = init;
    div.appendChild(slider)
    slider.oninput = function(e){
        let text = document.getElementById(shown_num.id)
        text.innerHTML = slider.value
        div.value = parseFloat(slider.value)
        callback();
    }
    //text2
    text2.innerText=" " + xmax + " } step: " + xstep+ "==> " + name + " = "
    div.appendChild(text2)
    shown_num.innerText = " " + init + " "
    shown_num.id = name + "_idvalue"
    div.appendChild(shown_num)
    div.value = parseFloat(slider.value)
    return div;
}
```

画图的代码
```js
v_init = 0;
v_step = .1;

function px(p, q) {
    return draw_function(linspace(-5, 5, 1000),
        (x) => 3 * Math.tanh(x) + p * Math.sin(10 * x) * (1 + x * q) / (q + 1)

    )
}

let old = px(v_init, v_init)
s1.appendChild(old)

// let slider1 = create_slider("p", -5, 5)
let slider1 = create_slider("p", -5, 5, fn, v_init, v_step)
let slider2 = create_slider("q", -1, 1, fn, v_init, v_step)
document.body.appendChild(slider1)
document.body.appendChild(slider2)

function fn() {
    s1.removeChild(old)
    log(slider1.value)
    log(slider2.value)
    old = px(slider1.value, slider2.value)
    s1.appendChild(old)
}
```
见图
![want](https://github.com/leperca/wpmc/blob/master/f6/result.gif)
## 最终结果见网页index.html
## https://leperca.github.io/wpmc/f6/index.html
