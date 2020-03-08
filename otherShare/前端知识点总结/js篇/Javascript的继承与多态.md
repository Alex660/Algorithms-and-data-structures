# Javascript的继承与多态
+ ES5
+ ES6
+ 概述
  + JS引入了prototype的概念，可以让我们以另一种方式模仿类，并通过原型链的方式实现了父类子类之间共享属性的继承以及身份确认机制。
### ES6之前的继承
+ 原型赋值方式
  + 直接将父类的一个实例赋给子类的原型
  + code
    ```javascript
    function Person(name) {
        this.name = name;
        this.className = 'person';
    }
    Person.prototype.getClassName = function(){
        console.log(this.className);
    }
    function Man(){
    }
    Man.prototype = new Person();
    var man = new Man();
    man.getClassName() // "person"
    man instanceof Person // true
    ``` 
    + 分析
      + 实现
        + 通过new一个父类的实例，赋值给子类的原型，使得
        + 父类原型中的方法属性及挂在this上的各种方法属性全赋给了子类的原型
      + 问题
        + 子类无法通过父类创建私有属性
        + 此种方法，每次继承来的人名都是同一个
+ 调用构造函数方式
  + 子类的在构造函数里用子类实例的this去调用父类的构造函数，
  + 从而达到继承父类属性的效果
    + call
    + apply
  + code
    ```javascript
    function Person(name){
        this.name=name;
        this.className="person" 
    }
    Person.prototype.getName=function(){
        console.log(this.name)
    }
    function Man(name){
        Person.apply(this,arguments)
    }
    var man1=new Man("Davin");
    var man2=new Man("Jack");
    man1.name // "Davin"
    man2.name // "Jack"
    man1.getName() // 报错
    man1 instanceof Person // false
    ``` 
    + 分析
      + 只能继承父类构造函数中声明的实例属性，并没有继承父类原型的属性和方法
+ 组合继承
  + 原型继承 + 调用构造函数
  + code
    ```javascript
    function Person(name){
        this.name=name||"default name";
        this.className="person" 
    }
    Person.prototype.getName=function(){
        console.log(this.name)
    }
    function Man(name){
        // 第一次调用Person()
        Person.apply(this,arguments)
    }
    //继承原型
    Man.prototype = new Person(); // 第二次调用Person()
    var man1=new Man("Tom");
    man1.name // "Tom"
    man1.getName() //"Tom"
    Man.prototype // Person {name: "default name", className: "person"}
    ``` 
    + 分析
      + 由Man.prototype可知实例中的name属性和原型不同，
      + 每次创建相当于覆盖原型的name属性，原型的依旧在
      + 无论在什么情况下，都会调用两次构造函数：
        + 一次是在创建子类型原型时，
        + 另一次是在子类型构造函数内部。
+ 寄生式继承
  + 和工厂模式类似，创建一个仅用于封装继承过程的函数，
  + 该函数在内部以某种方式来增强对象，最后返回对象。
  + code
    ```javascript
    function Person(name){
        this.name=name; //1
        this.className="person" 
    }
    function createAnother(origin){
        // 浅拷贝原型对象，继承方法
        var clone = Object(origin);
        // 以某种方式来增强这个对象
        clone.sayHi = function(){
            console.log("hi");
        };
        return clone; // 返回这个对象
    }
    var anotherPeople = createAnother(Person);
    anotherPeople.sayHi(); // hi
    ``` 
+ 寄生组合继承（ES5主流继承方式）
  + 去除原型直接赋值方式，改用[Object.creat(obj)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)浅拷贝原型对象
  + code
    ```javascript
    function Person(name){
        this.name=name; //1
        this.className="person" 
    }
    Person.prototype.getName=function(){
        console.log(this.name)
    }
    function Man(name){
        // 继承属性
        Person.apply(this,arguments)
    }
    // 浅拷贝原型对象 - 继承方法
    Man.prototype = Object.create(Person.prototype);
    // 增强对象
    Man.prototype.constructor = Man
    var man1=new Man("Tom");
    man1.name // "Tom"
    man1.getName() // "Tom"
    ``` 
    + 分析
      + 将父类的原型复制给了子类原型
      + constructor属性
        ```javascript
        Person.prototype.constructor
        Person(name){
            this.name=name; //1
            this.className="person" 
        }
        Man.prototype.constructor
        Person(name){
            this.name=name; //1
            this.className="person" 
        }
        man1 instanceof Person // true
        man1 instanceof Man // true
        ``` 
        + constructor是类的构造函数
          + 上述Man的构造函数应该是Man，所以一般需要重置，
            + 也叫增强对象
          + **Man.prototype.constructor = Man**
+ 总结
  + ![2.png](https://pic.leetcode-cn.com/523f0c8896abed9e54534784934e610a0ac4a4c5f4a0a1649e48e76f89a6cfc6-2.png)
  + 组合寄生：一条实现属性继承，一条实现方法的继承
### ES6继承
+ [Class](http://es6.ruanyifeng.com/#docs/class-extends#%E5%8E%9F%E7%94%9F%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E7%9A%84%E7%BB%A7%E6%89%BF)
  + extends
  + super
+ code
  ```javascript
  class A {}
  // B通过extends关键字继承了A类的所有属性和方法
  class B extends A {
    constructor(x,y,color){
        // 调用父类的constructor(x,y)
        // 相当于 A.prototype.constructor.call(this)
        super(x,y); 
        this.color = color
    }
    toString(){
        //调用父类的方法
        return this.color + ' ' + super.toString(); 
    }
  }
  ``` 
  + 分析
    + 一个继承语句同时存在两条继承链：属性继承 + 方法的继承
      ```javascript
      class B extends A{}
      B.__proto__ === A;  //继承属性
      B.prototype.__proto__ === A.prototype;//继承方法
      ``` 
  + 总结
    + ![1.png](https://pic.leetcode-cn.com/baa5c9b2de504917f1170fffe9304b34f12850f38068acdf96524c964264b5bc-1.png) 
#### 其它写法
```javascript
// 基类
function Base() {

}
// 派生类
function Derived() {
  Base.call(this);
}
// 将派生类的原型的原型链挂在基类的原型上
Object.setPrototypeOf(Derived.prototype,Base.prototype);
```
#### Object.setPrototypeOf(obj,prototype)
+ 参数
  + obj
    + 要设置其原型的对象
  + prototype
    + 该对象的新原型(一个对象 或null)
+ Polyfill
```javascript
Object.setPrototypeOf = Object.setPrototypeOf || function (obj,proto) {
  obj.__proto__ = proto;
  return obj;
}
```