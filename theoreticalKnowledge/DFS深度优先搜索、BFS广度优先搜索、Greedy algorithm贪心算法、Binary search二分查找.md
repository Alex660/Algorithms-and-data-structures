# 摘要
+ 深度优先搜索 与 广度优先搜索（depth first search,breadth first search）
+ 贪心算法
+ 二分查找
+ 课后思考

### DFS 和 BFS
+ 均基于数据机构-图
+ BFS
  + 借助队列实现
+ DFS
  + 借助栈实现   
+ 实践用应用
  + BFS
    + 找出社交网络中某个用户的三度好友关系
  + DFS
    + 走迷宫
+ 代码
  + 图的广度优先遍历 
    ![](https://static001.geekbang.org/resource/image/ea/23/ea00f376d445225a304de4531dd82723.jpg)
    ```java
    public void bfs(int s, int t) {
        if (s == t) return;
        boolean[] visited = new boolean[v];
        visited[s]=true;
        Queue<Integer> queue = new LinkedList<>();
        queue.add(s);
        int[] prev = new int[v];
        for (int i = 0; i < v; ++i) {
            prev[i] = -1;
        }
        while (queue.size() != 0) {
            int w = queue.poll();
            for (int i = 0; i < adj[w].size(); ++i) {
                int q = adj[w].get(i);
                if (!visited[q]) {
                    prev[q] = w;
                    if (q == t) {
                    print(prev, s, t);
                    return;
                    }
                    visited[q] = true;
                    queue.add(q);
                }
            }
        }
    }

    private void print(int[] prev, int s, int t) { // 递归打印 s->t 的路径
    if (prev[t] != -1 && t != s) {
        print(prev, s, prev[t]);
    }
    System.out.print(t + " ");
    }
    ``` 
  + 走迷宫深度优先搜索 
  ```java
    boolean found = false; // 全局变量或者类成员变量
    public void dfs(int s, int t) {
        found = false;
        boolean[] visited = new boolean[v];
        int[] prev = new int[v];
        for (int i = 0; i < v; ++i) {
            prev[i] = -1;
        }
        recurDfs(s, t, visited, prev);
        print(prev, s, t);
    }

    private void recurDfs(int w, int t, boolean[] visited, int[] prev) {
        if (found == true) return;
        visited[w] = true;
        if (w == t) {
            found = true;
            return;
        }
        for (int i = 0; i < adj[w].size(); ++i) {
            int q = adj[w].get(i);
            if (!visited[q]) {
            prev[q] = w;
            recurDfs(q, t, visited, prev);
            }
        }
    }
  ```
+ 模板
  + BFS
    ```python
        def BFS(graph, start, end):
        queue = [] 
        queue.append([start]) 
        visited.add(start)

        while queue: 
            node = queue.pop() 
            visited.add(node)

            process(node) 
            nodes = generate_related_nodes(node) 
            queue.push(nodes)

        # other processing work 
        ...
    ```  
  + DFS
    + 递归写法
      ```python
        visited = set() 
        def dfs(node, visited):
        if node in visited: # terminator
            # already visited 
            return 

            visited.add(node) 

            # process current node here. 
            ...
            for next_node in node.children(): 
                if not next_node in visited: 
                    dfs(next_node, visited)
      ```   
    + 非递归写法
      ```python
      def DFS(self, tree): 
        if tree.root is None: 
            return [] 

        visited, stack = [], [tree.root]

        while stack: 
            node = stack.pop() 
            visited.add(node)

            process (node) 
            nodes = generate_related_nodes(node) 
            stack.push(nodes) 

        # other processing work 
        ...
      ``` 

### 贪心算法
+ 定义
  + 是一种在每一步选择中都采取在当前状态下最好或最优(即最有利)的选择，从而希望导致结果是全局最好或最优的算法
+ 注意
  + 贪心算法并不能每次都能给出最优解
  + 例如最短路径问题，从顶点S开始，找到一条到顶点T的最短路径问题
    ![](https://static001.geekbang.org/resource/image/2d/42/2de91c0afb0912378c5acf32a173f642.jpg) 
    + 贪心算法算得：S->A->E->T,1+4+4=9
    + 实际上最短路径是：S->B->D->T,2+2+2+2=6
+ 区别
  + 和动态规划的不用在于它对每个子问题的解决方案都做出选择，不能回退
  + 动态规划则会保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能
+ 适用场景
  + 问题能分成子问题来解决
    + 子问题的最优解能递推到最终问题的最优解
    + 这种子问题最优解被称为最优子结构 
+ 实践应用
  + 图的最小生成树
  + 哈夫曼编码  
  + 分糖果问题
  + 钱币找零
  + 区间覆盖
    ![](https://static001.geekbang.org/resource/image/f0/cd/f0a1b7978711651d9f084d19a70805cd.jpg) 
    > 假设我们有n个区间，区间的起始端点和结束端点分别是[l1,r1],[l2,r2],[l3,r3],...,[ln,rn]。  
    我们从这n个区间中选出一部分区间，这部分区间满足两两不相交（端点不相交的情况不算相交），最多能选出多少个区间呢？
    + 思路
      + 假设这n个区间中最左端点是lmin，最右端点是rmax。
      + 这个问题等价于，我们选择几个不相交的区间，从左到右将[lmin,rmax]覆盖上。
      + 解题技巧
        + 按照起始端点从小到大的顺序对这n个区间排序  
      + 贪心选择
        + 遍历时，当且仅当左端点区间跟前面的已经覆盖的区间不重合，且右端点尽量小时，剩下的右边的未覆盖区间才能尽可能大，整体才可以放置更多的空间
        ![](https://static001.geekbang.org/resource/image/ef/b5/ef2d0bd8284cb6e69294566a45b0e2b5.jpg) 
    + 任务调度
      + 区间覆盖相同处理思想问题
    + 教师排课 
      + 区间覆盖相同处理思想问题  
    +  单源最短路径算法

### 二分查找（Binary Search）
+ 定义
  + 一种针对有序数据集合的查找算法，也叫折半查找算法 
+ 时间复杂度：O(logn)
  + 每次查找后数据都会缩小原来的一半，即除以2
    + 最坏情况下，直到查找区间被缩小为空，才停止
    ![](https://static001.geekbang.org/resource/image/d1/94/d1e4fa1542e187184c87c545c2fe4794.jpg) 
    + 如图，当空间缩小为1时，n/2^k == 1；k 为总共缩小的次数
      + k = log2n ，即O(logn) 
  + 相同时间复杂度的数据结构
    + 堆
    + 二叉树  
+ 实现
  + 警惕
    + 循环退出条件
      + low<=high ，而不是 
    + mid的取值
      + mid = (low+high)/2 有可能会出问题
        + 当low和high很大的时候，和会溢出
      + 优化
        + low+(high-low)/2      
      + 进一步优化
        + low+((high-low)>>1) 
  + 模版
    + java
      ```java
      public int bsearch(int[] a, int n, int value) {
        int low = 0;
        int high = n - 1;

        while (low <= high) {
          int mid = (low + high) / 2;
          if (a[mid] == value) {
            return mid;
          } else if (a[mid] < value) {
            low = mid + 1;
          } else {
            high = mid - 1;
          }
        }

        return -1;
      }
      ```    
    + python
      ```python
      left, right = 0, len(array) - 1 
      while left <= right: 
          mid = (left + right) / 2 
          if array[mid] == target: 
              # find the target!! 
              break or return result 
          elif array[mid] < target: 
              left = mid + 1 
          else: 
              right = mid - 1
      ``` 
  + 递归
    ```java
    // 二分查找的递归实现
    public int bsearch(int[] a, int n, int val) {
      return bsearchInternally(a, 0, n - 1, val);
    }

    private int bsearchInternally(int[] a, int low, int high, int value) {
      if (low > high) return -1;

      int mid =  low + ((high - low) >> 1);
      if (a[mid] == value) {
        return mid;
      } else if (a[mid] < value) {
        return bsearchInternally(a, mid+1, high, value);
      } else {
        return bsearchInternally(a, low, mid-1, value);
      }
    }
    ``` 
  + 非递归
+ 局限性
  + 依赖于顺序表结构，即数组
    + 利用数组的下标随机访问特性，时间复杂度O(1)
    + 其它数据结构如链表，时间复杂度为O(n)  
  + 针对的是有序数据 
    + 二分查找要求数据必须有序
      + 如果数据无序，则需要先排序，排序的时间复杂度最低为O(nlogn)  
      + 尤其不实用于动态的数据集合
  + 数量太小不实用
    + 直接顺序遍历来得快
  + 数量太大也不实用
    + 因为底层依赖数组，假设有1GB大小的数据，则需要申请1GB的连续存储空间，不现实
+ 常考变形问题
+ 数组中重复元素，且是给定值 
  + 查找第一个值等于给定值的元素
    ```java
    public int bsearch(int[] a, int n, int value) {
      int low = 0;
      int high = n - 1;
      while (low <= high) {
        int mid = low + ((high - low) >> 1);
        if (a[mid] >= value) {
          high = mid - 1;
        } else {
          low = mid + 1;
        }
      }

      if (low < n && a[low]==value) return low;
      else return -1;
    }
    ```  
  + 查找最后一个值等于给定值的元素
    ```java
    public int bsearch(int[] a, int n, int value) {
      int low = 0;
      int high = n - 1;
      while (low <= high) {
        int mid =  low + ((high - low) >> 1);
        if (a[mid] > value) {
          high = mid - 1;
        } else if (a[mid] < value) {
          low = mid + 1;
        } else {
          if ((mid == n - 1) || (a[mid + 1] != value)) return mid;
          else low = mid + 1;
        }
      }
      return -1;
    }
    ``` 
  + 查找第一个大于等于给定值的元素
    ```java
    public int bsearch(int[] a, int n, int value) {
      int low = 0;
      int high = n - 1;
      while (low <= high) {
        int mid =  low + ((high - low) >> 1);
        if (a[mid] >= value) {
          if ((mid == 0) || (a[mid - 1] < value)) return mid;
          else high = mid - 1;
        } else {
          low = mid + 1;
        }
      }
      return -1;
    }
    ``` 
  + 查找最后一个小于等于给定值的元素
    + 如何快速定位出一个IP地址的归属地？
    ```java
    public int bsearch7(int[] a, int n, int value) {
      int low = 0;
      int high = n - 1;
      while (low <= high) {
        int mid =  low + ((high - low) >> 1);
        if (a[mid] > value) {
          high = mid - 1;
        } else {
          if ((mid == n - 1) || (a[mid + 1] > value)) return mid;
          else low = mid + 1;
        }
      }
      return -1;
    }
    ``` 


### 课后思考
+ 如果一个有序数组是一个循环有序数组，比如4，5，6，1，2，3；如何实现一个求"值等于给定值"的二分算法呢？
  + 此题解答可参考[我的题解](https://github.com/Alex660/leetcode/blob/master/leetCode-33-search-in-rotated-sorted-array.md)
  + 解法思路
    + 一、
      1. 找到分界下标，分成两个有序数组
      2. 判断目标值在哪个有序数据范围内，做二分查找
    + 二、
      1. 找到最大值的下标 x;
      2. 所有元素下标 +x 偏移，超过数组范围值的取模;
      3. 利用偏移后的下标做二分查找；
      4. 如果找到目标下标，再作 -x 偏移，就是目标值实际下标。两种情况最高时耗都在查找分界点上，所以时间复杂度是 O(N）。复杂度有点高，能否优化呢？

    + 三、
      1. 我们发现循环数组存在一个性质：以数组中间点为分区，会将数组分成一个有序数组和一个循环有序数组。
      2. 如果首元素小于 mid，说明前半部分是有序的，后半部分是循环有序数组；
      3. 如果首元素大于 mid，说明后半部分是有序的，前半部分是循环有序的数组；
      4. 如果目标元素在有序数组范围中，使用二分查找；
      5. 如果目标元素在循环有序数组中，设定数组边界后，使用以上方法继续查找。时间复杂度为 O(logN)。
+ 使用二分查找，寻找一个半有序数组 [4, 5, 6, 7, 0, 1, 2] 中间无序的地方？
  + 分析
    + 设置low,high左右边界，算出中间数nums[mid]
    + 当nums[mid] > nums[high]时，说明出现了无序的地方在右边
      + low = mid+1 
    + 否则无序点在左侧
      + high = mid  
    + 两边夹逼直到low == high ，剩下的一个元素即为无序点 
  + 代码实现
    ```javascript
    // 查找无序地方即半有序数组的最小元素的索引
    function findMinElementI(nums){
      var low = 0;
      var high = nums.length -1;
      while(low<high){
        var mid = low + ((high - low)>>1);
        if(nums[mid] > nums[high]){
          low = mid +1;
        }else{
          high = mid;
        }
      }
      return low;
    } 
    ```  