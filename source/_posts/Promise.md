---
title: Promise 个人笔记
date: 2017-06-06 17:50:33
tags: js
---

## Promise 的用途
在传统异步请求中，在完成第一个异步操作的时候，我们并不能马上执行第二个操作的时候，只有将第二个操作的相关代码写入第一步的会掉里面。若异步操作有多个，那么就会嵌套很多代码。而 Promise 解决了这种回调地狱的问题，将这种回调的方式改写成链式，把异步用同步的方式表的了出来。
<!--more-->
### 构造一个 Promise
```js
const promise = new Promise((resolve, reject) => {
    resolve("I'm still standing yeah yeah yeah");
    reject(new Error('You starting down the road leaving me again'));
});

promise.then((value) => {
    console.log(value);
},(error) => {
    console.log('then 的写法：',error)
}).catch((error) => {
    console.log('catch 的写法：',error);
})
```
异步成功后会以 `resolve()` 函数传递出去，之后会执行 then 中的第一个函数。失败后通过 `reject()` 传递出去，在 then 中的第二个函数或者catch中执行。如果已经先有了 resolve 状态 reject 就不会执行。
> Promise 的状态一旦改变，就永久保持该状态，不会再变了。

### 转换为Promise
```js
const p = Promise.resolve('la la land');

const p2 = Promise.reject('Ha da da dee da hada hada da da');
```
可以将参数转为一个 promise 对象。这两个参数分别立刻执行 resolve , reject 状态。使用 catch 可以捕获到每一个 promise 的 reject 状态，then方法指定的回调函数，如果运行中抛出错误，也会被catch方法捕获。而 then 的第二个参数形式只能捕获到当前的 reject 状态。

## Promise 的链式调用
由于 promise 内置的 then 方法最后返回自身，所以可以继续调用下一个 `then()` 或 `catch()` 方法。

### Promise 处理多个异步操作
* 多个异步完成后执行下一步
当需要在等待多个异步都返回状态之后，在执行下一个步骤的情况下，可以使用 promise 的 `all()` 方法。
```js
const allPromise = Promise.all([p1, p2, p3]);

allPromise.then((arr) => {
    console.log(arr);
}).catch((error) => {
    console.log(error);
})
```
当返回了 resolve 状态，执行 then 将会得到一个数组参数，参数是所有的 Promise 的 resolve 返回的值的组合。 如果其中有一个有错误，就会返回 reject 状态。参数是错误的那一个的信息。

* 多个异步其中一个完成后执行下一步
当需要判断多个 promise 哪一个优先完成，然后执行之后的事件的时候，可以使用到 promise 的 `race()` 方法。
```js
const oneOfPromise = Promise.rece([p1, p2, p3]);

oneOfPromise.then((value) => {
    console.log(value);
}).catch((error) => {
    console.log(error);
});
```
只要所有的 promsie 之中有一个实例率先改变状态，状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给它的回调函数。

## Promise 管理同步 | 异步流程
promsie 的执行是在这一轮事件执行完之后执行的，所以当需要使用同步的方法的时候，事件的执行顺序就可能会不尽人意。[关于 try]('http://cryto.net/~joepie91/blog/2016/05/11/what-is-promise-try-and-why-does-it-matter/')
```js
console.log('~~~bui');
Promise.resolve().then(() => console.log('~~~bui bui'));
console.log('~~~bui bui bui');
// ~~~bui
// ~~~bui bui bui
// ~~~bui bui
```
`try()` 方法可以让同步的方法同步执行。
```js
console.log('~~~bui');
Promise.try('v').then(() => console.log('~~~bui bui'));
console.log('~~~bui bui bui');
// ~~~bui
// ~~~bui bui
// ~~~bui bui bui
```

## other
* `done()` 在最末尾调用保证捕获到所有的异常
* `finally()` 不管 promise 返回怎样的状态都会执行