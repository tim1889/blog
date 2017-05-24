---
title: 关于变量
date: 2017-05-24 11:34:58
tags: js
---

<!--more-->
## 变量声明

* let, const 只在当前作用域内有效
    * test1
    ```js
    let a = 'a';
    var c = 'c';
    {
        let a = 'aa';
        var c = 'cc';
        console.log(a) //aa
        console.log(c) //cc
    }
    console.log(a); //a
    console.log(c); //cc
    ```
    在 es6 中 let 声明的值只在当前作用域内才会有效。在块中声明的 a 不会影响到块外的 a 的值。

    * test2
    ```js
    const ulBox = document.getElementsByTagName('ul');
    for (var i = 0; i < 5; i++) {
        const btn = document.createElement('li');
        btn.innerHTML = i;
        btn.onclick = function () {
            console.log(i);
        }
        ulBox[0].appendChild(btn);
    }
    ```
    点击每一个 `li` 都将会输出 5 ，由于 i 是用 var 声明的，在全局内 i 都有效且在全局内只存在一个 i。所以 点击每一个 `li` 的时候都获取的是循环后最终得到的 i 的值
    
    ```js
    const ulBox = document.getElementsByTagName('ul');
    for (let i = 0; i < 5; i++) {
        const btn = document.createElement('li');
        btn.innerHTML = i;
        btn.onclick = function () {
            console.log(i);
        }
        ulBox[0].appendChild(btn);
    }
    ```
    这里的 i 是 用 let 声明的。let 声明的 i 只在当前作用域内有效。相当于每一次的 i 都是重新声明的。（js 引擎可以记住上一轮循环的值）

* const 声明的值是常量不可改变。实际上 const 声明的变量指向的内存地址不变。
    ```js
    const obj = {};     //指向这个 obj 的指针不变
    obj.a = 'a';        //使用 Object.freeze() 方法可以将对象冻结

    obj = {b: 'b'}      //报错，不可以改变 obj 本身
    ```    
* 变量将不会被提升到顶部
* 相同的作用域内不可重复声明
* 函数声明会被提升到所在作用域顶部
* 申明变量的方法一共 6 种： var const let function import class

## 变量赋值

* 解构赋值
    * 数组解构
    ```js
    const [a,b,[c], , ...e] = ['aa', 'bb', ['cc', 'ccc'], 'dd', 'ee', 'ff', 'gg'];
    //a = 'aa', b = 'b', c = 'cc', e = ['ee', 'ff', 'gg']
    ```
    对应位置匹配，左边便会被赋予相同的值。只要模式匹配了，就可以解构成功（只能匹配一部分,解构依然可以成功）。
    * 对象解构
    ```js
    const {a, b, arr: [c, {d}]} = {b: 'bb', a: 'aa', arr: ['cc', {d: 'dd'}]}
    // a = 'aa', b = 'bb', c = 'cc', d = 'dd'
    ```
    对应的属性名（）相同便会被赋值，位置不必相同
    * 解构中默认赋值 `const {a = 3} = {a: undefined}` 的属性值必须严格为 undefined
    