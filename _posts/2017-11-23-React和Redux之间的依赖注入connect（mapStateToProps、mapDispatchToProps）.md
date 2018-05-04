---
layout: post
title: "React和Redux 之间的依赖注入connect（mapStateToProps、mapDispatchToProps）"
date: 2017-11-23
description: "React和Redux 之间的依赖注入connect（mapStateToProps、mapDispatchToProps）"
tag: React
---

﻿在理解react-redux通过connect连接的关系之前，需要重温下组件的概念，组件分为两大类：展示组件和容器组件。展示组件就是用来显示UI的普通组件，不涉及业务逻辑和redux。容器组件的概念不容易理解，但它与展示组件之间却存在着明显不同特征。

**1. 容器组件**
容器组件是使用 store.subscribe() 从 Redux state 树中读取部分数据，并通过 props 来把这些数据提供给要渲染的组件。

> * 容器组件特征：
>  - 负责管理数据和业务逻辑，不负责 UI 的呈现
>  - 带有内部状态
>  - 使用 Redux 的 API
>
> * 展示组件特征：
>  - 只负责 UI 的呈现，不带有任何业务逻辑  
>  - 所有数据都由参数（this.props）提供
>  - 不使用任何 Redux 的 API

**2. connent()函数生成容器组件**

```
import { connect } from 'react-redux'

const VisibleTodoList = connect(
	[mapStateToProps], //参数1将 store 中的数据作为 props 绑定到组件上

	[mapDispatchToProps], //参数2将 action 作为 props 绑定到组件上。

	[mergeProps], //参数3用于自定义merge流程，将stateProps 和 dispatchProps merge 到parentProps之后赋给组件。通常情况下，你可以不传这个参数，connect会使用 Object.assign。

	[options] //如果指定这个参数，可以定制 connector 的行为。一般不用。
)(TodoList)

export default VisibleTodoList
```
上面代码中，TodoList 是 展示组件，VisibleTodoList 就是由 React-Redux 通过connect方法自动生成的容器组件。

**3.mapStateToProps()将state节点注入到与view相关的组件上**

使用 React Redux 库的 connect() 方法来生成容器组件前，需要先定义 mapStateToProps 这个函数来指定如何把当前 Redux store state 映射到展示组件的 props 中。例如，VisibleTodoList 需要计算传到 TodoList 中的 todos，所以定义了根据 state.visibilityFilter 来过滤 state.todos 的方法，并在 mapStateToProps 中使用。

```
const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
  }
}

const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
```

**4.mapDispatchToProps()将需要绑定的响应事件注入到组件上**

除了读取 state，容器组件还能分发 action。类似的方式，可以定义 mapDispatchToProps() 方法接收 dispatch() 方法并返回期望注入到展示组件的 props 中的回调方法。它可以是一个函数，也可以是一个对象。如果mapDispatchToProps是一个函数，会得到dispatch和ownProps（容器组件的props对象）两个参数。

例如，我们希望 VisibleTodoList 向 TodoList 组件中注入一个叫 onTodoClick 的 props 中，还希望 onTodoClick 能分发 TOGGLE_TODO 这个 action：

```
const mapDispatchToProps = (
  dispatch,
  ownProps
) => {
  return {
    onTodoClick: () => {
      dispatch({
        type: 'SET_VISIBILITY_FILTER',
        filter: ownProps.filter
      });
    }
  };
}
```

```
function mapDispatchToProps(dispatch){
    return {
        ...bindActionCreators(action, dispatch)
    }
}
// mapDispatchToProps()函数的bindActionCreators、action需要引入
    // import * as action from '../action';
    // import { bindActionCreators } from 'redux';



------------------------------------
------------------------------------
// 多个action 引入
import * as action from '../action';
import * as action2 from '../../../inde/action';
function mapDispatchToProps(dispatch){
    return {
        ...bindActionCreators(action, dispatch)
        ...bindActionCreators(action2, dispatch)
    }
}

------------------------------------
------------------------------------
// 引入一个action里面的多个方法
import { activeOrAbandonedApproval, showSeller, getAddOrderDiscounts } from '../orderInfo/action'
function mapDispatchToProps(dispatch) {
  return {
    ...bindActionCreators({ activeOrAbandonedApproval, showSeller, getAddOrderDiscounts }, dispatch)
  }
}

//示例来自：http://blog.csdn.net/genius_yym/article/details/64130120
```
