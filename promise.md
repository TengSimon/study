# 1.Promise的含义
Promise是解决异步操作的一种方法，相较于传统的回调方式。可以解决回调地狱的问题。代码更好理解。功能也更加强大。最早由社区提出。ES6进行了标准化，并提供`Promise`对象。
Promise有三种状态。`pendding`（进行中），`fulfilled`（已成功），`rejected`（已失败）。
异步操作的结果决定三种状态，Promise的状态更改后就不能改变。

跟据上面説的我们简单实现一下promise对象。
```        
        const pedding = 'PEDDING'
        const fulfilled = 'FULFILLED'
        const rejected = 'REJECTED'

        class MyPromise {
            constructor(fun) {
                fun(this.resolve, this.rejected)
            }
            state = null
            value = null
            reson = null
            resolveCBList = []
            rejectCBList = []
            resolve = (value)=> {
                this.state = fulfilled
                this.value = value
                while (this.resolveCBList.length) {
                    this.resolveCBList.shift()(value)
                }
            }
            rejected = (reson)=> {
                this.state = rejected
                this.reson = reson
                while (this.rejectCBList.length) {
                    this.rejectCBList.shift()(value)
                }
            }
            then = (resolveCB, rejectedCB)=>{
                if (this.state === fulfilled) {
                    resolveCB(this.value)
                } else if (this.state === rejected) {
                    rejectedCB(this.reson)
                } else if (this.state = pedding) {
                    this.resolveCBList.push(resolveCB)
                    this.rejectCBList.push(rejectedCB)
                }
            }

        }

        const promise = new MyPromise((resolve, reject)=>{
            setTimeout(() => {
                resolve("data")
            }, 1000);
        })

        promise.then((value)=>{
            console.log(value)
        })
```

可以看到，我们定义了三种状态。然后resolve和rejected负责修改状态。resolveCBList和rejectCBList主要为了处理异步的结果。注意的是这段代码实现的promise还不支持链式调用。

# 2.Promise的方法

## 1.Promise.prototype.then()
Promise的实列具有then方法。也就是`then()`方法是定义在Promise的原型对象。`Promise.prototype`上。

## 2.Promise.prototype.catch()
`Promise.prototype.catch()`是`.then(null, rejection)或者.then(undefined, rejection)`的别名。推荐Promise后面跟catch而不是then传入第二个参数。catch可以捕获异常。但是以改变resolve状态无效。Promise中错误具有冒泡性质会一直传递知道catch捕获。如果不写catch。Promise的异常不会抛到外层。会打印错误。但不会中止脚本。

## 3.Promise.prototype.finally()
`finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作

## 4.Promise.all()
将多个promise合并成一个。
```
    const p = Promise.all([p1, p2, p3]);

```
参数可以不是数组，但是必须有Iterator接口，并且返回promise实列。

参数里所有promise都fulfilled，合并的这个才会变为fulfilled
一个rejected都，就变rejected。
单独promise不写catch，会用集合的这个catch。
## 4.Promise.race()
基本同all()。不同的是。一个改变就都改变。

## 5.Promise.any()

基本同race()。不同的是。不会因为一个rejected而都rejected。而是等所有都rejected才会rejected。






















