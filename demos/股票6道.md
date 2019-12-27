## ***The stock issue With Javascript***

### 动态规划：***一法破万法*** 之 歼灭6道股票问题
#### 参考文章：[一个通用方法团灭 6 道股票问题](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)
#### 1、[121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
#### 2、[122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
#### 3、[123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/submissions/)
#### 4、[309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/submissions/)
#### 5、[188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/submissions/)
#### 6、[714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/submissions/)
#### 寻找共同特征
| 题号 |    交易日期/i  |  交易次数/k  |   操作   |  限制区别  |
| ---  |    -----:    |    :----:   |   -----:|  -----:   |
| 1    |      any     |     1       |buy/sell |    ——     |
| **2**|      any     |  +infinity  |buy/sell |    ——     |
| 3    |      any     |     2       |buy/sell |    ——     |
| **4**|**sell之后要等一天**|  +infinity  |buy/sell |    ——     |
| 5    |      any     |  k = any    |buy/sell |    ——     |
| **6**|      any     |  +infinity  |buy/sell |**每次交易去除手续费**|
#### 表格分析
+ +infinity为正无穷，代表当题不限制交易次数，可以进行n笔交易
+ 交易日期：sell之后要等一天
  + 为当题中的交易冷冻期
  + 意即，今天买入buy，那么昨天就不能卖出sell，只能前天sell
+ 交易次数
  + **不影响状态转移方程**
    + +infinity
      + 当题不限制交易次数，不影响交易利润
        + 例如我交易k次或者k+1次或者k+n次都可以，只要我能达到那个相对固定的最大利润值即可
        + 类似于爬楼梯、斐波那契数列问题
          + 无论你是爬一步或者每次两步上来都无所谓，能到达终点即可
      + 相当于0次或1次
    + 1
      + 无论是躺着还是睡着交易都是1次，可以多次买卖一支股票
      + 相当于0次
  + **影响状态转移方程**
    + 2
      + 交易1次和交易2次对于股票交易之利润的获取，肯定是不同的
      + 和交易次数为k次且k < n/2 情况相同
    + k
      + 一次交易 = 买入 + 卖出，至少需要2天
      + prices数组的长度即为最大交易天数n
        + 当 k < n/2 时
          + 为有效限制，即和上述交易限制次数为2的解法一样，只不过换成了动态数据
        + 当 k > n/2 时
          + 相当于 其它几题的可以多次买卖一支股票
          + 因而，此种情况是**不影响状态转移方程**的，所以解法也就回到了上面 k = +infinity 的情况，如出一辙
          + 又因为是**不影响状态转移方程**的，所以解法跟k = 0 或者 k = 1 次一样，是殊途同归的
+ 限制区别：每次交易去除手续费
  + 每一笔交易都要从获得利润中减去
  + 等价于
    + 买入股票时，价格升高了**或者**
    + 卖出股票时，价格降低了
#### 归纳总结
+ **状态定义**
  + 三要素
    + 1、交易日期：i
    + 2、交易次数：k
      + 每一次buy，交易次数必然减一：k-1
    + 3、交易操作：buy/sell  <=> 持有/抛售
      + sell：利润中加prices[i]
      + buy：利润中减去prices[i]
  + dp[i][k][0,1]：三维DP
    + **dp[i][k][0]**：第i天本人至今最多交易了k次，并且手上**没有持有**股票
      + 之前(**dp[i-1]**)就没有持有**或者**之前(**dp[i-1]**)已经sell抛售了
    + **dp[i][k][1]**：第i天本人至今最多交易了k次，并且手上**持有**股票
      + 之前(**dp[i-1]**)已经持有，并选择继续持有**或者**之前(**dp[i-1]**)就没有持有，今天选择buy持有
+ **转移方程**
  + **dp[i][k][0] = Max(dp[i-1][k][1][0],dp[i-1][k][1] + prices[i])**
  + **dp[i][k][1] = Max(dp[i-1][k][1][1],dp[i-1][k-1][0] - prices[i])**
+ **边界条件**
  + dp[-1][k][0] = 0
    + 第一天是i = 0，而i = -1 时意即还未开始，利润为0
    + 同理：dp[-n][k][0] = 0
  + dp[-1][k][1] = -Infinity
    + 还没开始时是不可能持有或买入股票的，用负无穷代表买入的是非正常不可能的负数的的股票
    + 同理：dp[-n][k][0] = -Infinity
  + dp[i][0][0] = 0
    + 交易次数最少为1才有可能产生利润，为0时，意味还没开始交易，利润为0
  + dp[i][0][1] = -Infinity
    + 还未交易时，是不可能持有股票的，用负无穷表示这种不可能
