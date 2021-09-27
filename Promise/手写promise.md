### 手写Promise

~~~javascript
/**
 * 自定义Promise函数模块：IIFE
 */
(function (window) {

    const PENDING = 'pending';
    const FULFILLED = 'fulfilled';
    const REJECTED = 'rejected';

    /**
     * Promise构造函数
     * @param executor 执行器函数（同步执行） (resolve,reject)=>{}
     * @constructor
     */
    function Promise(executor) {

        const _this = this; //保存当前实例的this
        this.promiseStatus = PENDING;//每个promise对象的状态，初始值为pending
        this.promiseResult = undefined;//每个promise对象用于保存结果数据的属性
        this.callbacks = [];//数组中存放的是对象 每个元素的结构:{onResolved:onResolved(){},onRejected:onRejected(){}}

        /**
         * executor 内部定义成功时调用的函数
         * @param value
         */
        function resolve(value) {

            if (_this.promiseStatus !== PENDING) return;//如果当前状态不是pending 立即结束 promise只能修改一次状态
            _this.promiseStatus = FULFILLED;//修改状态为成功
            _this.promiseResult = value;//结果为value
            //放入队列中执行成功的回调 promise回调为微任务 不是同步执行
            setTimeout(function () {
                _this.callbacks.forEach(callback => {
                    callback.onResolved(_this.promiseResult);
                })
                clearTimeout(this);
            })

        }

        /**
         * executor 内部定义失败时调用的函数
         * @param reason
         */
        function reject(reason) {

            if (_this.promiseStatus !== PENDING) return;//如果当前状态不是pending 立即结束 promise只能修改一次状态
            _this.promiseStatus = REJECTED;//修改状态为失败
            _this.promiseResult = reason;//结果为reason 失败的原因
            //放入队列中执行失败的回调 promise回调为微任务 不是同步执行
            setTimeout(function () {
                _this.callbacks.forEach(callback => {
                    callback.onRejected(_this.promiseResult);
                })
                clearTimeout(this);
            })

        }

        try {
            executor(resolve, reject);//执行executor同步函数
        } catch (e) {//如果执行器抛出异常 Promise对象变为失败状态
            reject(e);
        }

    }

    /**
     * Promise原型上的then方法
     * 执行成功和失败的回调函数
     * @param onResolved 成功的回调函数(value)=>{}
     * @param onRejected 失败的回调函数(reason)=>{}
     * @return 一个新的Promise对象 状态由onResolved/onRejected执行结果决定
     */
    Promise.prototype.then = function (onResolved, onRejected) {

        const _this = this;

        return new Promise((resolve, reject) => {

            if (typeof onResolved !== 'function') {
                onResolved = (value) => value;
            }

            if (typeof onRejected !== 'function') {
                onRejected = reason => {
                    throw reason;
                }
            }

            function action(carry) {

                try {
                    let result = carry(_this.promiseResult);
                    if (result instanceof Promise) { //判断返回值是否为Promise
                        result.then(resolve, reject);//执行内部的Promise
                    } else {
                        resolve(result);//返回结果
                    }
                } catch (e) {
                    reject(e);
                }

            }

            if (this.promiseStatus === PENDING) { //异步任务 将回调保存起来 等异步任务结束执行回调

                this.callbacks.push({ //then方法可多次执行，所以需要用数组保存
                    onResolved: function () {
                        action(onResolved);
                    },
                    onRejected: function () {
                        action(onRejected);
                    }
                })

            }

            if (this.promiseStatus === FULFILLED) {
                action(onResolved);
            }

            if (this.promiseStatus === REJECTED) {
                action(onRejected);
            }

        })


    }

    /**
     * Promise原型上的catch方法 是then的语法糖
     * 指定失败的回调函数
     * @param onRejected
     * @return Promise 返回一个新的Promise对象
     */
    Promise.prototype.catch = function (onRejected) {

        return this.then(undefined, onRejected);

    }

    /**
     * Promise函数对象的resolve方法
     * @param value value成功的值 如果为Promise对象，则为Promise对象的状态
     * @return Promise
     */
    Promise.resolve = function (value) {

        return new Promise((resolve, reject) => {

            if (value instanceof Promise) { //如果返回的是Promise对象，则继续回调
                value.then(resolve, reject);
            } else {
                resolve(value);
            }

        })

    }

    /**
     * Promise函数对象的reject方法
     * @param reason 失败的原因
     * @return
     */
    Promise.reject = function (reason) {

        return new Promise((resolve, reject) => {
            reject(reason);
        })

    }

    /**
     * Promise函数的all方法
     * @param promises
     * @return Promise
     *  一个promise，只有当所有的promise都成功才成功，有一个失败就失败，返回失败的原因
     */
    Promise.all = function (promises) {

        return new Promise((resolve, reject) => {
            let arr = [], index = 0;
            promises.forEach((v, i)=> {
                Promise.resolve(v).then(value => { //防止传过来的不是promise对象
                    arr[i] = value;
                    index++;
                    if (index === promises.length) {
                        resolve(arr);
                    }
                }).catch(reason => {
                    reject(reason);
                })
            })
        })
    }

    /**
     * Promise函数的race方法
     * @param promises
     * @return Promise
     */
    Promise.race = function (promises) {

        return new Promise((resolve,reject)=>{

            promises.forEach(v=>{
                Promise.resolve(v).then(value=>{
                    resolve(value);
                }).catch(reason=>{
                    reject(reason);
                })
            })

        })

    }

    window.Promise = Promise;

})(window)

~~~



