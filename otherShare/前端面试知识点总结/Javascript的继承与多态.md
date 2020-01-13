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
  + code
    ```javascript

    ``` 
# ------- 未完待续 -------