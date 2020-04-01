### 定义
+ 队列是一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作，
+ 和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头。
### 应用
+ 实现队列结构
  + 用数组
  + 用链表
+ 实现循环队列结构
  + 用数组
  + 用链表
+ 栗子
  + 击鼓传花 - 循环队列
  + 回文检测
### code
#### 实现队列结构
+ 用数组实现
  ```javascript
  class Queue {
    constructor() {
      this.items = []
    }
    enqueue(el) {
      this.items.push(el)
    }
    dequeue() {
      if(this.items.length) {
        return this.items.shift()
      }
      return -1
    }
    clear() {
      this.items = []
    }
    getFront() {
      return this.items[0]
    }
    getRear() {
      return this.items[this.items.length - 1]
    }
    size() {
      return this.items.length
    }
  }
  ``` 
+ 用链表实现
  ```javascript
  class Node {
    constructor(el) {
      this.el = el
      this.next = null
    }
  }
  class Queue {
    constructor() {
      this.head = null
      this.tail = null
    }
    enqueue(el) {
      if(!this.head) {
        this.head = new Node(el)
        this.tail = this.head
        return
      }
      this.tail.next = new Node(el)
      this.tail = this.tail.next
    }
    dequeue() {
      if(this.head) {
        const el = this.head.el
        this.head = this.head.next
        return el
      }
      return -1
    }
    size() {
      let n = 0,cur = this.head
      while(cur) {
        cur = cur.next
        n++
      }
      return n
    }
  }
  ``` 
#### 实现循环队列结构
+ 用数组实现
  + **解法一(推荐):**[设计循环双端队列-戳看](https://github.com/Alex660/leetcode/blob/master/leetCode-1-0641-design-circular-deque.md)
  + 解法二：
    ```javascript
    class CircularQueue {
      constructor(n) {
        this.queue = new Array(n) // 初始化循环队列
        this.front = 0 // 队头指针
        this.rear = 0 // 队尾指针
        this.count = 0 // 队列所用容量计数
        this.n = n // 队列容量
      }
      enqueue(el) {
        if(this.count === this.n) return false // 队列已满
        this.queue[this.rear] = el
        this.count++
        this.rear = (this.rear + 1) % this.n
        return true
      }
      dequeue() {
        if(!this.count) return false // 队列已空
        this.count--
        this.front = (this.front + 1) % this.n
        return true
      }
    }
    ```
+ 用链表实现
  ```javascript
  class Node {
    constructor(el) {
      this.el = el
      this.next = null
    }
  }
  class CircularQueue {
    constructor() {
      this.head = null
      this.tail = null
    }
    enqueue(el) {
      if(!this.head) {
        this.head = new Node(el)
        this.head.next = this.head
        this.tail = this.head
        return
      }
      const flag = this.head === this.tail
      this.tail.next = new Node(el)
      this.tail.next.next = this.head
      this.tail = this.tail.next
      if(flag) {
        this.head.next = this.tail
      }
    }
    dequeue() {
      if(!this.head) return -1
      if(this.head === this.tail) {
        const el = this.head.el
        this.head = null
        return el
      }
      const el =this.head.el
      this.head = this.head.next
      this.tail.next = this.head
      return el
    }
    printAll() {
      let res = 0
      console.log(`打印---dequeue---元素`)
      while(res !== -1) {
        console.log(res = this.dequeue())
      }
    }
  }
  ``` 
#### 栗子
+ 击鼓传花 - 循环队列
  + >在这个游戏中，孩子们围成一个圆圈，把花尽快地传递给旁边的人。某一时刻传花停止，
这个时候花在谁手里，谁就退出圆圈结束游戏。重复这个过程，直到只剩一个孩子（胜者）。
  ```javascript
  function drumFlower (children,k) {
    let queue = new Queue()
    for(let i = 0;i < children.length;i++) {
      queue.enqueue(children[i])
    }
    let outer = ''
    while(queue.size() > 1) {
      // 将第k个人前面的人插入到队尾
      for(let j = 0;j < k;j++) {
        queue.enqueue(queue.dequeue())
      }
      // 轮到谁谁出队
      queue.dequeue()
    }
    // 返回最后的胜利者✌️
    return queue.dequeue()
  }
  // 或者直接用数组
  function drumFlower2(arr,n){
	while(arr.length>1){
      for(var i=0;i<n;i++){
        arr.push(arr.shift());	
      }
      arr.shift();
	}
	console.log(arr[0]);
  }
  // 测试
  let children = [1,2,3,4,5,6,7]
  let k = 1
  console.log(`胜利者✌️：${drumFlower(children,k)}`)
  ``` 
+ 回文检测
  ```javascript
  // 可提issue
  ``` 