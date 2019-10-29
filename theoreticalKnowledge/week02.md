# 摘要
+ 哈希表、映射、集合
+ 树、二叉树、二叉搜索树、图(Graph)
+ 递归
+ 分治
+ 回溯
+ 课后思考
+ 源码分析

### 哈希表、映射、集合  
+ Hash table 也叫散列表
+ 核心问题
  + 散列函数设计
  + 散列冲突解决
+ 其实是数组对一种扩展 
+ Hash Function 也叫散列函数
  ![](https://static001.geekbang.org/resource/image/92/73/92c89a57e21f49d2f14f4424343a2773.jpg)
+ 散列冲突
  + 解决方法
    + 开放寻址法
      + 手段 
        + 线性探测
        + 二次探测
        + 双重散列
      + 优化
        + 装载因子
          + 保证散列表空间中一定比例对空闲槽
          + 装载因子越大，说明空闲位置越少，冲突越多，散列表的性能会下降。  
          > 散列表的装载因子 = 填入表中的元素个数 / 散列表的长度
    + 链表法
      ![](https://static001.geekbang.org/resource/image/a4/7f/a4b77d593e4cb76acb2b0689294ec17f.jpg) 
+ 工程实践
  + 电话号码簿
  + 用户信息表
  + 缓存(LRU Cache)
  + 键值对存储
  + Word文档单词拼写检查功能
    + 用散列表来存储整个英文单词词典。
    + 如果查到，则说明拼写正确；
    + 如果没有查到，则说明拼写可能有误
  + 假设我们有 10 万条 URL 访问日志，如何按照访问次数给 URL 排序？
    + 遍历 10 万条数据，以 URL 为 key，访问次数为 value，存入散列表，
    + 同时记录下访问次数的最大值 K，时间复杂度 O(N)。
    + 如果 K 不是很大，可以使用桶排序，时间复杂度 O(N)。
    + 如果 K 非常大（比如大于 10 万），就使用快速排序，复杂度 O(NlogN)。
  + 有两个字符串数组，每个数组大约有 10 万条字符串，如何快速找出两个数组中相同的字符串？
    + 以第一个字符串数组构建散列表，key 为字符串，value 为出现次数。
    + 再遍历第二个字符串数组，以字符串为 key 在散列表中查找，
    + 如果 value 大于零，说明存在相同字符串。时间复杂度 O(N)。



### 树
+ 一种非线性表结构
  + 高度(Height)
  + 深度(Depth)
  + 层  (Level)
    ![](https://static001.geekbang.org/resource/image/40/1e/4094a733986073fedb6b9d03f877d71e.jpg) 
    ![](https://static001.geekbang.org/resource/image/50/b4/50f89510ad1f7570791dd12f4e9adeb4.jpg)
  + 二叉树
    + 满二叉树
      + 叶子节点全部在最底层
      + 除了叶子节点之外，每个节点都有左右两个子节点
      + 也是完全二叉树的一种特殊情况
    + 完全二叉树
      + 叶子节点都在最底下两层 
      + 最后一层的叶子节点都靠左排列
      + 除了最后一层，其它层的节点个数都要达到最大
    + 二叉树的存储
      + 基于指针或引用的二叉链式存储法
      + 每个节点有三个字段
        + 存储数据
        + 存储左右子节点的指针
        ![](https://static001.geekbang.org/resource/image/12/8e/12cd11b2432ed7c4dfc9a2053cb70b8e.jpg) 
      + 基于数组的顺序存储法
        + 根节点 i=1
        + 左子节点 2*i
        + 右子节点 2*i+1 
        + 完全二叉树的存储
            ![](https://static001.geekbang.org/resource/image/14/30/14eaa820cb89a17a7303e8847a412330.jpg) 
        + 非完全二叉树的存储
            ![](https://static001.geekbang.org/resource/image/08/23/08bd43991561ceeb76679fbb77071223.jpg)   
        + 结论
          + 完全二叉树 用数组存储是最节省内存的一种方式 
          + 不需要像链式存储法那样额外存储左右子节点的指针
          + 也是完全二叉树要求最后一层的子节点都靠左的原因
        + 应用
          + 堆  
        + 思考
          + 给定一组数据，比如 1，3，5，6，9，10。你来算算，可以构建出多少种不同的二叉树？
            + n! 阶乘 即 卡塔兰数
    + 二叉树的遍历
      + 时间复杂度 O(n) 因为每个节点最多被访问两次，跟节点的个数 n 成正比 
      + 以下根代表 任意节点 
      + 前序(Pre-order):根-左-右
      + 中序(In-order):左-根-右
        + 一种升序排列 
      + 后序(Post-order):左-右-根
      + 均是一种递归的过程
      + 前序遍历的递推公式：
        + preOrder(r) = print r->preOrder(r->left)->preOrder(r->right)
      + 中序遍历的递推公式：
        + inOrder(r) = inOrder(r->left)->print r->inOrder(r->right)
      + 后序遍历的递推公式：
        + postOrder(r) = postOrder(r->left)->postOrder(r->right)->print r

      ![](https://static001.geekbang.org/resource/image/ab/16/ab103822e75b5b15c615b68560cb2416.jpg) 
      + 层序遍历
        + 可以看作以根节点为起点，图的广度优先遍历的问题
    + 二叉树的高度
      + 根节点的高度 = max(左子树高度，右子树高度) + 1  
    + 二叉搜索树 Binary Search Tree
      + 也叫二叉搜索排序树、有序二叉树(Ordered Binary Tree)、排序二叉树(Sorted Binary Tree)
      + 是指一颗空树 或 具有下列性质的二叉树
        + 左子树上 所有结点 的值均小于它的根节点的值
        + 右子树上 所有结点 的值均大于它的根节点的值
        + 以此类推：左、右子树也分别为二叉查找树(即重复性)
      + 常见操作
        + 查询
        + 插入新结点（创建）
        + 删除 
        + [操作图示](https://visualgo.net/zh/bst)



### 递归
+ 解题关键
  + 写出递推公式 
    + 递归模版
    ```java
    //  recur 递归
    public void recur(int level, int param) { 
      // terminator 终结条件
      if (level > MAX_LEVEL) { 
        // process result 流程结果
        return; 
      } 
      // process current logic  当前逻辑过程
      process(level, param); 
      // drill down 向下递归
      recur( level: level + 1, newParam); 
      // restore current status 恢复现状
    }
    ```  
  + 警惕堆栈溢出
  + 警惕重复计算
    + 可以使用备忘录模式 
    ![](https://static001.geekbang.org/resource/image/e7/bf/e7e778994e90265344f6ac9da39e01bf.jpg)
+ 应用实践
  + DFS深度优先搜索
  + 前中后序二叉树遍历





### 分治
+ 原问题分解成的子问题可以独立求解，子问题之间没有相关性，是和动态规划的明显区别
+ 实践应用
  + Google的 MapReduce 
+ 代码模版
  + 递归的代码模版加上合并所有子问题操作
    1. terminator 
    2. process(split your big problem)
    3. drill down subproblems
    4. merge(subsult)
    5. if need reverse states

### 回溯
+ 用来解决广义的搜索问题
  + 从一组可能的解中，选择出一个满足要求的解
  + 本质是枚举，即投石问路
+ 解题方法
  + 递归
  + 搜索减枝  
+ 实践应用
  + 蝴蝶效应
  + DFS 深度优先搜索算法
  + 正则表达式匹配
  + 编译原理总的语法分析
  + 数独
  + 八皇后问题
  + 0-1 背包
  + 图的着色
  + 旅行商问题
  + 全排列问题
+ 代码示例
  + 八皇后
  ![](https://static001.geekbang.org/resource/image/a0/f5/a0e3994319732ca77c81e0f92cc77ff5.jpg)
  ```javascript
  // 下标为row，其值为column
  var result = new Array(8);
  // 主函数
  function cal8Queens(row){
    if(row == 8){
      return result;
    }
    // 每一行有 column 种放法
    for(var column = 0;column < 8;column++){
      // 判断合法性
      if(isOK(row,column)){
        // 第row行第column列 可以放一个棋子
        result[row] = column;
        // 考察下一行
        cal8Queens(row+1);
      }
    }
  }
  // 核心放法
  // 判断上面每一行至其他列有值的地方是否与将要放的棋子相冲突
  function isOk(row,column){
    var leftColumn = column - 1;
    var rightColumn= column + 1;
    // 逐上考察每一行
    for(var i =row-1;i >= 0;i--){
      // 上面某一行当前列有值
      if(result[i] == column){
        return false;
      }
      // 左对角线有值
      if(result[i] == leftColumn){
        return false;
      }
      // 右对角线有值
      if(result[i] == rightColumn){
        return false;
      }
      // 两侧扩散
      leftColumn--;
      rightColumn++;
    }
    // 上面每一行都通过
    return true;
  }
  ```

  + 0-1背包问题
    > 一个背包 总重为 Wkg  
    现有 n 个物品 每个物品重量不等且不可分割  
    求在不超过背包重量的前提下，选择哪几样物品装载到背包中能使得物品的总重量最大？  
    求解思路：回溯  
    将物品依次排列，整个问题就分解成了N个阶段  
    每个阶段只有两种选择：装 与 不装  
    再递归处理剩下的物品
  ```javascript
  // w 背包重量
  // cw 表示已经装进去的物品重量和
  // items 每个物品的重量集合数组
  // n 物品个数
  // fn(n,cw)
  var maxW = 0；// 可以存储进入背包中的物品总重量的最大值
  function getMaxV(i,cw){
    // 递归终止条件
    if(cw == w || i == n){
      // cw == w 表示装满了，不判断大于0的情况是因为下面选择装的时候做了准入判断
      // i == n  表示已经考察完所有的物品
      // 更新结果最大值
      if( cw > maxW){
        maxW = cw;
      }
      return;
     }
    // 不装 && 进入下一个物品选择阶段
    getMaxV(i+1,cw)
    // 装 && 进入下一个物品选择阶段
    // 加上搜索减枝
    if(cw + items[i] <= w){
      getMaxV(i+1,cw + items[i])
    }
  }
  ```
  + 正则匹配
    > "*" 匹配任意多个(>=0)任意字符  
      "?" 匹配零个 或者 一个任意字符  
      求用回溯算法，判断一个给定的文本，能否跟给点的正则表达式匹配？
    + 思路 当依次考察正则表达式中的每个字符时
      + 当是非通配符时
        +  直接跟文本的字符进行匹配
          + 相同：递归进入下一个字符的一对一匹配
          + 不同：回溯 即回到当....时
      + 当是通配符时
        + 当是“*”时
          + 符合匹配规则则继续考察下一个字符
          + 不符合则回溯 即回到 当是"*"时 并进入下一个匹配规则
        + 当是“?”时
          + 同上处理 
  + 代码
  ```javascript
  function patternStr(pattern,text){
    var matched = false;
    var plen = pattern.length;
    var tlen = text.length;
    rematch(0,0);
    // 核心方法
    // ti text每个字符的当前索引位置
    // pj pattern 每个字符的当前位置索引
    function rmatch(ti,pj){
      // 递归过程中 匹配成功 则不需要继续了 直接返回结果
      if(matched){
        return;
      }
      if(pj == plen){//正则表达式匹配到结尾了
        if(ti == tlen){//文本串也到结尾了
          return;
        }
      }
      //* 匹配任意个字符
      if(pattern[pj] == '*'){
        for(var k = 0;k <= tlen - ti;k++){
          rematch(ti + k,pj+1);
        }
      }
      // ？匹配 0个 或 1个字符
      else if(pattern[j] == '?'){
        rematch(ti,pj+1);
        rematch(ti+1,pj+1);
      }
      //纯字符匹配 一对一
      else{
        rematch(ti+1,pj+1);
      }
    }
    // 返回结果
    return matched;
  }
  ``` 

### 课后思考
+ 树的面试题解法一般都是递归，为什么？
  + 树一般都是左右子树遍历查找 重复性质 可以递归调用

### 源码分析
+ Java Hash Map  
  + 由数组、链表、红黑树组成
    ![](https://upload-images.jianshu.io/upload_images/7853175-1b3908dd171372ea.png?imageMogr2/auto-orient/strip|imageView2/2/w/978/format/webp) 
  + 判断两个对象是否相等 ==(地址)
    ```java
    public boolean equals(Object obj) {
        return (this == obj);
    }
    ```   
    + 自反性
    + 对称性
    + 传递性
    + 一致性
    + 非空性
  + 获取节点的 hashCode
    + 用于查找的快捷性
    + 对象上就有 但可以覆写如String
    ```java
    static final int hash(Object key) {
        int h;
        // h = key.hashCode() 为第一步 取hashCode值
        // h ^ (h >>> 16)  为第二步 高位参与运算
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

    static int indexFor(int h, int length) {  
        return h & (length-1);  //第三步 取模运算
    }
    ``` 
    + 在JDK1.8的实现中，优化了高位运算的算法，  
    + 通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，
    + 主要是从速度、功效、质量来考虑的，这么做可以Node数组table的length比较小的时候，
    + 也能保证考虑到高低Bit都参与到Hash的计算中，同时不会有太大的开销。
  
    ![](https://segmentfault.com/img/remote/1460000015213256)
  + get 查找通过 getNode实现
    ```java
    /**
     * Implements Map.get and related methods
     *
     * @param hash hash for key
     * @param key the key
     * @return the node, or null if none
     */
    final Node<K,V> getNode(int hash, Object key) {
        // 变量说明：tab用于存储table引用，first用于存储数组中第一个节点，e为目标节点
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            // 在hash值相等的情况下，进行key值匹配
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            //  判断散列冲突 检测节点是链表还是红黑树，然后分别调用不同的方法进行匹配
            if ((e = first.next) != null) {
                // 红黑树
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                // 链表
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
    ```
  + put 函数 插入操作实现 
    ```java
    /**
     * Implements Map.put and related methods
     *
     * @param hash hash for key
     * @param key the key
     * @param value the value to put
     * @param onlyIfAbsent if true, don't change existing value
     * @param evict if false, the table is in creation mode.
     * @return previous value, or null if none
     */
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        // table 为空的检测
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        // 对hash进行映射，并检测数组头是否为空，为空则直接创建新的节点
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        // 数组头不为空，则进行碰撞的值的检测
        else {
            Node<K,V> e; K k;
            // 若当前数组头匹配，则直接返回数组头节点
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            // 对节点类型进行检测，若为红黑树，则调用红黑树的插入逻辑
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            // 否则为链表
            else {
                // 对链表进行匹配
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            // 对匹配成功对节点进行值的更新
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
    ```
    + 操作示意图
    ![](https://img-blog.csdn.net/20180602151326563)
   + 判断是否是红黑树即树形节点时 调用 TreeNode
   + 表扩容通过 resize函数实现
     + table的长度保持在2^n次方 
     + 在不超过最大容量限制的前提下进行2倍扩容操作
     + 将旧的table迁移到新的table中去
        > 旧元素的位置 == 新元素的位置索引  
          新插入的位置 == 偏移oldCap + index 的索引位置上去  
     +  => 位运算 优化模运算 
        >这个方法非常巧妙，它通过h & (table.length -1)来得到该对象的保存位，  
        而HashMap底层数组的长度总是2的n次方，这是HashMap在速度上的优化。  
        当length总是2的n次方时，h& (length-1)运算等价于对length取模，  
        也就是h%length，  但是&比%具有更高的效率。  
        +  假设原来的table大小为4，那么扩容之后会变为8，
        +  那么对于一个元素A来说，如果他的hashCode值为3，
        +  那么他在原来的table上的位置为(3 & 3) = 3,
        +  那么新位置呢？(3 & 7) = 3,
        +  这种情况下元素A的index和原来的index是一致的不用变。
        +  再来看一个元素B，他的hashCode值为47，那么在原来table中的位置为(47 & 3) = 3
        +  在新table中的位置为(47 & 7) = 7，也就是（3 + 4），正好偏移了oldCap个单位。
        +  因为我们的计算方法为:(hashCode & (length - 1))
     + 所以扩容之后新插入=元素比原来多判断一步 边插边判断是否需要移动元素到新的table中去 
     + if (oldTab != null)
        ![](https://segmentfault.com/img/remote/1460000015213258) 
        + 还是上面的两个元素A和B，哈希值分别为3和47，在table长度为4的情况下，因为(3) = (11)，
        + 所以A和B会有两位参与运算来获得index，A和B的二进制分别为：
          + 3 ： 11
          + 47： 101111
        + 在table的length为4的前提下：
          + 3-> 11 & 11 = 3
          + 47-> 000011 & 101111 = 3
        + 在扩容后，length变为8：
          + 3-> 011 & 111 = 3
          + 47-> 10111 & 00111 = 7
        + 对于3来说，新增的参与运算的位为0，所以index不变，
        + 而对于47来说，新增的参与运算的位为1，所以index需要变为(index + oldCap)
          + newTab[e.hash & (newCap - 1)] = e;   