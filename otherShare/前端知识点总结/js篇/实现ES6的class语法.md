#### 实现ES6的class语法
```javascript
function inherit(subType,superType) {
    subType.prototype = Object.create(superType.prototype,{
        constructor: {
            enumerable:false,
            configurable:true,
            writable:true,
            value:subType
        }
    })
    Object.setPrototypeOf(subType,superType)
}
``` 