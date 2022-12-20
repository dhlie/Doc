##### Promise

    let promise = new Promise(function(resolve, reject) {
        setTimeout(function() {
            if (true) {
                resolve('done')
            } else {
                reject('error')
            }
        }, 500)
    })//promise 已经开始执行

    promise.then(
        //onResolve
        () => {

        }, 
        //onReject
        () => {

        }
    )

    构造完立即执行,无法取消
    Promise对象的状态改变，只有两种可能：从Pending变为Resolved和从Pending变为Rejected
    就算改变已经发生，你再对Promise对象添加回调函数也会立即得到这个结果

    Promise.resolve(value) 返回 状态为 fulfilled 的 Promise 对象
    Promise.reject(reason) 返回 状态为 rejected 的 Promise 对象

    方法签名
    then(onFulfill, onReject): Promise;
    /**
    * A sugar method, equivalent to promise.then(undefined, onRejected).
    * 语法糖, 相当于 promise.then(undefined, onRejected), 还有另一个作用: 可以捕获 resolve 抛出的异常
    */
    catch(onRejected: (reason: any) => IWhenable<U>): Promise;

    //最后一个 promise 执行完才会回调, 可以并行执行多个异步任务, 结果是所有promise结果组成的数组
    all<A, B>(promises: IWhenable<[A, B]>): Promise<[A, B]>

    //只回调最快完成的结果, 其它结果丢弃
    race<T>(promises: Array<IWhenable<T>>): Promise<T>