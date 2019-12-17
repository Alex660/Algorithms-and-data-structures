## 回溯算法--求解八皇后问题
```javascript
// 8X8的棋盘上要求放满符合这样条件的棋子：
// 1、每个棋子的横行、竖列和左右对角线上不能有其它任何一颗棋子【皇后】
// 2、求这样的棋子有多少个

// 设结果数组：result 索引代表行，值代代表列 值可以存在时即为所求
// 初始化8个元素的数组
var result = new Array(8);

// 主函数
function cal8queens(row) {
    // 8 行都放满了 退出
    if(row == 8){
        printQueens(result);
        return;
    }
    for(var column = 0;column<8;column++){
        // 符合要求的就落子
        if(isOk(row,column)){
            result[row] = column;
            cal8queens(row+1);
        }
    }
}

// 判断 row 行 column 列放置是否合适
function isOk(row,column) {
    //  当前列的左右两列
    var leftColumn = column -1;
    var rightColumn = column +1;

    // 逐上判断每一行是否能放棋子
    for(var i = row - 1;i>=0;i--){
        // 第i行第column列是否有棋子【上一行同一列不能有棋子】
        if(result[i] == column){
            return false;
        }
        // 当左列存在 && 考虑左上对角线【递减斜向上】 第i行第leftColumn列是否有棋子
        if(leftColumn>=0){
            if(result[i] == leftColumn){
                return false;
            }
        }
        // 当右列存在 && 考虑右上对角线【递减斜向上】 第i行第rightColumn列是否有棋子
        if(rightColumn<8){
            if(result[i] == rightColumn){
                return false;
            }
        }
        leftColumn--;
        rightColumn++;
    }
    return true;
}

// 打印结果
function printQueens(result) {
    console.log(result);
    var str = '';
    for(var i=0;i<8;i++){
        for(var r=0;r<8;r++){
            if(result[i] == r){
                str+='Q ';
            }else{
                str+='* ';
            }
        }
        str+='\n'
    }
    console.log(str)
}
```