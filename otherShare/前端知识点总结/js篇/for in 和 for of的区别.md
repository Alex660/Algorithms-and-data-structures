## for in 和 for of的区别
### 参考文献
+ [MDN - for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)
+ [MDN - for...in](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)
+ [阮一峰 - Iterator 和 for...of 循环](http://es6.ruanyifeng.com/#docs/iterator#%E8%B0%83%E7%94%A8-Iterator-%E6%8E%A5%E5%8F%A3%E7%9A%84%E5%9C%BA%E5%90%88)
### for...of
+ 定义
  + ES6
  + 在可迭代对象上创建一个迭代循环
    + Array
    + Map
    + Set
    + String
    + TypedArray
    + arguments 对象
    + ...
### for...in
+ 定义
  + ES5
  + 以任意顺序遍历一个对象的除 Symbol 以外的可枚举属性。
+ 特点
  + index索引为字符串型数字，不能直接进行几何运算
  + 遍历顺序有可能不是按照实际数组的内部顺序
  + 会遍历数组所有的可枚举属性，包括原型
### 区别
+ for...of
  + 遍历可迭代对象定义要迭代的数据
  + 多用于遍历数组，循环出的为 value
  + 不能循环普通的对象，因为没有迭代器对象
    + 需要Object.keys()搭配，加迭代器对象也无用
        ```javascript
        var student={
            name:'张三',
            age:22,
            locate:{
                city:'深圳'
            }
        }
        for(var key of Object.keys(student)){
            console.log(key+": "+student[key]);
        }
        ``` 
      + 但是对类数组对象加迭代器有用
        + 栗子
          ```javascript
          let iterable = {
            0: 'a',
            1: 'b',
            2: 'c',
            length: 3,
            [Symbol.iterator]: Array.prototype[Symbol.iterator]
            };
            for (let item of iterable) {
            console.log(item); // 'a', 'b', 'c'
            }
          ``` 
+ for...in
  + 以任意顺序迭代对象的可枚举属性
  + 多用于遍历对象属性，循环出的是 key