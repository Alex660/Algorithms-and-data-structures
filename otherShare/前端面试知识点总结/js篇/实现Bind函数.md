## 实现Bind函数
#### 解法
```javascript
if(!Function.prototype.bind)(function(){
    Function.prototype.bind = () => {
        let that = this;
        let arg = arguments[0];
        let args = [].prototype.slice.call(arguments,1);
        if(typeof that !== 'function'){
            throw new TypeError('Fucntion.prototype.bind -'+'what is trying to be bound is not callable');
        }
        return function (){
            return that.apply(arg,args.concat([].prototype.slice.call(arguments)))
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
        self.apply(cxt,args.concat([].slice.call(arguments)));
    } 
}
```