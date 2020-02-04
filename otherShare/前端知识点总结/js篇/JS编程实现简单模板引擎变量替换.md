## JS编程实现简单模板引擎变量替换
### 解题关键
+ replace/正则
### 参考文献
+ [js简单前端模板引擎实现](https://segmentfault.com/a/1190000010366490)
+ [JavaScript模板引擎实现原理实例详解](https://www.jb51.net/article/152760.htm)
#### code
```javascript
let render = (str) => {
    return (obj) => {
        str = str.replace('YY',obj.YY);
        str = str.replace('MM',obj.MM);
        str = str.replace('DD',obj.DD);
        return str;
    }
}
// 栗子
const YY = '2020';
const MM = '02';
const DD = '04';
const str = render('YY-MM-DD')({YY,MM,DD});
console.log(str);// 2020-02-04
```