+ **结果**
  + **dp[n-1][k][0]**
    + 最后一个手上没有股票的利润要比手上还有没被抛售的股票的利润更大
+ **dp代码框架**
    + 设三维数组 dp[n][k+1][2]，n，k+1，2均为当前维度数组元素个数
    + i为天数，m为最大交易次数，0或1为交易状态；且0 <= i < n ，1 <= m <= k
    ```javascript
    var maxProfit = function(k, prices) {
        // 交易天数
        let n = prices.length;
        // 最大交易次数
        //-----如果当题k不影响状态转移方程，此处去掉
        let maxTime = k;
        if(n == 0){
            return 0;
        }
        // 初始化三维数组
        //-----如果当题k不影响状态转移方程，此处初始化去掉
        let dp = Array.from(new Array(n),() => new Array(maxTime+1));
        for(let i = 0;i < n;i++){
            for(let r = 0;r <= maxTime;r++){
                dp[i][r] = new Array(2);
            }
        }
        //-----如果当题k不影响状态转移方程，则只需二维数组
        // let dp = Array.from(new Array(n),() => new Array(2));

        // 遍历递推
        for(let i = 0;i < n;i++){
            //-----如果当题k不影响状态转移方程，内循环去掉
            for(let k = maxTime;k >= 1;k--){
                if(i == ...){
                    // 边界条件处理
                    // 
                    continue;
                }
                // 递推公式
                dp[i][k][0] = Max(前一天交易次数k：买，卖，不买也不卖);
                dp[i][k][1] = Max(前一天交易次数k-1：买，卖，不买也不卖);
            }
        }
        // 返回结果
        return dp[n-1][maxTime][0];
        //-----如果当题k不影响状态转移方程
        // return dp[n-1][0];
    };
    ```
