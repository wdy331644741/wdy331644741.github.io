---
title: react connect函数
---

> connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])


**mapStateToProps**(state, ownProps) : stateProps
这个函数允许我们将 store 中的数据作为 props 绑定到组件上
第一个参数就是 Redux 的 store



**mapDispatchToProps**(dispatch, ownProps): dispatchProps
将 action 作为 props 绑定到组件上

_bindActionCreators_() ?




**[mergeProps(stateProps, dispatchProps, ownProps): props]**

[文档翻译](http://taobaofed.org/blog/2016/08/18/react-redux-connect/)
[connect的五种用法](https://blog.benestudio.co/5-ways-to-connect-redux-actions-3f56af4009c8)
[https://github.com/reduxjs/react-redux/blob/master/docs/api/connect.md](https://github.com/reduxjs/react-redux/blob/master/docs/api/connect.md)

