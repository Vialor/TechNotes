Reference: [https://juejin.cn/post/6844904113352736776](https://juejin.cn/post/6844904113352736776)
priority: 4>3>2>1
1. 默认绑定规则：this===window; in strict mode, this===undefined
2. 隐式绑定规则：this是执行时产生的，只和调用它所在的函数的对象有关。但父函数还是可以通过可指定子函数的指向(thisArg)。
```JavaScript
const obj = {
    a: function () {
        console.log("a: ", this)
        function b () {
            console.log("b: ", this)
        }
        b()
    }
}
obj.a()
// a: obj（隐式）
// b: window（默认）
```
回调函数隐式绑定this丢失：obj.foo作为参数传递时被当做是匿名函数，匿名函数无this绑定
```JavaScript
const obj = {
    foo: function () {
        console.log(this)
    }
}
setTimeout(obj.foo, 0) // => window, this丢失
setTimeout(function () {
    obj.foo() // => obj
}, 10)
```
1. 显示绑定规则：
```HTML
f.call(thisObj, arg1, arg2, arg3) // run the function with thisObj
f.apply(thisObj, argList) // run the function with thisObj
f.bind(thisObj) // return a this-binded function
// 连续bind只有第一次有效
```
1. new 绑定规则：
`new foo()` foo中的this是返回的对象