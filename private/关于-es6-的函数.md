---
title: 关于 es6 的函数
date: 2017-06-01 10:13:33
tags: js
---

## 函数的参数

### 函数参数设定默认值
* es5 中给函数的参数设定默认值的方式要避免默认值为 `false` 非常麻烦
```js
function setDefault(x) {
    var x = x || 'Tim';
    console.log(x);
}
setDefault('undefined'); //Tim, （x 的默认值的布尔值是 false, 所以函数内部取了后面的值）
``` 
* es6 可以直接避免这个问题 
```js
funciton setDefault(x='Joey') {
    console.log(x);
}
setDefault('undefined'); //undefined
``` 

### 默认值的作用域
```js
var x = 1;
function log(x, y = x) {
    console.log(y); 
}
log(3) //3
```
可以将参数 log 的参数看作一个单独的作用域。这里 y 的值是 log 的参数 x 赋值的。x 是传递进来的 3 。

```js
var x = 1;
function log(y=x) {
    let x = 2;
    console.log(y)
}
log();
```


## 严格模式

## name 属性

## 箭头函数 与 this 绑定

## 尾化调用
