## promise的简单实现
+ 图解
  + ![](https://segmentfault.com/img/bVbmYtD?w=716&h=736)
+ code
+ 写法一：
    ```javascript
    let i = 1;
    var Promise = (fn) => {
        this.id = i++;
        this.status = 'PENDING';
        this.value = null;
        this.deffered = [];
        fn.call(this,this.resolve.bind(this),this.reject.bind(this));
    }
    Promise.prototype = {
        constructor:Promise,
        then(onfulfilled,onrejected){
            let obj = {
                onfulfilled:onfulfilled,
                onrejected:onrejected
            }
            obj.promise = new this.constructor(() => {});
            console.log(this.status);
            if(this.status === 'PENDING'){
                this.deffered.push(obj);
            }
            console.log(this.deffered);
            return obj.promise;
        },
        resolve(data){
            this.status = 'FULFILLED';
            this.value = data;
            this.deffered.forEach(item => {
                let p = item.onfulfilled(this.value);
                if(p && p.constructor === Promise){
                    p.deffered = item.promise.deffered;
                }
            });
        },
        reject(err){
            this.status = 'REJECTED';
            this.value = err;
            this.deffered.forEach(item => {
                let p = item.onrejected(this.value);
                if(p && p.constructor === Promise){
                    p.deffered = item.promise.deffered;
                }
            })
        }
    }
    ```
+ 写法二：
    ```javascript
    ```