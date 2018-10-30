---
title: react 仿简书知识点总结
categories: 前端学习笔记
tags: react
comment: true
date: 2018-10-28 20:26:07
---
这里是一篇对 react 仿简书视频教程的总结。

<!-- more -->

- [react官网](https://reactjs.org/)
- [项目代码地址](https://github.com/amenzai/react-jianshu)

## react 初探

- 简介
- 开发环境
  - cdn
  - create-react-app
- 工程目录
- 组件
- JSX语法

## react 基础

- 响应式设计思想
  - setState()
- 事件绑定
  - bind() or 箭头函数

```jsx
// 输入框中输入 html 标签时，加此属性不自动转义
<li dangerouslySetInnerHTML={{__html: item}}></li>

this.setState((prevState) => ({
    list: [...prevState.list, prevState.inputValue]
}))

constructor(props) {
	super(props)
    this.handleClick = this.handleClick.bind(this)
}
```

父组件通过属性向子组件传递方法时，父组件方法需要 bind(this)。

react 的一些思考：

- react：声明式开发
- jquery：命令式编程
- 可以与其它框架共存：#root标签之外的 DOM 不受 react 管理
- 单向数据流：react 不提倡子组件更改父组件数据
- 视图层框架
- 函数式编程

## react 进阶

第一点：

- PropTypes
- Default Props
- [参考地址](https://reactjs.org/docs/typechecking-with-proptypes.html)

第二点：

- props
- state
- render函数

当组件 state or props 改变，render 函数重新执行。

当父组件的 render 函数被运行时，它的子组件的 render 都将被运行一次

第三点：虚拟 DOM

就是个 JS 对象

[参考文章](http://amenzai.me/2018/08/14/interview3/)

虚拟 DOM 使得跨端应用得以实现

第四点：diff 算法

- 同层比对
- 循环中的 key 值不要使用 index

第五点：ref

```jsx
<span ref="{(el) => {this.span = el}}"></span> // this.span 就是 span 标签

this.setState(()=>{},()=>{}) // 在第二个参数获取 DOM 才正确
```

尽量不使用 ref，毕竟应该数据驱动视图

第六点：生命周期函数

某一个时刻执行的函数。

- init：props and state
- mounting
  - componentWillMount
  - render
  - componentDidMount
- updation
- unmounting

第七点：生命周期函数使用场景

避免子组件做无畏的渲染：

```jsx
shouldComponentUpdate(nextProps, nextState) {
    if (nextProps.content !== this.props.content) {
      return true
    } else {
      return false  
    }
}
```

第八点：发送ajax请求

在 componentDidMount() 生命周期函数内调用，使用 axios 库。

第九点：使用 charles 实现本地数据

[下载 charles](https://www.charlesproxy.com/latest-release/download.do)

新建 json 数据文件，在 charles 中创建相应代理。

第十点：react 实现 css 过渡动画

让一个 DOM 在显示 隐藏中切换（opacity），借助 CSS3 的 transition 属性即可实现。

第十一点：使用 css 动画效果

- 使用 @keyframes

- 方法就是添加类

第十二点：使用 react -transition-group

[使用方法](https://reactcommunity.org/react-transition-group/)

```jsx
// in 就是根据true false 判断 入场或出场动画
// unmountOnExit 出场后 移除DOM
// appear 页面显示 展示入场动画
<CSSTransition
    in={focused}
    timeout={200}
    classNames="slide"
    unmountOnExit
    onEntered={(el) => {el.style.color="blue"}}
    appear={true}
>
    <div className="slide-enter">123</div>
</CSSTransition>
```

- slide-enter slide-appear
- slide-enter-active fade-appear-active
- slide-exit
- slide-exit-active

如何实现列表中每插入一项都添加动画，方法可见上面链接的文档。

## redux 

[参考文章](http://amenzai.me/2018/03/28/redux/)

- store 是唯一
- 只有 store 能改变自己的内容
- reducer 是纯函数：只要函数体有 Date、ajax、定时器等都不是纯函数

一些 API：

- createStore()
- store.dispatch()
- store.getState()
- store.subscribe()

UI组件和容器组件：

- UI：页面渲染
- 容器：业务逻辑

容器组件编写业务逻辑，向 UI 组件通过属性传入所需数据，即 UI 只提供显示。

无状态组件：

就是个函数。

```jsx
const Demo = (pros) => {
	return (
    	<div>123{props.content}</div>
    )
}
```

处理异步的 redux 中间件:

redux-thunk：[使用方法](https://github.com/reduxjs/redux-thunk)

与 redux-devtools 结合使用：[查看地址](https://github.com/zalmoxisus/redux-devtools-extension#12-advanced-store-setup)

redux 中间件就是在 dispatch(action) 时，发现 action 是函数的话，会进行处理，然后再回到 dispatch(action)，当 action 对象时，store 接管，给 reducer 处理。

另一个异步中间件 redux-saga：[使用方法](https://github.com/redux-saga/redux-saga)

## react-redux

- Provider：传递 store
- connect：组件中拿 store

```jsx
// state 挂载到 props 上
const mapStateTOProps = (state) => {
    return {
		test: state.test
    }
}
// store.dispatch 挂载到 props 上
const mapDispatchToProps = (dispatch) => {
    return {
        handleLogin() {
            
        }
    }
}
export default connect(mapStateTOProps, mapDispatchToProps)(DemoComponent)
```

## 实战

1、

使用 style-components 对每个组件样式进行管理：

```bash
yarn add style-components
```

在 index.js 文件中引入 style.js 文件

2、

immutable.js：[使用方法](https://github.com/facebook/immutable-js)

```js
// 子 reducer.js
import { fromJS } from 'immutable';
const defaultState = fromJS({
	focused: false,
	mouseIn: false,
	list: [],
	page: 1,
	totalPage: 1
});
export default (state = defaultState, action) => {
	switch(action.type) {
		case constants.SEARCH_FOCUS:
			return state.set('focused', true);
		case constants.SEARCH_BLUR:
			return state.set('focused', false);
		case constants.CHANGE_LIST:
			return state.merge({
				list: action.data,
				totalPage: action.totalPage
			});
		case constants.MOUSE_ENTER:
			return state.set('mouseIn', true);
		case constants.MOUSE_LEAVE:
			return state.set('mouseIn', false);
		case constants.CHANGE_PAGE:
			return state.set('page', action.page);
		default:
			return state;
	}
}

// 总 reducer.js
import { combineReducers } from 'redux-immutable';

// index.js
const mapStateToProps = (state) => {
	return {
		focused: state.getIn(['header', 'focused']),
		list: state.getIn(['header', 'list']),
		page: state.getIn(['header', 'page']),
		totalPage: state.getIn(['header', 'totalPage']),
		mouseIn: state.getIn(['header', 'mouseIn']),
		login: state.getIn(['login', 'login'])
	}
}
```

3、

避免无意义请求发送：

根据每次触发事件时的参数来判断这个请求是否需要再次发送。

4、

每次开发，先分析页面布局，能抽的组件抽一下，然后编写样式，使用 redux 编写一些静态数据看效果，然后设置action，异步请求数据。

5、

获取 url 中的参数：

- 动态路由：/detail/:id(通过 this.props.match.params.id 获取)
- query 形式传递：this.props.location.search

6、

异步组件：[react-loadable](https://github.com/jamiebuilds/react-loadable)

7、

不是 `<Route path='/detail/:id' exact component={Detail}></Route>` 里面的组件，想要获取路由信息，需要这样：

```js
import { withRouter } from 'react-router-dom'
@withRouter
@connect(
	null,
	{ loadData }
)
```

8、

项目部署服务器