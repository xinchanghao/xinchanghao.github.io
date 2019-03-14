---
layout: post
title: "关于Warning_ setState(...)_ Can only update a mounted or mounting component.的解决方案"
date: 2019-01-12
description: "关于Warning_ setState(...)_ Can only update a mounted or mounting component.的解决方案"
tag: React
---


## 一、原因

在做项目的时候，控制台一直报一个错误。最初以为是脏数据导致的key重复问题，后来发现这个问题一直存在。细看，发现具体错误提示如下：
![错误描述](https://img-blog.csdnimg.cn/20190112154612147.png)

略经思考，脑子里首先闪现的问题原因是，setState异步函数在组件生命周期结束后异步抛出。经此设想，快速切换两个路由，此问题复现，即证明了猜想是正确的。

> react中快速切换路由时报此错误，是由于在组件开始挂载（componentWillmount）或者组件挂载之后（componentDidmount）进行了异步操作，比如ajax请求或者setState操作、定时器等。当进行快速切换路由操作的时候，组件已经卸载（unmounted），此时setState等异步操作中的callback回调执行时没有值，从而抛出报错。

## 二、解决方法
##### 1.在组件挂载时需要进行异步操作的组件封装为公共组件
在**满足业务场景**的情况下，将此异步操作封装在最外层的公共组件中，这样就不会出现所谓的组件卸载情况。但是，这种方法明显不满足所有业务场景，而且提升为公共组件也可能造成不必要的重复渲染。

##### 2. 重写setState()函数
这种方法基本能满足所有使用场景。当组件卸载时就会执行该方法，当组件重新挂载时会继承父组件的setState方法，从而不影响页面的渲染。

不过该方法也存在弊端，在集成的子组件中能修改父组件的setState方法，只是在javascript的语法中很适用，所以最好只在出现此bug的页面中使用。

```
componentWillUnmount = () => {
    this.setState = (state,callback)=>{
      return;
    };
}
```
##### 3.结合flag使用异步操作

```
componentDidMount = () => {
    this._flag = true;
    $.ajax('请求接口',{})
        .then(res => {
            if(this._flag){
                this.setState({
                    pageState:true
                })
            }
        })
        .catch(err => {})
}
componentWillUnMount = () => {
    this._flag = false;
}
```
##### 4.在卸载的时候对所有的操作进行清除（推荐）
当我们使用异步操作的时候，往往会忽略后续的清除工作，而事实上，最规范的做法是我们应该对组件中使用的不必要异步操作进行清除。 例如：abort你的ajax请求或者清除定时器。

```
componentDidMount = () => {
    //ajax请求
    $.ajax('请求接口',{})
        .then(res => {
            this.setState({
                  pageState:true
            })
        })
        .catch(err => {})
    //setTimeout定时器
    timer = setTimeout(() => {
        //dosomething
    },1000)
}
componentWillUnMount = () => {
    //ajax请求
    $.ajax.abort()
    //setTimeout定时器
    clearTimeout(timer)
}
```
...

转载请注明：[化风的博客](http://ChhXin.github.io) » [关于Warning_ setState(...)_ Can only update a mounted or mounting component.的解决方案](/2019/01/关于Warning_ setState(...)_ Can only update a mounted or mounting component.的解决方案/)                   
