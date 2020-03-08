## 实现一个 new
### 参考文献
+ [实现 new](https://zhuanlan.zhihu.com/p/84605717)
### 定义
1. 创建一个新的空的对象
2. 将构造函数的作用域赋给新对象(因而 this 就指向了这个新对象)
   1. 即将新对象的原型链链接到构造函数的原型对象上
3. 执行构造函数中的代码(为这个新对象添加属性)
4. 如果构造函数有返回值，则返回；否则，返回新对象。
#### code
```javascript
function myNew() {
    var constr = Array.prototype.shift.call(arguments);
    var obj = Object.create(constr.prototype);
    var res = constr.apply(obj,arguments);
    return res instanceof Object ? res : obj;
}
```
+ 或者这样写
```javascript
function myNew(fun,...args) {
    if(typeof fun !== 'function'){
        console.log('参数必须是一个函数');
    }
    let obj = Object.create(fun.prototype);
    let res = fun.call(obj,...args);
    if(res !== null && (typeof res === 'object' || typeof res === 'function')){
        return res;
    }
    return obj;
}
```