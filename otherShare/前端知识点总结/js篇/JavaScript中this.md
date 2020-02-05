## JavaScript中this
### 定义
+ **this指的是函数运行时所在的环境**
### 使用场景
+ 1、作为一般函数执行
  + this指向全局对象
    + 此处是 window
  + 栗子
    ```javascript
    var obj = {
        foo: function () { console.log(this.bar) },
        bar: 1
    };
    var foo = obj.foo;
    var bar = 2;
    foo() // 2
    ``` 
+ 2、作为对象属性执行
  + this指代上级对象
    + 此处是 obj
  + 栗子
    ```javascript
    var obj = {
        foo: function () { console.log(this.bar) },
        bar: 1
    };
    var bar = 2;
    obj.foo() // 1
    ``` 
+ 3、显示绑定
  + new
  + call、apply、bind调用
  + 栗子
    ```javascript
    // apply、call
    function foo(b){
        console.log(this.a,b);
        return this.a+b;
    }
    var obj = {
        a:2
    };
    var bar = function() {
        return foo.apply(obj,arguments);
    };
    var b = bar(3);// 2 3
    console.log(b);//5

    // bind
    function foo(b){
        console.log(this.a,b);
        return this.a+b;
    }
    var obj = {
        a:2
    };
    var bar = foo.bind(obj);
    var b = bar(3);// 2 3
    console.log(b);//5

    // new
    function foo(a){
        this.a = a;
    }
    var bar = new foo(2);
    console.log(bar.a);//2
    ``` 
+ 4、箭头函数中的 this
  + 不适用于上面任何一种规则
  + 根据外层作用域来决定this
    + 函数
    + 全局
    + 一旦绑定，不可再改
  + 栗子
    ```javascript
    // 函数
    function User() {
        this.name = 'John';
        return () => {
            console.log(this); 
        };
    }
    var obj1 = {a:2};
    var obj2 = {b:3};
    const user = new User()(); // {name:"John"}
    User()(); // window
    User.bind(obj1)()() // {a: 2, name: "John"}

    // 全局
    var o = {
        fn:() => {
            console.log(this)
        }
    }
    var obj = {a:2};
    o.fn() // window
    o.fn.bind(obj)() // window 不可再改

    // 一旦绑定，不可再改
    function User() {
        this.name = 'John';
        return () => {
            console.log(this); 
        };
    }
    var obj1 = {a:2};
    var obj2 = {b:3};
    var user = User.call(obj1);
    user(); // {a: 2, name: "John"}
    user.call(obj2); // {a: 2, name: "John"}
    ``` 