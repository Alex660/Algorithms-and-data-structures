## 实现一个 instanceof
#### code
```javascript
function myinstanceof(a,b) {
    while (a) {
        if( a.__proto__ === b.prototype) return true;
        a = a.__proto__;
    }
    return false;
}
```