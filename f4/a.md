开始画needham的第二幅图b，a b 两者拼合起来应该为：
![want](https://github.com/leperca/wpmc/blob/master/f4/want.png)

我们直接使用上一次的东西。
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
结果为：
![want](https://github.com/leperca/wpmc/blob/master/f4/result.gif)

## 见网页index.html
## https://leperca.github.io/wpmc/f4/index.html
