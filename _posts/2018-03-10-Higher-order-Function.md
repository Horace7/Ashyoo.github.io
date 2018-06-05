---
layout: post
title: '高阶函数'
subtitle: '高阶函数了解一下'
date: 2018-03-10
categories: Javascript
tags: React Javascript
catalog: true
---

简单了解一下高阶函数,为了方便理解高阶组件,以后抽空整理一下高阶组件

# 高阶函数

* 高阶函数就是可以把函数作为参数，或者是将函数作为返回值的函数

```javascript

function welcome(username) {
    console.log('welcome ' + username);
}

function goodbey(username) {
    console.log('goodbey ' + username);
}

function wrapWithUsername(wrappedFunc) {
    let newFunc = () => {
        let username = localStorage.getItem('username');
        wrappedFunc(username);
    };
    return newFunc;
}

welcome = wrapWithUsername(welcome);
goodbey = wrapWithUsername(goodbey);

welcome();
goodbey();

```



