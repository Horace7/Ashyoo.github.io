---
layout: post
title: 'JavaScript不要错过啊'
subtitle: '整理一下JavaScript中一些概念性的东西'
date: 2018-01-01-JavaScript
categories: Es6
tags: JavaScript
catalog: true
---

一些很基础的东西,在看源码啥的时候可以明白很多<br />
在这里敲一敲code一哈<br />
看到哪里写到哪里吧

***

## 作用域
* es3/es5中没有块级作用域的概念,只有局部作用域和全局作用域(eval不提)

```javascript
var a = 10; // 全局作用域
(function() {
    var b = 20; // 局部作用域
})();
```
* es6中存在块级作用域

```javascript
const arr = [1, 2, 3];
for(let i = 0; i < arr.length; i++) {
    ...
}
```
## 变量提升
* 每一个变量都会被前置声明,值是undefined
* 函数声明的优先级高于变量声明

```js
alert(x) // function;

var x = 10;
alert(x) // 10
x = 20

function x() {}
alert(x) // 20
```

## this

> **`es3/es5`this的指向在函数创建的时候是决定不了的，在调用的时候才能决定，谁调用的就指向谁，this永远指向的是最后调用它的对象**

>**`es6`箭头函数中this的指向,是定义时this的指向(this用的是已经声明好的this)**


* 全局的this === window

* 一般函数的this也指向window(严格模式指向undefined)

* 作为对象方法的this 指向调用的对象

* 可以说this指向的永远是个对象, 不是调用时或者定义时的对象, 就是window

```js
var obj = {
    props: 99,
    func: function(){
        return this.props
    }
};

console.log(obj.func()); // 99

// -------------------再来一个-------------------------//
var obj1 = {
    bar: function() {
        return this.baz
    },
    baz: 1
}

(function(){
    console.log(typeof arguments[0]()) // undefined
})(obj1.bar)

// 注意调用obj1.bar的是arguments,所以this是指向arguments的
// 如果在传参的时候调用obj1.bar()的话,则会输出 number
```
* 构造函数中的this

```js
function MyClass(){
    this.a = 99;
};

var obj = new MyClass()

console.log(obj.a) // 99

```

* call/apply中的this

> call和apply的作用是一样的,都是在特定的作用域中调用函数，等于设置函数体内this对象的值，以扩充函数赖以运行的作用域。
> 
> 不同的是apply接受一个数组,而call接受单独的参数

```js
function add (c, d) {
    return this.a + this.b + c + d
};

var obj = {
    a: 1,
    b: 2
};

add.call(obj, 3, 4); // 1+2+3+4 = 10

add.apply(obj, [3, 4]); // 1+2+3+4 = 10

// 可以看出,add中的this是指向obj的
// 因为它可以改变方法中的this,所以能调用call和apply的只有方法
```

* bind

> bind 是返回对应函数，便于稍后调用；apply 、call 则是立即调用 。

```js
var altwrite = document.write;
altwrite("hello");
//报错，因为altwrite方法是在window上进行调用的，而window上是没有该方法的

//正确方法是
altwrite.bind(document)("hello")
```
## 闭包

## 原型
`prototype和__proto__ 略探一二`

* prototype和__proto__的区别
> prototype是函数才有的属性
> __proto__是每个对象都有的属性

```javascript
    var a = {}
    console.log(typeof a.prototype)  // undefined

    var a1 = function(){}
    console.log(typeof a1.prototype)  // object

    var B = function(){}
    var b = new B() 
    console.log(typeof B.prototype)  // undefined
```

## 继承
* call和apply的继承
`a.call(b,arg1,...) 说白了就是将a对象的方法加给b对象`

```js
function GirlFriend(name, from) {
    this.name = name;
    this.from = from;
    this.alertName = function () {
        alert('my name is' + this.name);
    }
    this.alertFrom = function () {
        alert('I come from' + this.from);
    }
}
 
function myDream(name, from, age) {
    GirlFriend.call(this, name, from);
    // GirlFriend函数在当前this下执行
    // GirlFriend中的this指向了当前的this
    // 也是就指向了myDream,所以myDream可以继承到GirlFriend中的属性和方法
    this.age = age;
    this.alertAge = function () {
        alert(this.age);
    }
}
 
var fack = new myDream("Black Widow", "China", "18");
fack.alertName();//Black Widow
fack.alertFrom();//China
fack.alertAge();//18

```


### 普通事件与绑定事件的区别--事件委托

`普通事件只支持单个事件，而事件绑定可以添加多个事件`

```html
<button id="btn"></button>
<script type="text/javascript">
    var btn=document.getElementById("btn");
    // 普通事件:
    btn.onclick=function(){
        alert("普通事件1");
    }// 不执行
    
    btn.onclick=function(){
        alert("普通事件2");
    } // 执行

    // 绑定事件:
    btn.addEventListener('click',function(){
        alert("绑定事件1");
    },false); // 执行

    btn.addEventListener('click',function(){
        alert("绑定事件2");
    },false); // 执行
</script>
```

