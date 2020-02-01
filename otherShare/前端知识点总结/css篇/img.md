# div里包img图片下方有空白的问题
```javascript
<div class="banner">
    <img src="https://profile.csdnimg.cn/7/0/4/3_qq_34400736"/>
</div>
```
### 原理
+ 图片的display属性默认是inline，
+ 而这个属性的vertical-align的默认值是baseline。
+ 所以就会出现上图的情况（图片底部出现一个小留白区域）。
#### 解法
+ 把img变成块状元素，所以是否需要留白，可以用过padding来设置。
```css
img {
    display: block;
}
```
+ 直接修改vertical-align属性值
```css
img {
    vertical-align: middle/bottom;
}
```
+ 把img元素的底部外边距改成负值
```css
img {
    margin-bottom: -4px;
}
```
+ 出现留白的原因是因为垂直对齐的方式所导致的，所以可以修改父元素的font-size，把父元素的字体大小改为0，所以就没什么垂直对齐所言
```css
.banner {
    font-size: 0;
}
```
+ 把img的父元素行高设成0
```css
.banner {
    line-height: 0;
}
```