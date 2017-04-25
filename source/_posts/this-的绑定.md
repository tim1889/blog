---
title: this 的绑定
date: 2017-04-25 13:39:25
tags: js
---

## this 的默认调用

this 默认调用于全局

```
function fun() {
    console.log(this.name);
}

var name = 'Tim';

fun()   // tim
```


## this 的隐式绑定

决定于最后调用函数的上下文

```
function fun() {
    console.log(this.name)
}

var obj_1 = {
    name: 'Joey',
    fun: fun
}
obj_1.fun()  //Joey，最后调用 fun  的是 obj_1

var obj_2 = {
    name: 'Allen',
    fun: obj_1.fun
}
obj_2.fun()  //Allen，最后调用 fun  的是 obj_2

var obj_3 = {
    name: 'Alice',
    fun: obj_1
}
obj_3.fun.fun()  //Joey，最后调用 fun  的是 obj_2

var bar = obj_1.fun;

bar()   //undefind，最后调用 fun  的是 global，而 global 中并未定义 name
```

## this 的显示绑定

- call

    绑定在 call() 中传入的第一个参数上 ` fun.call(obj, arg1, arg2,...) ` fun 的 this 绑定在 obj 上 

    ```
    var obj = {
        name: 'Jonnhy'
    }

    function fun() {
        console.log(this.name);
    }

    fun.call(obj)  //Jonnhy
    ```

- apply

    同 call ，不同在于传入的第二个参数是一个数组 `apply(obj, arguments)`
   
    ```
    function Obj(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    function Fun(name, age, gender) {
        Obj.apply(this, [name, age, gender]); 
    }

    var fun = new Fun('Tim', '18', 'female');

    console.log(fun.name, fun.age, fun.gender);
    ```
    apply 将 Obj 的 this 绑定在了 fun 上 （new 的绑定见下）

- bind


## new 新对象绑定

this 绑定在 新创建的对象上

```
function Anmail(name) {
    this.name = name;
}

var cat = new Animal('cat');

console.log(cat.name)  //cat， this 被绑定在了 cat 上
```
