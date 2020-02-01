# js基本数据类型
#### ES5
+ 简单数据类型
  + String、Boolean
  + Null、Undefined
    + Undefined类型表示未定义，它只有一个值为undefined
    + undefined是一个变量，并非关键字，在局部作用域下，会被篡改
      + 因此判断是否为undefined，可以用
        + x !== void 0
        + void 0 返回undefined
    + Null类型只有一个值，为null
    + null是一个关键字
  + Number
    + 有 18437736874454810627(即 2^64-2^53+3) 个值
    + 额外场景
      + NaN，占用了 9007199254740990，这原本是符合 IEEE 规则的数字；
      + Infinity，无穷大；
      + -Infinity，负无穷大。
    + 精度问题
      + 根据双精度浮点数的定义，Number 类型中有效的整数范围是 -0x1fffffffffffff 至 0x1fffffffffffff，所以 Number 无法精确表示此范围外的整数。
      + console.log( 0.1 + 0.2 == 0.3); // false
      + 改正
        + console.log( Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON); // true
+ 复杂数据类型
  + Object
+ 其它规范类型
  + List 和 Record： 用于描述函数传参过程。
  + Set：主要用于解释字符集等。
  + Completion Record：用于描述异常、跳出等语句执行过程。
  + Reference：用于描述对象属性访问、delete 等。
  + Property Descriptor：用于描述对象的属性。
  + Lexical Environment 和 Environment Record：用于描述变量和作用域。
  + Data Block：用于描述二进制数据。
#### ES6
+ 新增
  + [Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)