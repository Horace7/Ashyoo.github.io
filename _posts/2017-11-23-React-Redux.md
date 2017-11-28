---
layout: post
title: '关于React-Redux'
subtitle: '项目中一直在用React-Redux,抽空整理一下'
date: 2017-11-23
categories: React
tags: React Redux
cover: 'https://ws3.sinaimg.cn/large/006tNc79gy1flwqnzvk1qj31cg0fe0tx.jpg'
catalog: true
---

项目中一直在用React-Redux,抽空整理一下,加深理解

## React
React是单向数据流,没有数据向上回溯的能力,只能向下分发
> React中有props和state

* `props`是父级传过来的属性

```javascript
// 父组件
  _hideAddress = () => {
    console.log('o')
  }
  <ModifyAddress
    hideAddress={this._hideAddress}
  />
// 子组件
  <Modal
    onClick={this.props.hideAddress}
  />
```

* `state`是组件内部自行管理的状态

```javascript
  this.state = {
    visible: false,
  }

  this.setState({
    visible: true,
  })
```

* 随着业务逻辑的增加,就会发现React无法让两个组件之间相互交流,拿到对方的数据

* 这个时候就只能将state放到公用的父组件中来管理,然后通过props分发 ==> reducer

* 而子组件去改变父组件的state,只能通过onClick触发父组件声明好的回调,
也就是说父组件需要提前声明好函数或方法描述state将如何改变,再将它作为属性传给子组件 ==> action

* 这样就可以想到一个方法,将所有的state放在顶层统一维护,然后分发给所有的组件

---

## Redux
Redux就作为一个专业的顶层将state分发给所有的组件

* action 声明式的数据结构，只提供事件的所有要素，不提供逻辑
* Redux 中只需把 action 创建函数的结果传给 dispatch() 方法即可发起一次 dispatch 过程

```jsx
// action
const addTodo = (text) = {
    type: 'ADD_TODO',
    text
  }
dispatch(addTodo('new text'))
```
* reducer 根据传入的 当前state 和 action , 返回一个新的 state
* action 只是描述事情发生了,而没有说明如何更新state,这就是reducer要做的事情

```jsx
// 引入action
    import { todoApp } from './actions'
// 初始化 state
    const initialState = {
      text: ''
    }
// 现在可以处理 ADD_TODO。需要做的只是改变 state 中的 text
    const todoApp = (state = initialState, action) = {
      switch (action.type) {
        case 'ADD_TODO':
          return { ...state, text}
        default:
          return state
      }
    }
// 需要注意的是:
// 1. 不要改变state.
// 2. 如果遇到未知的action,返回久的state
```
* store 通过dispatch(action)来更新state

---

## React-Redux
一般不会直接把这两个库直接拿来用,而是用react-redux

*react-redux提供一个**Provider**和**connect***

* `Provider`是一个普通组件,可以作为app的分发点,只要有store属性就可以了,他会将state分发给所有被connect的组件

`export default connect(mapStateToProps, mapDispatchToProps)(ComponentName)`

* `connect`先接受两个参数**数据绑定(mapStateToProps)**和**事件绑定(mapDispatchToprops)**再接收一个**将要绑定的组件**

>  mapStateToProps 你需要绑定一个函数,它的参数是state,返回你想要的几个值

```jsx
const mapStateToProps = (state) => {
  return {
    newText: state.todoApp.text,
  }
}
```
> mapDispatchToProps 声明好的action作为回调,它的参数是dispatch

```jsx
// 绑定所有action以及参数的dispatch，可以作为属性在组件里面使用
const mapDispatchToProps = (dispatch) => ({
  dispatch
})
// 在组件中使用
this.props.dispatch(action(addTodo()))
```

