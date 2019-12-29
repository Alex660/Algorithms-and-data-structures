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
      // 查找当前节点 - 根据val值
      // 适用于链表中无重复节点
      findNodeByVal (val) {
        let curr = this.head
        while(curr != null && curr.val != val){
            curr = curr.next
        }
        return curr ? curr : -1
      }
      // 查找当前节点 - 根据索引/index
      // 适用于链表中有重复节点
      findNodeByIndex (index) {
          let curr = this.head
          let pos = 1
          while(curr != null && pos !== index){
              curr = curr.next
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
      // 查找当前节点的前一个节点 - 根据val值
      // 适用于链表中无重复节点
      findNodePreByVal (nodeVal) {
          let curr = this.head
          while(curr.next != null && curr.next.val != nodeVal){
              curr = curr.next
          }
          return curr != null ? curr : -1
      }
      // 查找当前节点的前一个节点 - 根据索引/index
      // 适用于链表中无重复节点 
      // 同 findNodeByIndex，只要index传的是前一个节点的索引就对了
    
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
    + 解法一：迭代
    + 时间复杂度：O(n)
    + 空间复杂度：O(1)
    + 图解
    + ![截屏2019-12-19下午11.03.37.png](https://pic.leetcode-cn.com/88bfee967d158aa1cc94bd56539577f166571e1f63ab3a0510629b4189f4a3c1-%E6%88%AA%E5%B1%8F2019-12-19%E4%B8%8B%E5%8D%8811.03.37.png)
    + 思路
    + 关键
        + 将当前节点的指针指向上一个节点
        + 然后更新当前节点和下一个节点的值即顺移
    + 技巧
        + 设置哨兵节点 null，初始化时将head节点指向null，下一步将next指向head
        + 重复以上动作直到当前节点为尾节点的节点null
    ```javascript
    /**
     * Definition for singly-linked list.
     * function ListNode(val) {
     *     this.val = val;
     *     this.next = null;
     * }
     */
    /**
     * @param {ListNode} head
     * @return {ListNode}
     */
    var reverseList = function(head) {
        let prev = null;
        let curr = head;
        while(curr != null){
            let next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    };
    ```
    + 解法二：尾递归
    + 思路
    + 其实就是解法一的简化版
        + >prev = curr;  
        curr = next;
    + 此解法将 上面放在递归里返回
        + 同理都是做将当前节点指向前一个节点的操作之后，来顺移更新前一个、当前、和下一个节点的操作
    ```javascript
    /**
     * Definition for singly-linked list.
     * function ListNode(val) {
     *     this.val = val;
     *     this.next = null;
     * }
     */
    /**
     * @param {ListNode} head
     * @return {ListNode}
     */
    var reverseList = function(head) {
        let reverse = (prev,curr) => {
            if(!curr)return prev;
            let next = curr.next;
            curr.next = prev;
            return reverse(curr,next);
        }
        return reverse(null,head);
    };
    ```
    + 解法三：递归
    + 思路
    + 关键是反转操作
        + 当前节点 head，下一个节点 head.next
        + head.next.next = head
        + 此处将原 head.next 指向head，即是反转
        + head.next = null
        + 此处将原 head 指向head.next的指针断开
    + 递归
        + 由编译器函数调用执行栈原理可知
        + **最先调用的函数会在递归过程中最后被执行，而最后调用的会最先执行**
        + 因此此题，**最先返回最后两个节点开始反转操作**
        + 依次从后面两两节点开始反转
    + 图解
    + ![截屏2019-12-20上午8.07.31.png](https://pic.leetcode-cn.com/aa52928915a4e9d142e206901f8d334d767d29b7482e000078380f28d22c5699-%E6%88%AA%E5%B1%8F2019-12-20%E4%B8%8A%E5%8D%888.07.31.png)
    ```javascript
    /**
     * Definition for singly-linked list.
     * function ListNode(val) {
     *     this.val = val;
     *     this.next = null;
     * }
     */
    /**
     * @param {ListNode} head
     * @return {ListNode}
     */
    var reverseList = function(head) {
        // 如果测试用例只有一个节点 或者 递归到了尾节点，返回当前节点 
        if(!head || !head.next) return head;
        // 存储当前节点的下一个节点
        let next = head.next;
        let reverseHead = reverseList(next);
        // 断开 head ，如图闪电⚡️标记处
        head.next = null;
        // 反转，后一个节点连接当前节点
        next.next = head;
        return reverseHead;
    };
    ```
    + 解法四：栈解
    + 思路
    + 既然是反转，那么符合栈先进后出的特点
    + 将原节点依次入栈
    + 出栈时，重新构造链表改变指向
    + 同样设置哨兵节点
        + 最后返回哨兵的next即为所求
    ```javascript
    /**
     * Definition for singly-linked list.
     * function ListNode(val) {
     *     this.val = val;
     *     this.next = null;
     * }
     */
    /**
     * @param {ListNode} head
     * @return {ListNode}
     */
    var reverseList = function(head) {
        let tmp = head;
        let tHead = new ListNode(0);
        let pre = tHead;
        let stack = [];
        while(tmp){
            stack.push(tmp.val);
            tmp = tmp.next;
        }
        while(stack.length != 0){
            pre.next = new ListNode(stack.pop());
            pre = pre.next;
        }
        return tHead.next;
    };
    ```    
  + 链表中环的检测
    + 待更
  + 两个有序的链表合并
    ```javascript
    /**
      * Definition for singly-linked list.
      * function ListNode(val) {
      *     this.val = val;
      *     this.next = null;
      * }
      */
      /**
      * @param {ListNode} l1
      * @param {ListNode} l2
      * @return {ListNode}
    */
    ``` 
    + 解法一：双指针
    + 思路：归并排序的核心
    ```javascript
    let mergeTwoLists = (l1,l2) => {
        let preHead = new ListNode(-1);
        let prevNode = preHead;
        while(l1 != null && l2 != null){
            if(l1.val <= l2.val){
                prevNode.next = l1;
                l1 = l1.next;
            }else{
                prevNode.next = l2;
                l2 = l2.next;
            }
            prevNode = prevNode.next;
        }
        prevNode.next = l1 ? l1 : l2;
        return preHead.next;
    }
    ``` 
    + 解法二：分治
    ```javascript
    let mergeTwoLists = (l1,l2) => {
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        if(l1.val <= l2.val){
            l1.next = mergeTwoLists(l1.next,l2);
            return l1;
        }else{
            l2.next = mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
    ``` 
  + 删除链表倒数第n个节点
    + 解法一：单链表经典操作
    ```javascript
    var removeNthFromEnd = function(head, n) {
        // 反转链表
        let prev = null;
        let curr = head;
        var reverse = (prev,curr) => {
            while(curr){
                let next = curr.next;
                curr.next = prev;
                prev = curr;
                curr = next;
            }
            return prev
        }
        let newHead = reverse(prev,curr);
        // 边界条件处理
        if(n == 1){
            return reverse(null,newHead.next);
        }
        // 查询节点
        let findNodeByIndex = (currSearchTmp,n) => {
            let pos = 1;
            while(currSearchTmp && pos != n){
                currSearchTmp = currSearchTmp.next
                pos++
            }
            return currSearchTmp
        }
        let currSearch = findNodeByIndex(newHead,n);
        // 查找前一个节点
        let pre = findNodeByIndex(newHead,n-1);
        // 删除节点
        pre.next = currSearch.next;
        let prevNeed = null;
        let currNeed = newHead;
        // 反转回来
        return reverse(prevNeed,currNeed)
    };
    ```
    + 解法二：两次遍历
    + 时间复杂度：O(n)
    + 空间复杂度：O(1)
    + 思路
    + 删除正数第n个节点
        + 遍历链表，直到第n-1个节点，将第n-1个节点指向第n个节点即完成了删除操作
        + 通过索引位置自减，直到0，就可以找到
    + 删除倒数第n个节点
        + 如解法一
        + 我们通过反转链表来删除第n个节点，即是删除了倒数第n个节点
            + 查找节点
            + 通过遍历位置得到
        + 此处不需反转
        + 举个栗子
        + 1 -> 2 -> 3 -> 4 -> 5
        + 删除倒数第2个节点
        + 等价于
            + 删除正数第4(5 - 2 + 1)个节点
        + **因此此题转换成了删除正数第(L - n + 1)个节点**
        + L为链表长度，需要通过遍历算得
        + 然后解法同正数的相同即可
    ```javascript
    /**
     * Definition for singly-linked list.
     * function ListNode(val) {
     *     this.val = val;
     *     this.next = null;
     * }
     */
    /**
     * @param {ListNode} head
     * @param {number} n
     * @return {ListNode}
     */
    var removeNthFromEnd = function(head, n) {
        let preHead = new ListNode(-1);
        preHead.next = head;
        let len = 0;
        let first = head;
        while(first){
            len++;
            first = first.next;
        }
        len -= n;
        first = preHead;
        while(len != 0){
            len--;
            first = first.next;
        }
        first.next = first.next.next;
        return preHead.next;
    };
    ```
    + 解法三：双指针
    + 时间复杂度：O(n)
    + 空间复杂度：O(1)
    + 思路
      + 设快慢两个指针
        + 1、快指针先移n个节点
        + 2、快、慢指针一起移动，使得两指针之间一直保持n个节点
          + 2.1、当指针到达链表底了，将慢指针的指针指向它的下下一个指针
          + 2.2、慢指针的下一个指针即为要删除的节点
      + 边界
        + 当要删除的节点是head时，需要设置哨兵节点，即增设一个位于当前头部节点的前一个值为null的哨兵节点
          + 最后返回哨兵节点的next即为所求
    ```javascript
    /**
     * Definition for singly-linked list.
     * function ListNode(val) {
     *     this.val = val;
     *     this.next = null;
     * }
     */
    /**
     * @param {ListNode} head
     * @param {number} n
     * @return {ListNode}
     */
    var removeNthFromEnd = function(head, n) {
        let preHead = new ListNode(-1);
        preHead.next = head;
        let fast = preHead;
        let slow = preHead;
        while(n != 0){
            fast = fast.next;
            n--;
        }
        while(fast.next != null){
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return preHead.next;
    };
    ``` 
  + 求链表的中间结点
    + 待更