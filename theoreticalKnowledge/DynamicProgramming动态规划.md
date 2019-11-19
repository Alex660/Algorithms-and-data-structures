# 摘要
+ 动态规划
  + 分治、回溯、递归
  + demos
  + 解法总结
  + 课后思考


### 模板
+ 递归
  ```java
  public void recur(int level,int param){
      // terminator
      if(level > MAX_LEVEL){
          // process result
          return;
      }
      // process current logic
      process(level,param);
      // drill down
      recur(level:level+1,newParam);
  }
  ``` 
+ 分治 Divide & Conquer
  ```python
  def divide_conquer(problem,param1,param2,...):
    # recursion terminator
    if problem is None:
      print_result
      return
    # prepare data
    data = prepare_data(problem)
    subproblems = split_problem(problem,data)
    # conquer subproblems
    subresult1 = self.divide_conquer(subproblems[0],p1,...)
    subresult2 = self.divied_conquer(Subproblems[1],p1,...) 
    subresult3 = self.divied_conquer(Subproblems[2],p1,...) 
    ""
    # process and generate the final result
    result = process_result(subresult1,subresult2,subresult3,...)
    # revert the current level states
  ```
### 差异
+ 动态规划 和 递归或者分治 没有根本上的区别
  + 关键是看有无最优的子结构
+ 共性
  + 找到重复子问题
+ 差异性
  + 最优子结构、中途可以被淘汰次优解
### dp关键点
1. 最优子结构
   1. opt[n] = best_of(opt[n-1],opt[n-2],...)
2. 储存中间状态
   1. opt[i]    
3. 递推公式
   1. Fib: opt[n] = opt[n-1]+opt[n-2]
   2. 二维路径：opt[i,j] = opt[i+1][j] + opt[i][j+1]
      1. 需判断a[i,j]是否可达，如障碍物  
### demos
+ 斐波那契数列
  ```java
  int fib(int n){
      if(n <= 1){
          return n;
      }
      return fib(n-1) + fib(n-2);
  }
  int fib(int n){
      return n <= 1 ? n : fib(n-1) + fib(n-2)
  }
  int fib(int n,int [] meno){
      if(n <= 1){
          return n;
      }
      if(memo[n] == 0){
          meno = fib(n-1) + fib(n-2);
      }
      return memo[n];
  }
  // Bottom Up
  F[n] = F[n-1]+F[n-2]
  a[0] = 0,a[1] = 1;
  for(int i = 2;i <= n;i++){
      a[i] = a[i-1] + a[i-2]
  }
  a[n]
  ```  
+ 路径计数
  + 图解
![截屏2019-11-11上午9.34.08.png](https://pic.leetcode-cn.com/36282f3fc806f78063ad164b3bc633d7784e538ac5c9e282735f4701fca3392b-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8A%E5%8D%889.34.08.png)
![截屏2019-11-11上午9.35.49.png](https://pic.leetcode-cn.com/f1941ffefdddca21f8559002a0054ca432708941bf83a0305bc2776254668dc6-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8A%E5%8D%889.35.49.png)
  ```java
  int countParts(boolean[][]grid,int row,int col){
      if(!validSquare(grid,row,col)) return 0;
      if(isAtEnd(grid,row,col)) return 1;
      return countPaths(grid,row+1,col) + countPaths(grid,row,col+1);
  }
  ```  
![截屏2019-11-11上午9.36.09.png](https://pic.leetcode-cn.com/5d80f69d57d14c0f0fcc20118ac16a5a57900a96e20635719c1c2361d4f92b93-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8A%E5%8D%889.36.09.png)
![截屏2019-11-11上午9.36.20.png](https://pic.leetcode-cn.com/6d0721dd19897a4c59b69dad0c3873742e33034b0ffc626d0af806a0eb2a4038-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8A%E5%8D%889.36.20.png)
![截屏2019-11-11上午9.36.31.png](https://pic.leetcode-cn.com/97525ed14006d07ed2e8a95eebc130f15906ebe6076c424872de678193260846-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8A%E5%8D%889.36.31.png)
![截屏2019-11-11上午9.36.39.png](https://pic.leetcode-cn.com/bfa9912c6be099ceccfc95f2c02830be2578eec0d406b11c796eea6e697a090d-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8A%E5%8D%889.36.39.png)  
  + 求状态转移方程(DP方程) 
    + opt[i,j] = opt[i+1,j] + opt[i,j+1]
    + if a[i,j] = '空地';
      +  opt[i,j] = opt[i+1,j]+opt[i,j+1]
    + else
      + opt[i,j] = 0  
+ 其它dp题型
  + [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/solution/62-bu-tong-lu-jing-by-alexer-660/) 
  + [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/solution/63-bu-tong-lu-jing-ii-by-alexer-660/)
  + [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/solution/70-pa-lou-ti-by-alexer-660/)
### 解法总结
+ 1、define subproblems
  + 进行分治：总问题分解为子问题
+ 2、guess(part of solution)
  + 猜递归方程形式
+ 3、relate subproblem solutions
  + 将n个子问题的解联系起来 <=> 合并子问题的解
+ 4、recurse && memorize 或者 build DP table bottom-up
  + 递归+备忘录搜索 或者 建立一个自底向上的表来递推
#### 课后思考
+ [爬楼梯问题变种：图文-详解](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/demos/climbing-stairs3.md)