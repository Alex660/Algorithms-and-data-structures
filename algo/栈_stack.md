### 定义
+ 它是一种运算受限的线性表。限定仅在表尾进行插入和删除操作的线性表。
+ 特点是 先进后出
### 应用
+ 实现栈结构
  + 用数组
  + 用链表
+ 栗子
  + 实现浏览器的前进后退
  + 实现表达式求值(四则运算)
  + 实现括号匹配
  + 实现进制转换
### code
#### 实现栈结构
+ 用数组实现栈
  ```javascript
  class Stack {
    constructor() {
      this.items = [];
    }
    push(el) {
      this.items.push(el)
    }
    pop() {
      if(!this.items.length) return -1
      return this.items.pop()
    }
    peek() {
      return this.items[this.items.length-1]
    }
    clear() {
      this.items = []
    }
    print() {
      console.log(this.items.toString())
    }
  }
  ``` 
+ 用链表实现栈
  ```javascript
  class Node {
    constructor(el) {
      this.el = el
      this.next = null
    }
  }
  class Stack {
    constructor() {
      this.top = null
    }
    push(val) {
      const node = new Node(val)
      if(!this.top) {
        this.top = node
        return
      }
      node.next = this.top
      this.top = node
    }
    pop() {
      if(!this.top) return -1
      const val = this.top.el
      this.top = this.top.next
      return val
    }
    peek() {
      return this.top
    }
    clear() {
      this.top = null
    }
    print() {
      if(this.top) {
        let cur = this.top
        while(cur) {
          console.log(cur.el)
          cur = cur.next
        }
      }
    }
  }
  ``` 
#### 栗子
+ 实现浏览器的前进后退
+ 图解
  + 1、顺序浏览了a、b、c三个页面
    + 借助左边前进栈
    + ![](https://static001.geekbang.org/resource/image/4b/3d/4b579a76ea7ebfc5abae2ad6ae6a3c3d.jpg)
  + 2、依次后退c、b两个页面
    + 借助右边后退栈
    + ![](https://static001.geekbang.org/resource/image/b5/1b/b5e496e2e28fe08f0388958a0e12861b.jpg)
  + 3、又前进一个页面，此时应显示b页面
    + 借助后退栈后退一个页面、前进栈压进一个页面
    + ![](https://static001.geekbang.org/resource/image/ea/bc/ea804125bea25d25ba467a51fb98c4bc.jpg)
  + 4、基于b页面，跳转到新的页面d。此时后退栈的所有历史页面将无法再后退访问
    + 保留前进栈，清空后退栈
    + ![](https://static001.geekbang.org/resource/image/a3/2e/a3c926fe3050d9a741f394f20430692e.jpg)
+ code
  ```javascript
  class browserRouter {
    // 初始化前进、后退栈
    constructor() {
      this.frontStack = new Stack()
      this.backStack = new Stack()
    }
    // 浏览新页面
    pushNewPage(url) {
      this.frontStack.push(url)
      this.backStack.clear()
      this.printAll()
    }
    // 前进
    front() {
      const url = this.backStack.pop()
      if(url !== -1) {
        this.backStack.push(url)
        this.printAll()
        return
      }
      console.log('无法前进')
    }
    // 后退
    back() {
      const url = this.frontStack.pop()
      if(url !== -1) {
        this.backStack.push(url)
        this.printAll()
        return
      }
      console.log('无法后退')
    }
    // 打印页面栈
    printAll() {
      console.log(`--前进页面--`)
      this.frontStack.print()
      console.log(`--后退页面--`)
      this.backStack.print()
    }
  }
  // 测试
  var browser = new browserRouter()
  browser.pushNewPage('url-1')
  browser.pushNewPage('url-2')
  browser.pushNewPage('url-3')
  // 前进/后退
  browser.back()
  browser.back()
  browser.front()
  browser.pushNewPage('url-new')
  ``` 
+ 实现表达式求值
  ```javascript
  // 可提issue
  ``` 
+ 实现括号匹配
  ```javascript
  // 可提issue
  ```
+ 实现进制转换
  ```javascript
  // 可提issue
  ```