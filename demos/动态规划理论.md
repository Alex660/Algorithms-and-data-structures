## 动态规划理论
```javascript
// “一模三特”
// 一、模型【多阶段决策最优解模型】
// ----解决问题过程经历n个决策阶段 && 分别对应一组状态
// 二、特征【最优子结构、无后效性、重复子问题】
// ----<1>、最优子结构
//    问题的最优解包含子问题的最优解 => 子问题的最优解推导出问题的最优解
//    后面阶段的状态可以通过前面阶段的状态推导出来
// ----<2>、无后效性
//    1⃣️：不关心当前状态的推导，只关心前面阶段的状态值
//    2⃣️：前一阶段的状态一旦确定就不受之后阶段的决策影响
// ----<3>、重复子问题
//    不同的决策序列，到达某个相同的阶段时，可能会产生重复的状态

// 实例
// 一个 n*n 的矩阵 w[n][n]
// 求从左上角出发到右下角最短距离【路经的数字和为路径长度】
// 每次只能向右或向下移动一位

//  [i,j] 0 1 2 3
//    0   1 3 5 9
//    1   2 1 3 4
//    2   5 2 6 7
//    3   6 8 4 3

// (0,0)->(n-1,n-1) 共 2*(n-1)步 => 2*(n-1)个阶段状态集合 <= 向右走/向下走
// min_dist(i,j) <= (0,0)->(i,j)的最短路径长度
// (0,0)->(i-1,j)/(i,j-1)->(i,j)
//  ==>
// min_dist(i,j) == Math.min(min_dist(i-1,j),min_dist(i,j-1)) + w[i][j]


// 动态规划题目解题思路
// 一、状态转移表法
// 二、状态转移方程法

// 一：
    // 回溯算法->回溯加“备忘录”
        // 代码实现
        var minDist = 0;//最短路径
        // 当前状态最短路径
        var dist = 0;
        // 定义n*n 
        var n = 4;
        // 初始化随机二维整数【1～10】数组
        var w = [];
        while (w.length<n) {
            // 1~10随机数
            w.push(Math.floor(Math.random(0,1)*10)+1);
        }
        function minDistBacktracing(i,j,dist) {
            // 路过所有节点/到达了n-1,n-1这个位置了【有多个集合】
            if(i == n && j == n){
                if(dist<minDist){
                    minDist = dist;
                }
            }
            // 往下走
            if(i<n){//边界
                minDistBacktracing(i+1,j,dist)
            }
            // 往右走
            if(j<n){//边界
                minDistBacktracing(i,j+1,dist)
            }
        }

        //  [i,j] 0  1  2  3
        //    0   1  3  5  9
        //    1   2  1  3  4          
        //    2   5  2  6  7
        //    3   6  8  4  3

//                   ||
//                   ||
//                   \/

        //  [i,j] 0  1  2  3
        //    0   1  4  9  18
        //    1   3  4  7  11
        //    2   8  6  12 18 
        //    3   14 14 16 19
    // 状态规划->   
        // 状态转移表
            // 一般情况下二维【数组】 (高维时就不适合这种方法了太复杂)
            // [i,j] = value 行，列，数组值
            // 代码实现
            // n*n
            var n = 4;
            // 初始化二维数组
            var result = Array(n);
            result[0] = [1,3,5,9];
            result[1] = [2,1,3,4];
            result[2] = [5,2,6,7];
            result[3] = [6,8,4,3];
            function getMinDist(result) {
                // 临时最短路径
                var minDist = 0;
                // 初始化第一行n列数据
                for(var i = 0;i<n;i++){
                    minDist += result[0][i];
                    result[0][i] = minDist;
                    // 此处也可以再定义一个数组，利于分析 下面第状态转移表亦是如此
                    // example[0][i] = minDist;
                    // 但递归时用备忘录的话在原数组上修改就不好判断了就需要另外创建一个数组
                }
                // 初始化第一列n行数据 
                minDist = 0;
                for(var i = 0;i<n;i++){
                    minDist += result[i][0];
                    result[i][0] = minDist;
                }
                // 状态转移表推进
                for(var i = 1;i<n;i++){
                    for(j = 1;j<n;j++){
                        result[i][j] = result[i][j] + Math.min(result[i-1][j],result[i][j-1]);
                    }
                }
                // 返回最后一个也就是结果
                return result[n-1][n-1];
            }

        // 状态转移方程
            // 由一系列的分析可知状态转移方程：
            // min_dist(i,j) = w[i][j] + min(min_dist(i,j-1),min_dist(i-1,j))
                //递归加“备忘录”
                // 代码实现
                    // 路线数组
                    var matrix = [
                        [1,3,5,9],
                        [2,1,3,4],
                        [5,2,6,7],
                        [6,8,4,3]
                    ];
                    // 初始化最短路径数组
                    var result = [
                        [-1,-1,-1,-1],
                        [-1,-1,-1,-1],
                        [-1,-1,-1,-1],
                        [-1,-1,-1,-1],
                    ]
                    var n = 4;
                    function minDist(i,j) {
                        // 递推到第一个状态停止递推 
                        if(i == 0 && j == 0){
                            return matrix[0][0]
                        }
                        var leftUp = 0;
                        var topUp = 0;
                        // 求向右走后的上一步最短路径 && 考虑边界情况
                        j-1>=0 && (leftUp = minDist[i][j-1])
                        // 求向下走后的上一步最短路径 && 考虑边界情况
                        i-1>=0 && (topUp = minDist[i-1][j])
                        // 求上两种情况的最新上一步路径+当前路径 = 当前结点最小路径
                        result[i][j] = matrix[i][j] + Math.min(leftUp,topUp);
                        return result[i][j];
                    }
                //迭代递推
                    // ......
```