#### 卍解
+ 1、***121***
  + **状态转移方程**
    + ***k = 1***
    ```javascript
    dp[i][1][0] = Max(dp[i-1][1][0],dp[i-1][1][1] + prices[i])
    dp[i][1][1] = Max(dp[i-1][1][1],dp[i-1][1-1][0] - prices[i])
                = Max(dp[i-1][1][1],0-prices[i])
                = Max(dp[i-1][1][1],-prices[i])
    ```
    + 正如表格分析可知，k都是1，对整个三维状态转移方程毫无影响，可以化掉
    ```javascript
    dp[i][0] = Max(dp[i-1][0],dp[i-1][1] + prices[i])
    dp[i][1] = Max(dp[i-1][1],-prices[i])
    ``` 
  + **解法一**
    ```javascript
    /**
     * @param {number[]} prices
     * @return {number}
     */
    var maxProfit = function(prices) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let dp = Array.from(new Array(n),() => new Array(2));
        for(let i = 0;i < n;i++){
            if(i == 0){
                dp[i][0] = 0;
                dp[i][1] = -prices[i];
                continue;
            }
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]+prices[i]);
            dp[i][1] = Math.max(-prices[i],dp[i-1][1]);
        }
        return dp[n-1][0];
    };
    ```
  + **解法二 - 降维**
    + 新的状态只和相邻的一个状态有关，所以只需要两个相邻状态的变量进行递归即可
    + 正如[70. 爬楼梯-解法一](https://leetcode-cn.com/problems/climbing-stairs/solution/70-pa-lou-ti-by-alexer-660/)问题一样
    ```javascript
    /**
     * @param {number[]} prices
     * @return {number}
     */
    var maxProfit = function(prices) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        var dp_i_0 = 0;
        var dp_i_1 = -Infinity;
        for(let i = 0;i < n;i++){
            dp_i_0 = Math.max(dp_i_0,dp_i_1 + prices[i]);
            dp_i_1 = Math.max(-prices[i],dp_i_1);
        }
        return dp_i_0;
    };
    ```
___
+ 2、***122***
  + **状态转移方程**
    + ***k = +infinity***
    ```javascript
    dp[i][k][0] = Max(dp[i-1][k][0],dp[i-1][k][1] + prices[i])
    dp[i][k][1] = Max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i])
                = Max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i])
                = Max(dp[i-1][k][1],dp[i-1][k][0] - prices[i])
    ```
    + 正如表格分析可知，k为正无穷时，k和k-1都可以到达最大利润，对整个三维状态转移方程无影响，可以化掉
    ```javascript
    dp[i][0] = Max(dp[i-1][0],dp[i-1][1] + prices[i])
    dp[i][1] = Max(dp[i-1][1],dp[i-1][0] - prices[i])
    ```
  + **解法一** 
    ```javascript
    /**
    * @param {number[]} prices
    * @return {number}
    */
    var maxProfit = function(prices) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let dp = Array.from(new Array(n),() => new Array(2));
        for(let i = 0;i < n;i++){
            if(i == 0){
                dp[0][0] = 0;
                dp[0][1] = -prices[0];
                continue;
            }
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]+prices[i]);
            dp[i][1] = Math.max(dp[i-1][0]-prices[i],dp[i-1][1]);
        }
        return dp[n-1][0];
    };
    ``` 
  + **解法二 - 降维**
    ```javascript
    /**
    * @param {number[]} prices
    * @return {number}
    */
    var maxProfit = function(prices) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let dp_i_0 = 0;
        let dp_i_1 = -Infinity;
        for(let i = 0;i < n;i++){
            var tmp = dp_i_0; 
            dp_i_0 = Math.max(dp_i_0,dp_i_1+prices[i]);
            dp_i_1 = Math.max(tmp-prices[i],dp_i_1);
        }
        return dp_i_0;
    }; 
___

+ 3、***123***
  + **状态转移方程**
    + ***k = 2***
    ```javascript
    dp[i][k][0] = Max(dp[i-1][k][0],dp[i-1][k][1] + prices[i])
    dp[i][k][1] = Max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i])
    ```
    + 正如表格分析可知，k为2，对整个三维状态转移方程有影响，不能化掉k和k-1
  + **解法一** 
    ```javascript
    /**
    * @param {number[]} prices
    * @return {number}
    */
    var maxProfit = function(prices) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let maxTime = 2;
        let dp = Array.from(new Array(n),() => new Array(maxTime+1));
        for(let i = 0;i < n;i++){
            for(let r = 0;r <= maxTime;r++){
                dp[i][r] = new Array(2).fill(0);
            }
        }
        for(let i = 0;i < n;i++){
            for(let k = maxTime;k >= 1;k--){
                if(i == 0){
                    dp[i][k][0] = 0;
                    dp[i][k][1] = -prices[i];
                    continue;
                }
                dp[i][k][0] = Math.max(dp[i-1][k][0],dp[i-1][k][1] + prices[i]);
                dp[i][k][1] = Math.max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i]);
            }
        }
        return dp[n-1][maxTime][0];
    };
    ``` 
  + **解法二 - 降维**
    + k = 2
      + 只有两种情况，直接列举出所有的4种状态即可
    ```javascript
    /**
    * @param {number[]} prices
    * @return {number}
    */
    var maxProfit = function(prices) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let dp_i_1_0 = 0;
        let dp_i_1_1 = -prices[0];
        let dp_i_2_0 = 0;
        let dp_i_2_1 = -prices[0];
        for(let i = 0;i < n;i++){
            dp_i_1_0 = Math.max(dp_i_1_0,dp_i_1_1 + prices[i]);
            dp_i_1_1 = Math.max(dp_i_1_1,0 - prices[i]);
            dp_i_2_0 = Math.max(dp_i_2_0,dp_i_2_1+prices[i]);
            dp_i_2_1 = Math.max(dp_i_2_1,dp_i_1_0-prices[i]);
        }
        return dp_i_2_0;
    };
    ``` 
___
+ 4、***309***
  + **状态转移方程**
    + ***k = +infinity***
    + i：每次交易后要隔一天才能继续交易，即冷冻期
      + 即第i天选择1即buy时，
        + 第i-2天才是上一步交易的开始状态，第i-1天不能交易保持原来持有状态
    ```javascript
    dp[i][k][0] = Max(dp[i-1][k][0],dp[i-1][k][1] + prices[i])
    dp[i][k][1] = Max(dp[i-1][k][1],dp[i-2][k-1][0] - prices[i])
                = Max(dp[i-1][k][1],dp[i-2][k-1][0] - prices[i])
                = Max(dp[i-1][k][1],dp[i-2][k][0] - prices[i])
    ```
    + 和第2、题一样，k 对整个三维状态转移方程毫无影响，可以化掉
    ```javascript
    dp[i][0] = Max(dp[i-1][0],dp[i-1][1] + prices[i])
    dp[i][1] = Max(dp[i-1][1],dp[i-2][0] - prices[i])
    ``` 
  + **解法一** 
    ```javascript
    /**
    * @param {number[]} prices
    * @return {number}
    */
    var maxProfit = function(prices) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let dp = Array.from(new Array(n),() => new Array(2));
        for(var i = 0;i < n;i++){
            if(i == 0){
                dp[0][0] = 0;
                dp[0][1] = -prices[i];
                continue;
            }else if(i == 1){
                dp[1][0] = Math.max(dp[0][0],dp[0][1]+prices[i]);
                dp[1][1] = Math.max(dp[0][1], - prices[i]);
                continue;
            }
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i-1][1],dp[i-2][0] - prices[i]);
        }
        return dp[n-1][0];
    };
    ```
  + **解法二 - 降维**   
    ```javascript
    /**
    * @param {number[]} prices
    * @return {number}
    */
    var maxProfit = function(prices) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let dp_i_0 = 0;
        let dp_i_1 = -Infinity;
        let dp_pre = 0;
        for(var i = 0;i < n;i++){
            let tmp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0,dp_i_1 + prices[i]);
            dp_i_1 = Math.max(dp_i_1,dp_pre - prices[i]);
            dp_pre = tmp;
        }
        return dp_i_0;
    };
    ```
+ 5、***188***
  + **状态转移方程**
    + ***k = any***
      + **k > n/2  时，<=> k = +infinity**
        ```javascript
        dp[i][k][0] = Max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
        dp[i][k][1] = Max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
                    = Max(dp[i-1][k][1], dp[i-1][k][0] - prices[i])
        ```
        + 和第2、题一样，k 对整个三维状态转移方程毫无影响，可以化掉
        ```javascript
        dp[i][0] = Max(dp[i-1][0],dp[i-1][1] + prices[i])
        dp[i][1] = Max(dp[i-1][1],dp[i-1][0] - prices[i])
        ``` 
      + **k < n/2  时，<=> k = 2**
        ```javascript
        dp[i][k][0] = Max(dp[i-1][k][0],dp[i-1][k][1] + prices[i])
        dp[i][k][1] = Max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i])
        ```
        + 正如表格分析可知，k为2，对整个三维状态转移方程有影响，不能化掉k和k-1
  + **解法一** 
    + 超时
      + k输入过大，导致dp数组申请内存超出限制
      + 所以要分情况
     ```javascript
    /**
    * @param {number} k
    * @param {number[]} prices
    * @return {number}
    */
    var maxProfit = function(k, prices) {
        let n = prices.length;
        let maxTime = k;
        if(n == 0){
            return 0;
        }
        let dp = Array.from(new Array(n),() => new Array(maxTime+1));
        for(let i = 0;i < n;i++){
            for(let r = 0;r <= maxTime;r++){
                dp[i][r] = new Array(2);
            }
        }
        for(let i = 0;i < n;i++){
            for(let k = maxTime;k >= 1;k--){
                if(i == 0){
                    dp[i][k][0] = 0;
                    dp[i][k][1] = - prices[i];
                    continue;
                }
                dp[i][k][0] = Math.max(dp[i-1][k][0],dp[i-1][k][1] + prices[i]);
                dp[i][k][1] = Math.max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i]);
            }
        }
        return dp[n-1][maxTime][0];
    };
    ```
    + 分情况优化版
      + k > n/2 <=> k = +infinity
      + k < n/2 <=> k = 2
      + 合并两种情况解法即为此题解法
      ```javascript
        /**
        * @param {number} k
        * @param {number[]} prices
        * @return {number}
        */
        var maxProfit = function(k, prices) {
            let n = prices.length;
            let maxTime = k;
            if(n == 0){
                return 0;
            }
            let maxProfitInfinity = (prices) => {
                let n = prices.length;
                if(n == 0){
                    return 0;
                }
                let dp = Array.from(new Array(n),() => new Array(2));
                for(let i = 0;i < n;i++){
                    if(i == 0){
                        dp[0][0] = 0;
                        dp[0][1] = -prices[0];
                        continue;
                    }
                    dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1]+prices[i]);
                    dp[i][1] = Math.max(dp[i-1][0]-prices[i],dp[i-1][1]);
                }
                return dp[n-1][0];
            };
            if(maxTime > n/2){
                return maxProfitInfinity(prices);
            }
            let dp = Array.from(new Array(n),() => new Array(maxTime+1));
            for(let i = 0;i < n;i++){
                for(let r = 0;r <= maxTime;r++){
                    dp[i][r] = new Array(2).fill(0);
                }
            }
            for(let i = 0;i < n;i++){
                for(let k = maxTime;k >= 1;k--){
                    if(i == 0){
                        dp[i][k][0] = 0;
                        dp[i][k][1] = - prices[i];
                        continue;
                    }
                    dp[i][k][0] = Math.max(dp[i-1][k][0],dp[i-1][k][1] + prices[i]);
                    dp[i][k][1] = Math.max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i]);
                }
            }
            return dp[n-1][maxTime][0];
        };
        ``` 
  + **解法二 - 降维**  
    ```javascript
    /**
    * @param {number} k
    * @param {number[]} prices
    * @return {number}
    */
    var maxProfit = function(k, prices) {
        let n = prices.length;
        let maxTime = k;
        if(n == 0){
            return 0;
        }
        // 此处降维-复用第2题k=infinity的方法
        // 但不能复用第4题的，因为第4天仍有交易天数的限制
        let maxProfitInfinity = (prices) => {
            let n = prices.length;
            if(n == 0){
                return 0;
            }
            let dp_i_0 = 0;
            let dp_i_1 = -Infinity;
            for(let i = 0;i < n;i++){
                var tmp = dp_i_0; 
                dp_i_0 = Math.max(dp_i_0,dp_i_1+prices[i]);
                dp_i_1 = Math.max(tmp-prices[i],dp_i_1);
            }
            return dp_i_0;
        };
        if(maxTime > n/2){
            return maxProfitInfinity(prices);
        }
        // 此处不能能像第3题k=2那样降维
        // 因为：第三题k值较小，它第解法是直接列举处所有可能的情况出来
        // 但是：此题当k<n/2的情况下，k依然可能很大，如果k=10，11，12
        // 那这样就不像爬楼梯和斐波那契数列那样只有两种情况了可以直接列举降维了
        // 因此只能用for循环枚举所有k可能的状态，三维无可避免。
        let dp = Array.from(new Array(n),() => new Array(maxTime+1));
        for(let i = 0;i < n;i++){
            for(let r = 0;r <= maxTime;r++){
                dp[i][r] = new Array(2).fill(0);
            }
        }
        for(let i = 0;i < n;i++){
            for(let k = maxTime;k >= 1;k--){
                if(i == 0){
                    dp[i][k][0] = 0;
                    dp[i][k][1] = - prices[i];
                    continue;
                }
                dp[i][k][0] = Math.max(dp[i-1][k][0],dp[i-1][k][1] + prices[i]);
                dp[i][k][1] = Math.max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i]);
            }
        }
        return dp[n-1][maxTime][0];
    };
    ``` 
___
+ 6、***714***
  + **状态转移方程**
    + ***k = +infinity***
    ```javascript
    dp[i][k][0] = Max(dp[i-1][k][0],dp[i-1][k][1] + prices[i])
    dp[i][k][1] = Max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i] - fee)
                = Max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i] - fee)
                = Max(dp[i-1][k][1],dp[i-1][k][0] - prices[i] - fee)
    ```
    + 和第2、题一样，k 对整个三维状态转移方程毫无影响，可以化掉
    ```javascript
    dp[i][0] = Max(dp[i-1][0],dp[i-1][1] + prices[i])
    dp[i][1] = Max(dp[i-1][1],dp[i-1][0] - prices[i] - fee)
    ``` 
  + **解法一** 
    ```javascript
    /**
     * @param {number[]} prices
     * @param {number} fee
     * @return {number}
     */
    var maxProfit = function(prices, fee) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let dp = Array.from(new Array(n),() => new Array(2));
        for(let i = 0;i < n;i++){
            if(i == 0){
                dp[0][0] = Math.max(0,-Infinity+prices[0]);
                dp[0][1] = Math.max(-Infinity,0 - prices[0] - fee);
                continue;
            }
            dp[i][0] = Math.max(dp[i-1][0],dp[i-1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i-1][1],dp[i-1][0] - prices[i] - fee);
        }
        return dp[n-1][0];
    };
    ``` 
  + **解法二 - 降维**
    ```javascript
    /**
    * @param {number[]} prices
    * @param {number} fee
    * @return {number}
    */
    var maxProfit = function(prices, fee) {
        let n = prices.length;
        if(n == 0){
            return 0;
        }
        let dp_i_0 = 0;
        let dp_i_1 = -Infinity;
        for(let i = 0;i < n;i++){
            let tmp = dp_i_0;
            dp_i_0 = Math.max(dp_i_0,dp_i_1 + prices[i]);
            dp_i_1 = Math.max(dp_i_1,tmp - prices[i] - fee);
        }
        return dp_i_0;
    };
    ``` 
#### [更多leetCode题解敬请戳看👇](https://github.com/Alex660/leetcode)