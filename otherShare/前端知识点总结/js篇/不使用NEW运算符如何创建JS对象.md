## 不使用NEW运算符如何创建JS对象
#### 解法
+ 利用字面量
  ```javascript
  let a = [],b = {},c = /abc/g;
  ``` 
+ 利用 dom api
  ```javascript 
  let d = document.createElement('p');
  ```
+ 利用JavaScript内置对象api
  ```javascript
  let e = Object.create(null);
  let f = Object.assign({x:2},{y:3});
  let g = JSON.parse('{}');
  ``` 
+ 利用装箱转换
  ```javascript
  let h = Object(undefined);
  let i = Object(null);
  let j = Object(1);
  let k = Object('a');
  let l = Object(true);
  let m = (function() {return this}).call(1)
  ``` 
+ 利用 ES6构造器
  ```javascript
  class myObj{
      constructor(a) {this.a = a;}
  }
  ``` 