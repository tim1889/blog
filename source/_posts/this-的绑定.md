---
title: this 的绑定
date: 2017-04-25 13:39:25
tags: js
---

> this 的调用总是决定于最后调用那个函数的上下文！
<!--more-->

## this 
```js
function foo(num) {
    console.log( "foo: " + num );
    // 记录foo 被调用的次数
    this.count++;
}
foo.count = 0;
var i;
for (i=0; i<10; i++) {
    if (i > 5) {
        foo( i );
    }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9
// foo 被调用了多少次？
console.log( foo.count ); // 0 -- WTF?
```
- 为什么 count 的输出和预期的不符？
- 为什么 count 是全局的，并且值是 NaN?


## this 的默认调用
> this 默认调用于全局，严格模式下绑定到 undefined

```js
function fun() {
    console.log(this.name);
}
```
声明一个函数 fun

```js
var name = 'Tim';
fun()   // tim
```
在全局中声明了变量 name ,此时在全局中引用了 `fun()` 此时 函数中的 name 就是指向的是全局中的 name


## this 的隐式绑定
> 决定于最后调用函数的上下文

```js
var obj_1 = {
    name: 'Joey',
    fun: fun
}
obj_1.fun() //Joey
```
最后调用 fun 的是 obj_1

```js
var obj_2 = {
    name: 'Allen',
    fun: obj_1.fun
}
obj_2.fun()  //Allen
```
最后调用 fun 的是 obj_2

```js
var obj_3 = {
    name: 'Alice',
    fun: obj_1
}
obj_3.fun.fun()  //Joey
```
最后调用 fun 的是 obj_2

```js
var bar = obj_1.fun;

bar()   //undefind
```
最后调用 fun  的是 global，而 global 中并未定义 name


## this 的显示绑定
> 显示绑定的优先级高于隐示绑定，默认优先级最低。 

- call
   > 绑定在 call() 中传入的第一个参数上 ` fun.call(obj, arg1, arg2,...) ` fun 的 this 绑定在 obj 上 

    ```js
    var obj = {
        name: 'Jonnhy'
    }

    function fun() {
        console.log(this.name);
    }

    fun.call(obj)  //Jonnhy
    ```

- apply
    > 同 call ，不同在于传入的第二个参数是一个数组 `apply(obj, arguments)`
   
    ```js
    function Obj(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    function Fun(name, age, gender) {
        Obj.apply(this, [name, age, gender]); 
    }

    var fun = new Fun('Tim', '18', 'female');

    console.log(fun.name, fun.age, fun.gender); //Tim, 18, female
    ```
    apply 将 Obj 的 this 绑定在了 fun 上 （new 的绑定见下）

- bind


## new 新对象绑定

> this 绑定在 新创建的对象上

```js
function Cat(name) {
    this.name = name;
    this.food = 'fish';
}

var yellowCat = new Cat('Kittey');

console.log(yellowCat.name);  //kittey
```
this 被绑定在了 yellowCat 上，yellowCat 默认继承了 Cat 类的属性。

```js
Cat.prototype.eat = function () {
    console.log(this.food);
}
yellowCat.eat(); //fish
```
在 Cat 的原型链上创建一个吃的方法,里面绑定了食物。
调用这个食物方法的是 yellowCat , tish 指向的的是 yellowCat。所以输出 fish

```js
yellowCat.food = 'banana';
yellowCat.eat();  //banana
```
调用这个 `eat()` 的是 yellowCat , tish 指向的的是 yellowCat, yellowCat 的 food 已经被修改 

```js
var blueCat = new Cat('Tim');

blueCat.eat(); //fish
```
调用 `eat()` 的是 blueCat, this 指向 blueCat, blueCat 继承了 Cat 的所有属性，由于 Cat 内部的 food 依旧是 fish，所以 blueCat 的 food 依旧是 fish


## 箭头函数中的 this
> 在箭头函数中 this 是绑定在创建的时候的
```js
function a() {
    const name = 'a';
    const fun = () => {
        console.log(this.name);
    }
    fun();
}
```

## 小结

### 判断 this 的绑定
1. 是否在 new 中调用
2. 是有 apply ， call 或 bind 的显示绑定
3. 是否有隐式的绑定
4. 默认绑定 window，严格模式下绑定到 undefined 
 