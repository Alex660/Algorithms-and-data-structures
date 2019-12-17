## 薅羊毛
```javascript
// 满200减50元 假设购物车有n个商品
// 在凑够满减条件的前提下 让选出来的商品价格总和接近 满减值 是为 薅羊毛

// 类似01背包问题动态规划求解方法
// n个商品每个商品买或不买，决策之后对应不同的状态集合；用二维数组 states[n][x]记录决策后的状态
// 0-1背包问题中，找的是 <=w 的最大值 x = 背包的最大承载重量 = w+1
// 对于这个问题 x 为大于等于 200【满减条件】值中最小的
// 求X后 倒推出被选择的商品序列

// items 商品价格
// n     商品个数
// w     满减条件

var items = [100,50,60,50,110,106];
var n = items.length;
var w = 200;
var needBuy = [];
function getNeedShop() {
    // 设置满减值的3倍为薅羊毛的最大利益化 && 初始化二维数组
    var result = Array(n);
    for(var i = 0;i<n;i++){
        result[i] = Array(3*w+1);
    }
    // 第一行数据特殊处理 作为哨兵优化
    result[0][0] = true;
    if(items[0] <= 3*w){
        result[0][items[0]] = true;
    }
    // 动态规划状态转移
    for(var i = 1;i<n;i++){
        // 不购买第 i 个商品
        for(var j = 0;j<= 3*w;j++){
            if(result[i-1][j] == true){
                result[i][j] = result[i-1][j];
            }
        }
        // 购买第 i 个商品
        for(var j = 0;j<=3*w - items[i];j++){
            if(result[i-1][j] == true){
                result[i][j+items[i]] = true;
            }
        }
    }
    // 确认结果大于等于w的最小值
    var minMaxJ;
    for(minMaxJ = w;j<3*w+1;j++){
        if(result[n-1][minMaxJ] == true){
            break;
        }
    }
    // 不存在利益最大化的解
    if(minMaxJ == 3*w+1){
        return;
    }
    // 有解 倒推出选择的商品
    // 分析
    // （i,j） <=  (i-1,j) 或 (i-1,j-items[i])
    // =>  检查result[i-1][j] 或者 result[i-1][j-itmes[i]]是否为true
    // => result[i-1][j]可达 => 没有选择购买第 i 个 商品
    // => result[i-1][j-items[i]] 可达 => 购买了第 i 个 商品
    // 继续迭代其它商品是否选择购买
    for(var i = n-1;i>=1;i--){
        if(j-items[i] >= 0 && result[i-1][j-items[i]] == true){
            needBuy.push(itmes[i]);
            // 更新列
            j = j -itmes[i];
        }//else 没有购买这个商品，j不变
    }
    // 动态规划初始值由此推动而来所以要加上
    if(j!=0){
        needBuy.push(itmes[0])
    }
    return needBuy;
}
```