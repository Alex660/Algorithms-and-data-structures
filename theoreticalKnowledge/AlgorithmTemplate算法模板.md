#### 二叉树
+ [演示demo](https://visualgo.net/zh/bst?slide=1)
  
#### 递归
+ Python 
    ```Python
    def recursion(level, param1, param2, ...): 
        # recursion terminator 
        if level > MAX_LEVEL: 
        process_result 
        return 

        # process logic in current level 
        process(level, data...) 

        # drill down 
        self.recursion(level + 1, p1, ...) 

        # reverse the current level status if needed
    ```
+ Java
    ```Java
    public void recur(int level, int param) { 

    // terminator 
    if (level > MAX_LEVEL) { 
        // process result 
        return; 
    } 

    // process current logic 
    process(level, param); 

    // drill down 
    recur( level: level + 1, newParam); 

    // restore current status 
    
    }
    ``` 

#### 分治
+ Python
    ```Python
    def divide_conquer(problem, param1, param2, ...): 
    # recursion terminator 
    if problem is None: 
        print_result 
        return 

    # prepare data 
    data = prepare_data(problem) 
    subproblems = split_problem(problem, data) 

    # conquer subproblems 
    subresult1 = self.divide_conquer(subproblems[0], p1, ...) 
    subresult2 = self.divide_conquer(subproblems[1], p1, ...) 
    subresult3 = self.divide_conquer(subproblems[2], p1, ...) 
    …

    # process and generate the final result 
    result = process_result(subresult1, subresult2, subresult3, …)
        
    # revert the current level states
    ```

#### 深度优先搜索、广度优先搜索
+ DFS
  + Python 
    + 递归写法
      ```Python
        visited = set() 

        def dfs(node, visited):
            if node in visited: # terminator
                # already visited 
                return 

            visited.add(node) 

            # process current node here. 
            ...
            for next_node in node.children(): 
                if next_node not in visited: 
                    dfs(next_node, visited)
      ```
    + 非递归写法
      ```Python
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
+ BFS
  + Python
    + 非递归写法
        ```Python
        def BFS(graph, start, end):
            visited = set()
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

#### 二分查找
+ [F扩展阅读](https://www.beyond3d.com/content/articles/8/)
+ 二分模版
    ```Python
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
#### 二分查找变体
+ [参考文章 - 二分查找算法细节详解](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/solution/er-fen-cha-zhao-suan-fa-xi-jie-xiang-jie-by-labula/)
+ 寻找数组中一个数的最左边出现的位置和最右边出现的位置
  + **寻找左侧边界的二分查找**
    + 模板
    ```javascript
      let leftBound = (array,target) => {
          let left = 0;
          let right = array.length;
          while(left < right){
              let mid = (left + right) >> 1;
              // 缩小「搜索区间」的上界 right，在区间 [left, mid) 中继续搜索
              // 即不断向左收缩，达到锁定左侧边界的目的。
              if(array[mid] == target){
                  right = mid;
              }else if(array[mid] < target){
                  left = mid + 1;
              }else if(array[mid] > target){
                  right = mid;
              }
          }
          return left;
      }
    ``` 
  + **寻找右侧边界的二分查找**
    + 模板
    ```javascript
      let rightBound = (array,target) => {
          let left = 0;
          let right = array.length;
          while(left < right){
              let mid = (left + right) >> 1;
              // 增大「搜索区间」的下界 left，使得区间不断向右收缩，
              // 达到锁定右侧边界的目的。
              if(array[mid] == target){
                  // mid = left - 1
                  left = mid + 1; 
              }else if(array[mid] < target){
                  left = mid + 1;
              }else if(array[mid] > target){
                  right = mid;
              }
          }
          // 对 left 的更新必须是 left = mid + 1
          // while 循环结束时，nums[left] 一定不等于 target ，
          // 而 nums[left-1] 可能是 target。
          return left - 1;
      }
    ``` 

#### Trie树
+ Python
    ```Python
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
+ Java
    ```Java
    class TrieNode {

        // R links to node children
        private TrieNode[] links;

        private final int R = 26;

        private boolean isEnd;

        public TrieNode() {
            links = new TrieNode[R];
        }

        public boolean containsKey(char ch) {
            return links[ch -'a'] != null;
        }
        public TrieNode get(char ch) {
            return links[ch -'a'];
        }
        public void put(char ch, TrieNode node) {
            links[ch -'a'] = node;
        }
        public void setEnd() {
            isEnd = true;
        }
        public boolean isEnd() {
            return isEnd;
        }
    }
    class Trie {
        private TrieNode root;

        public Trie() {
            root = new TrieNode();
        }

        // Inserts a word into the trie.
        public void insert(String word) {
            TrieNode node = root;
            for (int i = 0; i < word.length(); i++) {
                char currentChar = word.charAt(i);
                if (!node.containsKey(currentChar)) {
                    node.put(currentChar, new TrieNode());
                }
                node = node.get(currentChar);
            }
            node.setEnd();
        }
        // search a prefix or whole key in trie and
        // returns the node where search ends
        private TrieNode searchPrefix(String word) {
            TrieNode node = root;
            for (int i = 0; i < word.length(); i++) {
            char curLetter = word.charAt(i);
            if (node.containsKey(curLetter)) {
                node = node.get(curLetter);
            } else {
                return null;
            }
            }
            return node;
        }

        // Returns if the word is in the trie.
        public boolean search(String word) {
        TrieNode node = searchPrefix(word);
        return node != null && node.isEnd();
        }
        // Returns if there is any word in the trie
        // that starts with the given prefix.
        public boolean startsWith(String prefix) {
            TrieNode node = searchPrefix(prefix);
            return node != null;
        }
    }
    ```
+ Javascript
    ```javascript
    // 字典树节点
    class TreeNode{
        constructor(){
            // 是不是最后一个节点
            this.END = false;
            // 所有的儿子节点
            this.links = new Array(26);  
        }
        containsKey(letter) {
            return this.links[letter.charCodeAt()-97] != undefined;
        }
        getCh(letter){
            return this.links[letter.charCodeAt()-97];
        }
        putCh(letter,newTrieNode){
            this.links[letter.charCodeAt()-97] = newTrieNode;
        }
        setEnd(){
            this.END = true;
        }
        isEnd(){
            return this.END;
        }
    }
    let root = null;
    /**
    * Initialize your data structure here.
    */
    // 初始化字典树
    var Trie = function() {
        root = new TreeNode();
    };

    /**
    * Inserts a word into the trie. 
    * @param {string} word
    * @return {void}
    */
    // 搜索单词字符链接是否存在于字典树组成的链接中
    let searchPrefix = (word) => {
        // 从根节点开始
        let currNode = root;
        for(let i = 0;i < word.length;i++){
            // 当前单词当前字符存在字典树正常向下遍历的链接中
            if(currNode.containsKey(word[i])){
                // 存在则继续向下
                currNode = currNode.getCh(word[i]);
            }else{
                // 不存在
                return null;
            }
        }
        // 单词遍历完，均在字典树中，说明单词字符链接至少是字典树中的前缀，
        // 或者是一条完整的链接即单词存在
        return currNode;
    }
    // 建立字典树
    // 在自字典树中插入一个单词
    Trie.prototype.insert = function(word) {
        let currNode = root;
        for(let i = 0;i < word.length;i++){
            // 当前节点没有儿子节点，则创建当前节点作为新的父节点，
            // 并创建一个新的TreeNode子节点与其相连
            if(!currNode.containsKey(word[i])){
                currNode.putCh(word[i],new TreeNode());
            }
            // 当前节点存在，意即当前单词中的当前字符存在于字典树中，
            // 则继续搜索下一个链接，即子节点
            currNode = currNode.getCh(word[i]);
        }
        // 直到搜索完毕，当前节点标记为结束节点
        currNode.setEnd();
    };

    /**
    * Returns if the word is in the trie. 
    * @param {string} word
    * @return {boolean}
    */
    // 在字典树中查找一个完全匹配的单词
    Trie.prototype.search = function(word) {
        // 将查找单词字符链接函数定义成一个函数，后面搜索前缀的可以复用
        let currNode = searchPrefix(word);
        // 如果单词至少是前缀，且字典树中也到了当前一条叉的尾部，
        // 则说明是完整的单词存在于字典树，
        // 否则最多是前缀即下面的stratsWith搜索
        return currNode != null && currNode.isEnd();
    };

    /**
    * Returns if there is any word in the trie that starts with the given prefix. 
    * @param {string} prefix
    * @return {boolean}
    */
    // 分析同search操作
    Trie.prototype.startsWith = function(prefix) {
        let currNode = searchPrefix(prefix);
        return currNode != null;
    };

    /** 
    * Your Trie object will be instantiated and called as such:
    * var obj = new Trie()
    * obj.insert(word)
    * var param_2 = obj.search(word)
    * var param_3 = obj.startsWith(prefix)
    */
    ```
    + 或者
    ```javascript
    class TrieNode{
        constructor(){
            this.END = false;
            this.children = {};
        }
    }
    let root = null;
    /**
    * Initialize your data structure here.
    */
    var Trie = function() {
        root  = new TrieNode();
    };

    /**
    * Inserts a word into the trie. 
    * @param {string} word
    * @return {void}
    */
    Trie.prototype.insert = function(word) {
        let currNode = root;
        for(let i = 0;i < word.length;i++){
            if(currNode.children[word[i]] == undefined){
                currNode.children[word[i]] = new TrieNode();
            }
            currNode = currNode.children[word[i]];
        }
        currNode.END = true;
    };
    let searchPrefix = (word) => {
        let currNode = root;
        for(let i = 0;i < word.length;i++){
            if(currNode.children[word[i]]){
                currNode = currNode.children[word[i]];
            }else{
                return null;
            }
        }
        return currNode;
    }
    /**
    * Returns if the word is in the trie. 
    * @param {string} word
    * @return {boolean}
    */
    Trie.prototype.search = function(word) {
        let currNode = searchPrefix(word);
        return currNode != null && currNode.END;
    };

    /**
    * Returns if there is any word in the trie that starts with the given prefix. 
    * @param {string} prefix
    * @return {boolean}
    */
    Trie.prototype.startsWith = function(prefix) {
        return searchPrefix(prefix) != null;
    };

    /** 
    * Your Trie object will be instantiated and called as such:
    * var obj = new Trie()
    * obj.insert(word)
    * var param_2 = obj.search(word)
    * var param_3 = obj.startsWith(prefix)
    */
    ```

#### 并查集
+ Java
    ```java
    class UnionFind { 
        private int count = 0; 
        private int[] parent; 
        public UnionFind(int n) { 
            count = n; 
            parent = new int[n]; 
            for (int i = 0; i < n; i++) { 
                parent[i] = i;
            }
        } 
        public int find(int p) { 
            while (p != parent[p]) { 
                parent[p] = parent[parent[p]]; 
                p = parent[p]; 
            }
            return p; 
        }
        public void union(int p, int q) { 
            int rootP = find(p); 
            int rootQ = find(q); 
            if (rootP == rootQ) return; 
            parent[rootP] = rootQ; 
            count--;
        }
    }
    ```
+ Python
    ```Python
    def init(p): 
        # for i = 0 .. n: p[i] = i; 
        p = [i for i in range(n)] 
    
    def union(self, p, i, j): 
        p1 = self.parent(p, i) 
        p2 = self.parent(p, j) 
        p[p1] = p2 
    
    def parent(self, p, i): 
        root = i 
        while p[root] != root: 
            root = p[root] 
        while p[i] != i: # 路径压缩 ?
            x = i; i = p[i]; p[x] = root 
        return root
    ```
+ Javascript
  + [547. 朋友圈-解法三、四](https://leetcode-cn.com/problems/friend-circles/solution/547-peng-you-quan-by-alexer-660/)
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

#### 双向BFS
+ Javascript
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

#### A* 
+ Python
    ```Python
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
+ 解法参考
  + [二进制矩阵中的最短路径的 A* 解法](https://leetcode.com/problems/shortest-path-in-binary-matrix/discuss/313347/A*-search-in-Python)
  + [8 puzzles 解法比较](https://dataaspirant.com/2015/04/11/five-most-popular-similarity-measures-implementation-in-python/)
+ 文献
  + [相似度测量方法](https://zxi.mytechroad.com/blog/searching/8-puzzles-bidirectional-astar-vs-bidirectional-bfs/)

#### 二叉树遍历
+ 前序(Pre-order)：根-左-右
    ```Python
    def preorder(self, root): 
        if root: 
            self.traverse_path.append(root.val) 
            self.preorder(root.left) 
            self.preorder(root.right) 
    ```
+ 中序(In-order)：左-根-右
    ```Python
    def preorder(self, root): 
        if root: 
            self.preorder(root.left) 
            self.traverse_path.append(root.val) 
            self.preorder(root.right) 
    ```
+ 后序(Post-order)：左-右-根
    ```Python
    def preorder(self, root): 
        if root: 
            self.preorder(root.left) 
            self.preorder(root.right) 
            self.traverse_path.append(root.val) 
    ```

#### 布隆过滤器
+ Python
    ```Python
    from bitarray import bitarray 
    import mmh3 

    class BloomFilter: 
        def __init__(self, size, hash_num): 
            self.size = size 
            self.hash_num = hash_num 
            self.bit_array = bitarray(size) 
            self.bit_array.setall(0) 

        def add(self, s): 
            for seed in range(self.hash_num): 
                result = mmh3.hash(s, seed) % self.size 
                self.bit_array[result] = 1 

        def lookup(self, s): 
            for seed in range(self.hash_num): 
                result = mmh3.hash(s, seed) % self.size 
                if self.bit_array[result] == 0: 
                    return "Nope" 
            return "Probably" 

    bf = BloomFilter(500000, 7) 
    bf.add("dantezhao") 
    print (bf.lookup("dantezhao")) 
    print (bf.lookup("yyj")) 
    ```

#### LRU缓存
+ Python
    ```Python
    class LRUCache(object): 

        def __init__(self, capacity): 
            self.dic = collections.OrderedDict() 
            self.remain = capacity

        def get(self, key): 
            if key not in self.dic: 
                return -1 
            v = self.dic.pop(key) 
            self.dic[key] = v   # key as the newest one 
            return v 

        def put(self, key, value): 
            if key in self.dic: 
                self.dic.pop(key) 
            else: 
                if self.remain > 0: 
                    self.remain -= 1 
                else:   # self.dic is full
                    self.dic.popitem(last=False) 
            self.dic[key] = value
    ```
+ Javascript
    ```javascript
    实现哈希表
    实现双链表
    let LRUCache = function(capacity) {
        初始化容量，用于判断LRU是否已满
        初始化哈希表映射，用于查找节点
        初始化双链表，用于插入、删除节点
    };
    LRUCache.prototype.get = function(key) {
        if(哈希查找 不存在){
            return -1;
        }else{
            将此节点置于开头，为最近访问节点
            return 节点值
        }
    }
    LRUCache.prototype.put = function(key, value) {
        if(哈希查找 已经存在){
            删除旧的数据
            
        }else{
            if(LRU 即 双链表已满){
                删除尾节点
                更新哈希映射值，并将更新后的节点置于开头
            }
            将新节点置于开头
            新建新节点的哈希映射
        }
    }

    // 写法一：
    // 链表节点
    class Node{
        constructor(key,val){
            this.key = key;
            this.val = val;
        }
    }
    // 双链表
    class DoubleList{
        // 初始化头、尾节点、链表最大容量
        constructor(){
            this.head = new Node(0,0);
            this.tail = new Node(0,0);
            this.size = 0;
            this.head.next = this.tail;
            this.tail.prev = this.head;
        }
        // 在链表头部添加节点
        addFirst(node){
            node.next = this.head.next;
            node.prev = this.head;
            this.head.next.prev = node;
            this.head.next = node;
            this.size++;
        }
        // 删除链表中存在的node节点
        remove(node){
            node.prev.next = node.next;
            node.next.prev = node.prev;
            this.size--;
        }
        // 删除链表中最后一个节点，并返回该节点
        removeLast(){
            // 链表为空
            if(this.tail.prev == this.head){
                return null;
            }
            let last = this.tail.prev;
            this.remove(last);
            return last;
        }
    }
    /**
    * @param {number} capacity
    */
    var LRUCache = function(capacity) {
        this.cap = capacity;
        this.map = new Map();
        this.cache = new DoubleList();
    };

    /** 
    * @param {number} key
    * @return {number}
    */
    LRUCache.prototype.get = function(key) {
        let map = this.map;
        if(!map.has(key)){
            return -1;
        }
        let val = map.get(key).val;
        // 最近访问数据置前
        this.put(key,val);
        return val;
    };

    /** 
    * @param {number} key 
    * @param {number} value
    * @return {void}
    */
    LRUCache.prototype.put = function(key, value) {
        let cache = this.cache;
        let map = this.map;
        let node = new Node(key,value);
        if(map.has(key)){
            // 删除旧的节点，新的插到头部
            cache.remove(map.get(key));
        }else{
            if(this.cap == cache.size){
                // 删除最后一个
                let last = cache.removeLast();
                map.delete(last.key);
            }
        }
        // 新增头部
        cache.addFirst(node);
        // 更新 map 映射
        map.set(key,node);
    };

    /** 
    * Your LRUCache object will be instantiated and called as such:
    * var obj = new LRUCache(capacity)
    * var param_1 = obj.get(key)
    * obj.put(key,value)
    */

    // 写法二
    /**
     * @param {number} capacity
     */
    var LRUCache = function(capacity) {
        this.cap = capacity;
        this.cache = new Map();
    };

    /** 
     * @param {number} key
     * @return {number}
     */
    LRUCache.prototype.get = function(key) {
        let cache = this.cache;
        if(cache.has(key)){
            let val = cache.get(key);
            cache.delete(key);
            cache.set(key,val);
            return val;
        }else{
            return -1;
        }
    };

    /** 
     * @param {number} key 
     * @param {number} value
     * @return {void}
     */
    LRUCache.prototype.put = function(key, value) {
        let cache = this.cache;   
        if(cache.has(key)){
            cache.delete(key);
        }else{
            if(cache.size == this.cap){
                cache.delete(cache.keys().next().value);
            }
        }
        cache.set(key,value);
    };

    /** 
     * Your LRUCache object will be instantiated and called as such:
     * var obj = new LRUCache(capacity)
     * var param_1 = obj.get(key)
     * obj.put(key,value)
     */
    ```

#### 快速排序
+ Java
    ```java
    public static void quickSort(int[] array, int begin, int end) {
        if (end <= begin) return;
        int pivot = partition(array, begin, end);
        quickSort(array, begin, pivot - 1);
        quickSort(array, pivot + 1, end);
    }

    static int partition(int[] a, int begin, int end) {
        // pivot: 标杆位置，counter: 小于pivot的元素的个数
        int pivot = end, counter = begin;
        for (int i = begin; i < end; i++) {
            if (a[i] < a[pivot]) {
                int temp = a[counter]; a[counter] = a[i]; a[i] = temp;
                counter++;
            }
        }
        int temp = a[pivot]; a[pivot] = a[counter]; a[counter] = temp;
        return counter;
    }
    ```

#### 归并排序
+ Java
    ```java
    public static void mergeSort(int[] array, int left, int right) {
        if (right <= left) return;
        int mid = (left + right) >> 1; // (left + right) / 2

        mergeSort(array, left, mid);
        mergeSort(array, mid + 1, right);
        merge(array, left, mid, right);
    }

    public static void merge(int[] arr, int left, int mid, int right) {
            int[] temp = new int[right - left + 1]; // 中间数组
            int i = left, j = mid + 1, k = 0;

            while (i <= mid && j <= right) {
                temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
            }

            while (i <= mid)   temp[k++] = arr[i++];
            while (j <= right) temp[k++] = arr[j++];

            for (int p = 0; p < temp.length; p++) {
                arr[left + p] = temp[p];
            }
            // 也可以用 System.arraycopy(a, start1, b, start2, length)
        }
    ```

#### 堆排序
+ Java
    ```Java
    static void heapify(int[] array, int length, int i) {
        int left = 2 * i + 1, right = 2 * i + 2；
        int largest = i;

        if (left < length && array[left] > array[largest]) {
            largest = left;
        }
        if (right < length && array[right] > array[largest]) {
            largest = right;
        }

        if (largest != i) {
            int temp = array[i]; array[i] = array[largest]; array[largest] = temp;
            heapify(array, length, largest);
        }
    }

    public static void heapSort(int[] array) {
        if (array.length == 0) return;

        int length = array.length;
        for (int i = length / 2-1; i >= 0; i-) 
            heapify(array, length, i);

        for (int i = length - 1; i >= 0; i--) {
            int temp = array[0]; array[0] = array[i]; array[i] = temp;
            heapify(array, i, 0);
        }
    }
    ```
+ C++
    ```C++
    void heap_sort(int a[], int len) {

        priority_queue<int,vector<int>,greater<int> > q;
        
        for(int i = 0; i < len; i++) {
            q.push(a[i]);
        }

        for(int i = 0; i < len; i++) {
            a[i] = q.pop();
        }
    }
    ```

#### 十大经典排序算法及其动图展示
+ [参考文献](https://www.cnblogs.com/onepixel/p/7674659.html)
+ [总结 - 排序算法](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md#%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95)