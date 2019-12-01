# 摘要
+ Trie树
+ 并查集
+ 高级搜索
+ 高级树
+ 课后思考

### Trie
+ Trie 树，也叫“字典树”，“单词查找树”或“键树”。顾名思义，它是一个树形结构。
+ 它是一种专门处理字符串匹配的数据结构，用来解决在一组字符串集合中快速查找某个字符串的问题。
+ 优点
  + 最大限度地减少无谓的字符串比较，查找效率比哈希表高。
+ 实现题型
  + [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/208-shi-xian-trie-qian-zhui-shu-by-alexer-660/)
+ 应用题型
  + [212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/solution/212-dan-ci-sou-suo-ii-by-alexer-660/)
+ [其它应用](https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/shi-xian-trie-qian-zhui-shu-by-leetcode/)
  + 地址或其它搜索栏的自动补全功能
  + 拼写检查
  + IP路由（最长前缀匹配）
  + T9（九宫格）打字预测
  + 单词游戏
+ 核心思想
  + 空间换时间
  + 利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的
+ 代码模板
  ```python
  class Trie(object):
  
	def __init__(self): 
		self.root = {} 
		self.end_of_word = "#" 
 
	def insert(self, word): 
		node = self.root 
		for char in word: 
			node = node.setdefault(char, {}) 
		node[self.end_of_word] = self.end_of_word 
 
	def search(self, word): 
		node = self.root 
		for char in word: 
			if char not in node: 
				return False 
			node = node[char] 
		return self.end_of_word in node 
 
	def startsWith(self, prefix): 
		node = self.root 
		for char in prefix: 
			if char not in node: 
				return False 
			node = node[char] 
		return True
  ``` 

### 并查集
+ 适用场景
  + 组团、配对问题
