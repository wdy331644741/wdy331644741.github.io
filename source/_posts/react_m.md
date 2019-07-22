---
title: react组件生命周期
date: 2018-04-27 23:00:34
categories:
- 前端
tags:
- react
#top: true
---



## UI=render(state)

### react生命周期

## 1. 装载
- **constructor**构造
- **getInitialState** 同构造（React.createClass） 弃用
- **getDefaultProps** 同构造（React.createClass） 弃用
- **componentWillMount**()  在render之前 这里的东西都可以放在构造里面
- render 纯函数
- **componentDidMount**() 在render之后 当所有的render完成之后 。在客户端浏览器上面全部执行（可用其他UI库jquery）

## 2. 更新 -当用户交互页面时，通过修改state和props
- componentWillReceiveProps(nextProps);
> 当父 组件的render 函数被调用， 在 render函数 里面被渲染 的子组件就会经历 更新过程， 不管父组件传给子组件 的 props 有没有改变， 都会 触发 子组件的componentWillReceiveProps函数。
	
- shouldComponentUpdate(nextProps, nextState){return true;} 在setState  之前执行。
> 在更新过程中， React 库首先调用 shouldComponentUpdate 函数， 如果这个函数返回true， 那就会继续更新 过程， 接下来 调用render 函数； 反之， 如果得到 一个 false， 那就立刻停止更新过程.

- 当上面函数返回true时。依次调用：
	componentWillUpdate
	render
	componentDidUpdate

## 3. 卸载
- componentWillUnmount
- 要配合componentDidMount 使用。要不然会出现内存泄漏



> tip:

- [ ] forceUpdate()；刷新render

![image](https://user-images.githubusercontent.com/9856659/51670011-a8777280-2000-11e9-96ba-447a3770872f.png)

