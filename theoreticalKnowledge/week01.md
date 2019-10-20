# 摘要
+ 数组
+ 链表
+ 栈
+ 队列
+ 源码分析
+ 图示
    ![](https://static001.geekbang.org/resource/image/b6/77/b6b71ec46935130dff5c4b62cf273477.jpg)
### 数组
+ 数组是一个线性表，是一组连续的存储空间
  + 支持随机访问
  + 低效的“插入”和“删除” 
  + 警惕数组的访问越界问题
  + 下标从0开始
      + 从数组存储的内存模型上来看，“下标”最确切的定义应该是“偏移(offset)”,
        + 如果用a来表示数组的首地址，a[0]就是偏移为0的位置，也就是首地址，
        + a[k]就表示偏移k个type_size的位置，
        + 所以计算a[k]的内存地址只需要
        + a[k]_address = base_address + k * type_size
      + 若是数组从1开始计数，那么我们计算数组元素a[k]的内存地址就是
        + a[k]_address = base_address + (k-1) * type_size
        + 从1开始编号，每次随机访问数组元素都多了一次减法运算，对于cpu来说，就是多了一次减法指令。
   + 容器类实现
        + Java的 ArrayList
          + 支持动态扩容 

+ 操作时间复杂度
    + prepend O(1)
    + append  O(1)
    + lookup  O(1)
    + insert  O(n)
    + delete  O(n)
### 链表 Linked list
+ 是一种在内存中通过节点记录内存地址而互相链接形成一条链的存储方式
+ 区别数组图示
    ![](https://static001.geekbang.org/resource/image/d5/cd/d5d5bee4be28326ba3c28373808a62cd.jpg)
+ 操作时间复杂度
    + prepend O(1)
    + append  O(1)
    + lookup  O(n)
    + insert  O(1)
    + delete  O(1)
+ 解题技巧
    + 警惕指针丢失和内存泄露
      + 插入结点时，注意操作的顺序
        >new_node->next = p->next;  
         p->next = new_node;
      + 删除链表结点时，记得手动释放内存空间
        > p->next = p->next->next; 
    + 不用哨兵结点插入首结点、删除尾结点
      > if (head == null) {  
        head = new_node;  
        }
      >if (head->next == null) {  
        head = null;  
        }
    + 利用哨兵简化实现难度
      + 头结点为哨兵结点
        ![](https://static001.geekbang.org/resource/image/7d/c7/7d22d9428bdbba96bfe388fe1e3368c7.jpg)
    + 重点留意边界条件处理
    + 举例画图，辅助思考
       ![](https://static001.geekbang.org/resource/image/4a/f8/4a701dd79b59427be654261805b349f8.jpg)
    + 多写多练
+ 常考题
    + 单链表反转
    + 链表中环的检测
    + 两个有序的链表合并
    + 删除链表倒数第n个结点       
+ 经典应用场景
    + LRU 缓存淘汰算法
+ 缓存
    + 是一种提高数据读取性能的技术
    + 常见策略
      + LFU(Least Frequently Used)  最少使用策略
      + LRU(Least Recently Used) 最近最少使用策略
+ 常用链表结构
  + 单链表
   ![](https://static001.geekbang.org/resource/image/b9/eb/b93e7ade9bb927baad1348d9a806ddeb.jpg)
     + 插入与删除
        ![](https://static001.geekbang.org/resource/image/45/17/452e943788bdeea462d364389bd08a17.jpg) 
  + 循环链表
    +  是一种特殊的单链表
    +  典型应用
       + 约瑟夫问题 
  + 双链表
    ![](https://static001.geekbang.org/resource/image/cb/0b/cbc8ab20276e2f9312030c313a9ef70b.jpg) 
  + 双向循环链表
    ![](https://static001.geekbang.org/resource/image/d1/91/d1665043b283ecdf79b157cfc9e5ed91.jpg) 
#### 栈
+ 是一种受限的线性表
+ 先进后出
+ 表现
  + 数组实现叫 顺序栈
  + 链表实现叫 链式栈
+ 应用
  + 函数调用
  ```java
    int main() {
        int a = 1; 
        int ret = 0;
        int res = 0;
        ret = add(3, 5);
        res = a + ret;
        printf("%d", res);
        reuturn 0;
   }
    int add(int x, int y) {
        int sum = 0;
        sum = x + y;
        return sum;
    }
  ```
    ![](https://static001.geekbang.org/resource/image/17/1c/17b6c6711e8d60b61d65fb0df5559a1c.jpg)
  + 表达式求值
    >34+13*9+44-12/3
  ![](https://static001.geekbang.org/resource/image/bc/00/bc77c8d33375750f1700eb7778551600.jpg)
  + 括号匹配
    + 用栈来保存未匹配的左括号，从左到右依次扫描字符串。当扫描到左括号时，则将其压入栈中；
    + 当扫描到右括号时，从栈顶取出一个左括号。
    + 如果能够匹配，比如“(”跟“)”匹配，“[”跟“]”匹配，“{”跟“}”匹配，则继续扫描剩下的字符串。
    + 如果扫描的过程中，遇到不能配对的右括号，或者栈中没有数据，则说明为非法格式。
  + 浏览器前进后退功能
#### 队列
+ 是一种操作受限的线性表
+ 先进先出
+ 与栈区分
  + 栈只需维护一个 栈顶指针
    ![](https://static001.geekbang.org/resource/image/9e/3e/9eca53f9b557b1213c5d94b94e9dce3e.jpg)  
  + 队列需要维护两个指针 
    + 队首 Head 永远指向队首元素索引位置
    + 队尾 Tail 永远指向队尾元素下一个索引位置 
+ 表现
  + 用数组实现的队列叫 顺序队列
     ![](https://static001.geekbang.org/resource/image/5c/cb/5c0ec42eb797e8a7d48c9dbe89dc93cb.jpg) 
  + 用链表实现的队列叫 链式队列 
     ![](https://static001.geekbang.org/resource/image/c9/93/c916fe2212f8f543ddf539296444d393.jpg)
+ 循环队列
  ![](https://static001.geekbang.org/resource/image/71/80/71a41effb54ccea9dd463bde1b6abe80.jpg)
  + 队空
    + tail == head
  + 队满
    ![](https://static001.geekbang.org/resource/image/3d/ec/3d81a44f8c42b3ceee55605f9aeedcec.jpg)
    + (tail+1)%n == head
        ```java
        public class CircularQueue {
            // 数组：items，数组大小：n
            private String[] items;
            private int n = 0;
            // head 表示队头下标，tail 表示队尾下标
            private int head = 0;
            private int tail = 0;

            // 申请一个大小为 capacity 的数组
            public CircularQueue(int capacity) {
                items = new String[capacity];
                n = capacity;
            }
            // 入队
            public boolean enqueue(String item) {
                // 队列满了
                if ((tail + 1) % n == head) return false;
                items[tail] = item;
                tail = (tail + 1) % n;
                return true;
            }
            // 出队
            public String dequeue() {
                // 如果 head == tail 表示队列为空
                if (head == tail) return null;
                String ret = items[head];
                head = (head + 1) % n;
                return ret;
            }
        }
        ```      
+ 阻塞队列
  + 在队列的基础上增加阻塞操作 
  + 应用
    + 生产者 - 消费者 模型
      ![](https://static001.geekbang.org/resource/image/9f/67/9f539cc0f1edc20e7fa6559193898067.jpg)
+ 并发队列
  + 线程安全的队列  
    + 最简单直接的实现方式是直接在 enqueue()、dequeue() 方法上加锁，但是锁粒度大并发度会比较低，同一时刻仅允许一个存或者取操作。
    + 实际上，基于数组的循环队列，利用 CAS 原子操作，可以实现非常高效的并发队列。这也是循环队列比链式队列应用更加广泛的原因。
    + 实现无锁并发队列
      + cas + 数组 
+ 其它应用
  + 分布式应用中的消息队列 如 kafka
+ 源码分析
  + [Java](http://fuseyism.com/classpath/doc/java/util/Queue-source.html) 
    ![](https://images2017.cnblogs.com/blog/1182892/201711/1182892-20171122100317930-842768608.png) 
    + Queue
      + java.util.Collection接口的子类型 
      + 方法
        + add  
          + 增加一个元索  如果队列已满，则抛出一个IIIegaISlabEepeplian异常
        + remove  
          + 移除并返回队列头部的元素  如果队列为空，则抛出一个NoSuchElementException异常
        + element  
          + 返回队列头部的元素 如果队列为空，则抛出一个NoSuchElementException异常
        + offer  
          + 添加一个元素并返回true 如果队列已满，则返回false
        + poll   
          + 移除并返问队列头部的元素 如果队列为空，则返回null
        + peek    
          + 返回队列头部的元素 如果队列为空，则返回null
      + PriorityQueue 