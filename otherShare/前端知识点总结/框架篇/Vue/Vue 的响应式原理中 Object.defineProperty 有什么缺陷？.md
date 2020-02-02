## Vue 的响应式原理中 Object.defineProperty 有什么缺陷？
### 为什么在 Vue3.0 采用了 Proxy，抛弃了 Object.defineProperty？
#### 参考文献
+ [Vue为什么不能检测数组变动](https://segmentfault.com/a/1190000015783546#comment-area)
+ [抱歉，学会 Proxy 真的可以为所欲为](https://zhuanlan.zhihu.com/p/35080324)
+ [阮一峰 - Proxy详解](http://es6.ruanyifeng.com/#docs/proxy)
+ [面试官: 实现双向绑定Proxy比defineproperty优劣如何?](https://juejin.im/post/5acd0c8a6fb9a028da7cdfaf)
#### 参考答案
+ Object.defineProperty无法监控到数组下标的变化，导致通过数组下标添加元素，不能实时响应；
  + Vue通过修改数组的八种方法进行了hack处理
    + push()
    + pop()
    + shift()
    + unshift()
    + splice()
    + sort()
    + reverse()
+ Object.defineProperty只能劫持对象的属性，从而需要对每个对象，每个属性进行遍历，如果，属性值是对象，还需要深度遍历。
  + Proxy可以劫持整个对象，并返回一个新的对象。
  + Proxy有13种劫持操作
+ Proxy不仅可以代理对象，还可以代理数组。还可以代理动态增加的属性。