+ 并查集
  + 是一种树型的数据结构，用来**处理一些不相交集合(Disjoint Sets)的合并及查询问题**
  + 联合-查找算法(union-find algorithm)，规范了对此数据结构的操作
    + Find：**确定元素属于哪一个子集**
      + 方法是不断向上查找它的根节点
      + 因而可以**被用来确定两个元素是否属于同一子集**的问题
    + Union：**将两个子集合并成同一个集合**
    + 初始化：
      + 每个元素是自己的集合代表，即自己是一个集合，我为自己代言；
        + ![截屏2019-11-21下午10.39.28.png](https://pic.leetcode-cn.com/4da71cd9c2f65f0b473ae4b9b66cce2bf86ace39fd80cd53ca5588edac7faebf-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8B%E5%8D%8810.39.28.png) 
      + 设当前元素为**代表**
        + Find(x)返回x所属集合的代表
        + Union(Find(x<sub>1</sub>),Find(x<sub>2</sub>))
          + **按秩合并两个不相交集合**
          + **求连通分量**
+ 求解
  + 并查集森林
    + 是一种**将每一个集合以树表示的数据结构**，**每一个节点保存着它的父节点引用地址**
    + **每个集合的代表即是集合的根节点**
    + **查找**
      + 根据父节点引用向上进行搜索直到root根节点
    + **联合**
      + 将两棵树合并到一起，即将其中一棵树的根连接到另一棵树的根的操作
    + **方法模板**
        ```javascript
        let MakeSet = (x) => {
            parent[x] = x;
        }
        let find = (x) => {
            if(parent[x] == x){
                return x;
            }
            return find(parent[x])
        }
        let union = (x,y) => {
            let rootX = find(x);
            let rootY = find(y);
            if(rootX == rootY){
                return;
            }
            parent[rootX] = rootY;
        }
        ``` 
    + 优化方法
      + **按秩合并**
        + ![截屏2019-11-21下午10.39.38.png](https://pic.leetcode-cn.com/ff5696fcb2fbadfbf42cbe6aeec6acf73618686b062ade33023c37720aa21b88-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8B%E5%8D%8810.39.38.png)
        + 这里的**秩**可以理解为**树的深度**
        + **总是将更小的树连接到更大的树上，即改变小根节点到大根节点上**
        + 初始化
          + 单元素大树秩定义为0
        + 当两棵秩相同时，均+1
        + 效率
          + 提升至O(logn)
        + **优化方法模板A**
          ```javascript
          let MakeSet = (x) => {
              parent[x] = x;
              rank[x] = 1;
          }
          let find = (x) => {
              if(parent[x] == x){
                  return x;
              }
              return find(parent[x])
          }
          let union = (x,y) => {
              let rootX = find(x);
              let rootY = find(y);
              if(rank[rootX] > rank[rootY]){
                  parent[rootY] = rootP;
              }else if(rank[rootX] < rank[rootY]){
                  parent[rootX] = rootY;
              }else{
                  parent[rootY] = rootX;
                  rank[rootX]++;
              }
          }
          ``` 
      + **路径压缩**
        + ![截屏2019-11-21下午10.39.44.png](https://pic.leetcode-cn.com/3273f7fc5f5e6e810fafa277d1babf07e259a61fb4fba7355edb06eede4e3eb8-%E6%88%AA%E5%B1%8F2019-11-21%E4%B8%8B%E5%8D%8810.39.44.png)
        + **处于同一路径上的节点都直接连接到根节点上**
        + **优化方法模板B**
        ```javascript
        let MakeSet = (x) => {
            parent[x] = x;
        }
        let find = (x) => {
            while(parent[x] != x){
               parent[x] = Find(parent[x]);
            }
            return parent[x];
        }
        let union = (x,y) => {
            let rootX = find(x);
            let rootY = find(y);
            if(rootX == rootY){
                return;
            }
            parent[rootX] = rootY;
        }
        ``` 
    + 总结：将以上两种优化方法合并，执行时间提升客观
    + 这两种方法的优势互补，同时使用二者的程序每个操作的平均时间仅为
    + $$O(\alpha (n))$$  
    + 实际上，这是渐近最优算法
  + 总结下包含**路径压缩**和**按秩合并**两种优化的最优代码模板来求解并查集问题
    ```javascript
    /**
     * @param {number[][]} M
     * @return {number}
     */
    var findCircleNum = function(M) {
        let n = M.length;
        if(n == 0){
            return 0;
        }
        // 求连通分量
        let count = n;
        // 给每个元素单集建立秩
        let rank = new Array(n);
        let find = (p) => {
            while( p != parent[p]){
                // 路径压缩
                // 区别与上述模版，很巧妙，通过直接将当前节点的父节点直接指向爷爷节点
                parent[p] = parent[parent[p]];
                p = parent[p];
            }
            return p;
        }
        // 查询两个相交集合
        let union = (p,q) => {
            let rootP = find(p);
            let rootQ = find(q);
            // 集合相交则不合并
            if(rootP == rootQ){
                return;
            }
            // 按秩合并
            if(rank[rootP] > rank[rootQ]){
                parent[rootQ] = rootP;
            }else if(rank[rootP] < rank[rootQ]){
                parent[rootP] = rootQ;
            }else{
                parent[rootQ] = rootP;
                // 相同的秩都加1
                rank[rootP]++;
            }
            // 求连通分量，每合并两个集合，即整体减少一个独立集合
            count--;
        }
        let parent = new Array(n);
        for(let i = 0;i < n;i++){
            parent[i] = i;
            // 初始化秩
            rank[i] = 1;
        }
        // 并查集搜索开始
        for(let i = 0;i < n;i++){
            for(let j = 0; j < n;j++){
                if(M[i][j] == 1){
                    union(i,j);
                }
            }
        }
        // 返回连通分量结果
        return count;
    };
    ```
  + 三要素
    + makeSet(s)：建立一个新的并查集，其中包含n个单元素集合
    + unionSet(x,y)：将元素x和元素y所在的集合合并
      + 若不相交则不合并
      + 若相交
        + 按秩合并为优化手段
        + 可用于求连通分量
    + find(x)：
      + 找到元素x所在的集合的代表
        + 路径压缩为优化手段
        + 可用于判断两个元素是否在同一个子集
+ 练习题型
  + [200. 岛屿数量-解法三](https://leetcode-cn.com/problems/number-of-islands/solution/200-dao-yu-shu-liang-by-alexer-660/)
  + [547. 朋友圈](https://leetcode-cn.com/problems/friend-circles/solution/547-peng-you-quan-by-alexer-660/) 


### 高级搜索
+ 减枝
  + 斐波那契数列问题
+ 双向BFS
  + 从过到右的BFS+从右到左的BFS
+ 启发式搜索（A<sup>*</sup>)
  + 估价函数
    + 启发式函数： h(n)，它用来评价哪些结点最有希望的是一个我们要找的结点，
    + h(n) 会返回一个非负实数,也可以认为是从结点n的目标结点路径的估计成本。
    + 启发式函数是一种告知搜索方向的方法。
    + 它提供了一种明智的方法来猜测哪个邻居结点会导向一个目标。
  + 基于 BFS 代码
    ```python
    def BFS(graph, start, end): 
        queue = [] 
        queue.append([start]) 
        visited.add(start) 
        while queue: 
            node = queue.pop() # can we add more intelligence here ? 
            visited.add(node) 

            process(node) 
            nodes = generate_related_nodes(node) 
            queue.push(nodes) 
    ``` 
    + A* search
    ```python
    def AstarSearch(graph, start, end): 
        pq = collections.priority_queue() # 优先级 —> 估价函数
        pq.append([start]) 
        visited.add(start) 
        while pq: 
            node = pq.pop() # can we add more intelligence here ? 
            visited.add(node) 

            process(node) 
            nodes = generate_related_nodes(node) 
            unvisited = [node for node in nodes if node not in visited] 
            pq.push(unvisited) 
    ``` 
 
### 高级树
+ 树
  + 父、子节点
  + 根节点、兄弟节点、叶子节点或叶节点
  + 高度、深度、层
+ 二叉树
  + 根节点、左子节点、右子节点
  + 满二叉树
    + 叶子节点全部在最底层
    + 除叶子节点外，每个节点都有左右两个子节点
  + 完全二叉树
    + 叶子节点都在最底下两层
    + 最后一层的叶子节点都靠左排列
    + 且除了最后一层，其它层的节点个数都要达到最大
  + 存储
    + 链式存储法
    + 顺序存储法
  + 遍历
    + 图解
      ![](https://static001.geekbang.org/resource/image/ab/16/ab103822e75b5b15c615b68560cb2416.jpg)
    + 模板
    ```python
    def preorder(self, root): 
        if root: 
            self.traverse_path.append(root.val) 
            self.preorder(root.left) 
            self.preorder(root.right) 
    def inorder(self, root): 
        if root: 
            self.inorder(root.left) 
            self.traverse_path.append(root.val) 
            self.inorder(root.right) 
    def postorder(self, root): 
        if root: 
            self.postorder(root.left) 
            self.postorder(root.right) 
            self.traverse_path.append(root.val) 
    ```
+ 二叉搜索树【Binary Search Tree】
  + 也叫有序二叉树、排序二叉树
    + 是一颗空树
    + 也是一颗这样的树
      + 1、左子树上所有子节点均小于它的根节点的值
      + 2、右子树上所有子节点均大于它的根节点的值
      + 3、左、右子树也分别为二叉查找树
  + 应用
    + 中序遍历即升序排列
+ AVL树
  + 平衡因子（Balance Factor)
    + 是它的左子树和右子树的高度差的绝对值不能超过1
    + balacnce factor = {-1,0,1}
  + 一颗平衡的二叉搜索树
    + 通过四种基础的旋转操作来进行平衡
      + 左旋【右右子树】
        ![截屏2019-11-24上午7.47.55.png](https://pic.leetcode-cn.com/353ad960fceda966aa5f5451886508656a3596a91aba8b513b733b5d87893474-%E6%88%AA%E5%B1%8F2019-11-24%E4%B8%8A%E5%8D%887.47.55.png) 
      + 右旋【左左子树】
        ![截屏2019-11-24上午7.48.05.png](https://pic.leetcode-cn.com/f4f3ae978d37dd2c1516f236926b7cc344e6a2abc260ce766705592c56ddc716-%E6%88%AA%E5%B1%8F2019-11-24%E4%B8%8A%E5%8D%887.48.05.png)
      + 左右旋【左右子树】
        + 左旋+右旋
        ![截屏2019-11-24上午7.48.28.png](https://pic.leetcode-cn.com/7b812f36c6f6a1927c0df8aeb6d20c4783425a6f95126024902ec77bdc834832-%E6%88%AA%E5%B1%8F2019-11-24%E4%B8%8A%E5%8D%887.48.28.png)
        ![截屏2019-11-24上午7.49.03.png](https://pic.leetcode-cn.com/558ad072576c0a19bf954fddd01b5559169ddc72fb8a3faa0feb49a3b485905d-%E6%88%AA%E5%B1%8F2019-11-24%E4%B8%8A%E5%8D%887.49.03.png)
      + 右左旋【右左子树】
        + 右旋+左旋
        ![截屏2019-11-24上午7.56.19.png](https://pic.leetcode-cn.com/ab2586bf844f05cbaea8e7a02599cccbe80361637d14d0fedf9993226c8c647b-%E6%88%AA%E5%B1%8F2019-11-24%E4%B8%8A%E5%8D%887.56.19.png)
        ![截屏2019-11-24上午7.56.26.png](https://pic.leetcode-cn.com/570a968f0793f10a9686926bb7bf5e7cc8de33d5aadd9656b9b7f4a946f152d1-%E6%88%AA%E5%B1%8F2019-11-24%E4%B8%8A%E5%8D%887.56.26.png)
    + 复杂示例
      ![截屏2019-11-24上午7.57.36.png](https://pic.leetcode-cn.com/4ebecdee91eef86540ae3603e515035e56c19b807e5efff5b37bacd6719aedf5-%E6%88%AA%E5%B1%8F2019-11-24%E4%B8%8A%E5%8D%887.57.36.png) 
    + 缺点
      + 结点需要存储额外信息、且调整次数频繁
+ 红黑树
  + Red-black Tree
  + 一种近似平衡的二叉搜索树(Binary Search Tree)
  + 性质
    + **每个结点的左右子树的高度差小于两倍**
      + ***从根到叶子的最长的可能路径不多于最短的可能路径的两倍长***
    + 根节点是黑色
    + 每个叶节点(这里指左右子节点的最后的空节点)是看不到的黑色
    + 两个相邻节点不能都是红色
    + 从任一节点到其每个叶子节点的所有路径都包含相同数目的黑色节点
+ AVL树和红黑树的区别
  + AVL trees provide faster lookups than Red Black Trees because they are more strictly balanced.
    + AVL树有更加快速的查询(lookups) ，因为它是严格按照平衡的(子树高度差绝对值为1)
  + Red Black Trees provide faster insertion and removal operations than AVL trees as fewer rotations are done due to relatively relaxed balancing.
    + 红黑树提供了更快的插入和删除操作，因为AVL旋转操作更多，红黑树相对少一点，是一颗近似平衡树
  + AVL trees store balance factors or heights with each node, thus requires storage for an integer per node whereas Red Black Tree requires only 1 bit of information per node.
    + AVL每个节点要存 factors 和 heights额外信息，需要更多内存，红黑树只要1个bit 0或1 表示红色还是黑色节点
  + Red Black Trees are used in most of the language libraries like map, multimap, multisetin C++ whereas AVL trees are used in databases where faster retrievals are required.
    + 读操作多，写入操作少时，用AVL
    + 写入操作大于等于一半时，用红黑树好

### 课后思考
+ [212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/solution/212-dan-ci-sou-suo-ii-by-alexer-660/)用Trie树实现的时间复杂度
  + 暴力法时间复杂度分析(看视频)
    + O(N*m*m*4<sup>k</sup>)
  + Trie树时间复杂度分析
    + 递归四连通判断：O(m * n * 4<sup>k</sup>)
    + 构建Trie树：O(n * k)
      + 外层遍历：O(n)
      + 内存单词字符循环建立子节点：O(k)
    + 合计：O(n * k + m * n * 4<sup>k</sup>)
  + 即为O(m * n * 4<sup>k</sup>)
+ 双端BFS模板总结
  + 单端模版
    ```python
    def BFS(graph, start, end):

        queue = [] 
        queue.append([start]) 

        while queue: 
            node = queue.pop() 
            visited.add(node)

            process(node) 
            nodes = generate_related_nodes(node) 
            queue.push(nodes)

        # other processing work 
        ...
    ``` 
  + 双端模版
    + Set 替换 Queue
    ```javascript
    let double_bfs = (start,end) => {
        let startSet = new Set();
        let endSet = new Set();
        startSet.add(start);
        endSet.add(end);
        while(startSet.size > 0){
          let next_startSet = new Set();
          for(let key of startSet){
              // process(node)
              let new_key = process(key);
              if(endSet.has(new_key)){
                  return something;
              }
              next_startSet.add(new_key);
          }
          startSet = next_startSet;
          if(startSet.size > endSet.size){
              [beginSet,endSet] = [endSet,beginSet]
          }
        }
    }
    ``` 