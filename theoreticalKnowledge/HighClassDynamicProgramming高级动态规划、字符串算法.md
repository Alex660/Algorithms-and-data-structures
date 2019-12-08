# 摘要
+ 高级动态规划
+ 字符串算法
+ 课后思考

### 高级动态规划
+ “Simplifying a complicated problem by breaking it down into simpler sub-problems”(in a recursive manner)
+ Divide & Conquer + Optimal substructure 
  + 分治 + 最优子结构
+ 顺推形式： 动态递推
+ 顺推模板
  ```javascript
  function DP(): 
    // ⼆维情况 
    dp = [][]
    for i = 0 .. M {
      for j = 0 .. N { 
        dp[i][j] = _Function(dp[i’][j’]…) 
      } 
    } 
  return dp[M][N]; 
  ``` 
+ 关键点
  + 有无最优的子结构
  + 找到重复子问题
  + 最优子结构、中途可以淘汰次优解
+ 实例
  + [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/solution/70-pa-lou-ti-by-alexer-660/)
  + [746. 使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/solution/746-shi-yong-zui-xiao-hua-fei-pa-lou-ti-by-alexer-/)
  + [爬楼梯问题变种：图文-详解](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/demos/climbing-stairs3.md)
  + [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/solution/62-bu-tong-lu-jing-by-alexer-660/)
  + [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/solution/63-bu-tong-lu-jing-ii-by-alexer-660/)
  + [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/solution/198-da-jia-jie-she-by-alexer-660/)
  + [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/solution/213-da-jia-jie-she-ii-by-alexer-660/)
  + [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/solution/64-zui-xiao-lu-jing-he-by-alexer-660/)
  + [编辑距离](https://leetcode-cn.com/problems/edit-distance/solution/72-bian-ji-ju-chi-by-alexer-660/)
  + 股票6道
    + 1、[121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
    + 2、[122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
    + 3、[123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/submissions/)
    + 4、[309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/submissions/)
    + 5、[188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/submissions/)
    + 6、[714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/submissions/)

### 字符串算法
+ 最长子序列
  + [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/solution/1143-zui-chang-gong-gong-zi-xu-lie-by-alexer-660/)
  + s1[i-1] == s2[j-1] && dp[i][j] = dp[i-1][j-1] + 1
  + s1[i-1] != s2[j-1] && dp[i][j] = Max(dp[i-1][j],dp[i][j-1])
+ 最长子串
  + 最长子序列的变体
    + 即子序列必须在原字符串中连续
  + s1[i-1] == s2[j-1] && dp[i][j] = dp[i-1][j-1] + 1
  + s1[i-1] != s2[j-1] && dp[i][j] = 0
+ 编辑距离
  + [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/solution/72-bian-ji-ju-chi-by-alexer-660/)
    + s1[i-1] == s2[j-1] && dp[i][j] = dp[i-1][j-1]
    + s1[i-1] != s2[j-1] && dp[i][j] = Min(dp[i-1][j],dp[i][j-1],dp[i-1][j-1]+1)
+ 最长回文串
  + [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/solution/5-zui-chang-hui-wen-zi-chuan-by-alexer-660/)
  + dp[i][j] = s[i] == s[j] && (j - i < 2 || dp[i+1][j-1])
+ 字符串匹配算法KMP
  + 暴力法（brute force） - O(mn)
  + Rabin-Karp 算法
  + KMP 算法
    + KMP算法（Knuth-Morris-Pratt）的思想就是，当子串与目标字符串不匹配时，
    + 其实你已经知道了前面已经匹配成功那 一部分的字符（包括子串与目标字符串）。
    + 以阮一峰的文章为例，当空格与 D 不匹配时，你其实 知道前面六个字符是“ABCDAB”。
    + KMP 算法的想法是，设法利用这个已知信息，不要把“搜索位置” 移回已经比较过的位置，继续把它向后移，这样就提高了效率。 
  + 暴力法
    ```java
    public static int forceSearch(String txt, String pat) {
      int M = txt.length();
      int N = pat.length();
      for (int i = 0; i <= M - N; i++) {
        int j;
        for (j = 0; j < N; j++) {
          if (txt.charAt(i + j) != pat.charAt(j))
            break;
        } 
        if (j == N) {
          return i;
        } 
        // 更加聪明？
        // 1. 预先判断– hash(txt.substring(i, M)) == hash(pat)
        // 2. KMP 
      } 
      return -1;
    }
    ``` 
  + 文献
    + [文献A](https://www.bilibili.com/video/av11866460?from=search&seid=17425875345653862171)
    + [文献B](http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)
  + 学习资料
    + [Boyer-Moore 算法](https://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html)
      + 在朴素算法中，我们需要挨个比较所有字符，才知道目标字符串中是否包含子串。
      + 那么， 是否有别的方法可以用来判断目标字符串是否包含子串呢？
        + 答案是肯定的，确实存在一种更快的方法。为了避免挨个字符对目标字符串和子串进行比较， 
        + 我们可以尝试一次性判断两者是否相等。因此，我们需要一个好的哈希函数（hash function）。 
        + 通过哈希函数，我们可以算出子串的哈希值，然后将它和目标字符串中的子串的哈希值进行比较。 
        + 这个新方法在速度上比暴力法有显著提升。
      + 核心思想
        1. 假设子串的长度为 M (pat)，目标字符串的长度为 N (txt)
        2. 计算子串的 hash 值 has_pat
        3. 计算目标字符串txt中每个长度为 M 的子串的 hash 值（共需要计算 N-M+1次）
        4. 比较 hash 值：如果 hash 值不同，字符串必然不匹配; 如果 hash 值相同，还需要使用朴素算法再次判断
        > KMP算法（Knuth-Morris-Pratt）的思想就是，当子串与目标字符串不匹配时，其实你已经知道了前面已经匹配成功那 一部分的字符（包括子串与目标字符串）。  
        以阮一峰的文章为例，当空格与 D 不匹配时，你其实 知道前面六个字符是“ABCDAB”。  
        KMP 算法的想法是，设法利用这个已知信息，不要把“搜索位置” 移回已经比较过的位置，继续把它向后移，这样就提高了效率。  
      + code
        ```java
        public final static int D = 256;
        public final static int Q = 9997;
        static int RabinKarpSerach(String txt, String pat) {
          int M = pat.length();
          int N = txt.length();
          int i, j;
          int patHash = 0, txtHash = 0;
          for (i = 0; i < M; i++) {
            patHash = (D * patHash + pat.charAt(i)) % Q;
            txtHash = (D * txtHash + txt.charAt(i)) % Q;
          }
          int highestPow = 1; // pow(256, M-1)
          for (i = 0; i < M - 1; i++) 
            highestPow = (highestPow * D) % Q;
          for (i = 0; i <= N - M; i++) {
            // 枚举起点 
            if (patHash == txtHash) {
              for (j = 0; j < M; j++) {
                if (txt.charAt(i + j) != pat.charAt(j))
                break;
              } 
              if (j == M)
                return i;
            } 
            if (i < N - M) {
              txtHash = (D * (txtHash - txt.charAt(i) * highestPow) + txt.charAt(i + M)) % Q;
              if (txtHash < 0)
                txtHash += Q;
            } 
          }
          return -1;
        }
        ```  
    + [Sunday 算法](https://blog.csdn.net/u012505432/article/details/52210975)

### 课后思考
+ [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/solution/62-bu-tong-lu-jing-by-alexer-660/)
  + 状态定义
    + dp[i][j]：来到当前格子的最短路径总数
  + 状态转移方程
    + **dp[i][j] = dp[i-1][j] + dp[i][j-1]**
+ [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/solution/63-bu-tong-lu-jing-ii-by-alexer-660/)
  + 状态定义
    + dp[i][j] == 0 ，此路不通
    + dp[i][j] != 0 ，最短路径总数
  + 状态转移方程
    + **obstacleGrid[i][j] != 1 && dp[i][j] = dp[i-1][j] + dp[i][j-1]**
