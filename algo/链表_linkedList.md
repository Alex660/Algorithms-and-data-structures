+ 定义
  + 略
+ 对比
  + ![](https://static001.geekbang.org/resource/image/d5/cd/d5d5bee4be28326ba3c28373808a62cd.jpg)
+ 单链表
  + 图解
    + ![](https://static001.geekbang.org/resource/image/b9/eb/b93e7ade9bb927baad1348d9a806ddeb.jpg)
  + 插入/删除
    + ![](https://static001.geekbang.org/resource/image/45/17/452e943788bdeea462d364389bd08a17.jpg)
+ 循环链表
  + ![](https://static001.geekbang.org/resource/image/86/55/86cb7dc331ea958b0a108b911f38d155.jpg)
+ 双向链表
  + ![](https://static001.geekbang.org/resource/image/cb/0b/cbc8ab20276e2f9312030c313a9ef70b.jpg)
+ 双向循环链表
  + ![](https://static001.geekbang.org/resource/image/d1/91/d1665043b283ecdf79b157cfc9e5ed91.jpg)
+ **代码实现【结合图解看】**
  + 单链表插入、删除、查找
  ```javascript
  class Node {
      constructor (val){
          this.val = val
          this.next = null
      }
  }
  class LinkedList {
      constructor () {
          this.head = new Node('head')
      }
      // 查找节点 - 根据val值
      findNodeByVal (val) {
        let curr = this.head
        while(curr != null && curr.val != val){
            curr = curr.next
        }
      }
      // 查找节点 - 根据索引/index
      findNodeByIndex (index) {
          let curr = this.head
          let i = 0
          while(curr != null && i !== index){
              pos++
          }
          return curr != null ? curr : -1
      }
      // 插入
      insert (newVal,val) {
          let curr = this.findNodeByVal(val)
          if(crr == -1) return false;
          let newNode = new Node(newVal)
          newNode.next = curr.next
          curr.next = newNode
      }
      // 查找当前节点的前一个节点
      findNodePre (nodeVal) {
          let curr = this.head
          while(curr.next != null && curr.next.val != nodeVal){
              curr = curr.next
          }
          return curr != null ? curr : -1
      }
      // 删除节点
      remove (nodeVal) {
          let needRemoveNode = findNodeByVal(nodeVal)
          if(needRemoveNode == -1) return false;
          let prevNode = this.findNodePre(nodeVal)
          prevNode.next = needRemoveNode.next
      }
      // 遍历节点
      display (){
          let res = []
          let curr = this.head
          while(curr != null){
              res.push(curr.val)
              curr = curr.next
          }
          return res
      }
  }
  ``` 
  + 单链表反转
  + 链表中环的检测
  + 两个有序的链表合并
  + 删除链表倒数第n个节点
  + 求链表的中间结点
  ```javascript
    // 忙，待更
  ``` 