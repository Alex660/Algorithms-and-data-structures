## 实现 call、apply、bind
#### 实现 call
```javascript
Function.prototype.call = function (context) {
    // 如果对象传入是空或者null的时候指向window
    const cxt = context || window;
    // 以对象的方式调用的方式绑定this
    cxt.fun = this;
    // 获取实参
    const args = Array.from(arguments).slice(1);
    // 或 [...arguments].splice(1) 或 遍历+eval
    const res = arguments.length > 1 ? cxt.fun(...args) : cxt.fun();
    // 删除方法，不然会对传入的对象造成污染(添加该方法)
    delete cxt.fun;
    // 返回结果
    return res;
}
```
#### 实现 apply
```javascript
Function.prototype.apply = function (context) {
    const cxt = context || window;
    cxt.fun = this;
    const res = arguments[1] ? cxt.fun(...arguments[1]) : cxt.fun();
    delete cxt.fun;
    return res;
}
```
#### 实现 bind
```javascript
if(!Function.prototype.bind)(function(){
    Function.prototype.bind = () => {
        let fun = this;
        let cxt = arguments[0];
        let bindArgs = [].slice.call(arguments,1);
        if(typeof fun !== 'function'){
            throw new TypeError('Fucntion.prototype.bind -'+'what is trying to be bound is not callable');
        }
        // return function (){
        //     return fun.apply(cxt,bindArgs.concat([].prototype.slice.call(arguments)))
        // }
        return function (...callArgs) {
            let allArgs = bindArgs.concat(callArgs);
            if(this instanceof fun){
                // 如果是 通过 new 调用的，而 new 的优先级高于 bind
                return new fun(...allArgs);
            }
            return fun.call(cxt,...allArgs);
        }
    }
})();
``` 
+ 或者这样写
```javascript
Function.prototype.bind = function () {
    let self = this;
    let cxt = arguments[0];
    let args = [].slice.call(arguments,1);
    return function (){
        let allArgs = args.concat([].slice.call(arguments));
        if(this instanceof self){
            // 如果是 通过 new 调用的，而 new 的优先级高于 bind
            return new self(...allArgs);
        }
        return self.apply(cxt,allArgs);
    } 
}
```
+ 或者这样写
```javascript
Function.prototype.bind = function () {
    let self = this;
    let cxt = arguments[0] || window;
    let args = [...arguments].splice(1);
    return function () {
        let allArgs = [...args,...arguments];
        if(this instanceof self){
            // 如果是 通过 new 调用的，而 new 的优先级高于 bind
            return new self(...allArgs);
        }
        self.apply(cxt,allArgs);
    }
}
```