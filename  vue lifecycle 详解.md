<img src='https://raw.githubusercontent.com/peterpanBest/images-bed/master/lifecycle.png' width='500' height='1200' style='display:block;margin:0 auto;'>

##### 本文主依据 vue 2.0 来讲解 vue 的生命周期函数

<hr>

##### 什么是生命周期?

######  vue 的实例在被创建时，会有一个过程，在这个过程中可以进行一系列的操作，为方便地进行操作，vue 根据这个周期每个阶段的特点抽象出了一系列周期函数，利用这些周期函数可以实现很多功能。


##### 下面是钩子函数的具体介绍
<img src='https://github.com/peterpanBest/images-bed/blob/master/vue_lifecycle_table.png?raw=true' width='600' height='400' style='display:block;margin:0 auto;'>

<hr>

##### 钩子函数在项目中的不同作用：

##### beforeCreate : <br>
在这个钩子函数执行的时刻，el 和 data 没有初始化。转场动画 loading 可以放在这个钩子函数里面。 

##### created : <br>
属性初始化及绑定完成完成，DOM 还没有生成，$el 属性还不存在。在这个钩子函里面结束转场动画，一些在页面初始化时需要自执行的一些方法可以放在这里面，但是需要对 DOM 有操作的方法不要放在这里，因为这个时刻 DOM 还没有生成。

##### mounted : <br>
模板编译和挂载完成。可以将页面初始化需要请求数据的方法放在这里自执行。<br>
在这个钩子函数内如果需要对 DOM  进行操作，需放到 nextTick() 的回调里面进行。

##### beforeDestroy：<br>
组件销毁前会被执行。

<hr>

##### nextTick() 介绍 :<br>
官方文档解释：在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。<br>

```javascript
Vue.nextTick(function () {
    // DOM 更新了
})
```

###### vue 2.1.0 起新增 Promise 调用 <br>

```javascript
Vue.nextTick()
    .then(function () {
    // DOM 更新了
})
```

###### 在继续研究下去之前，我们先要知道这些概念 : <br>
###### vue DOM 是异步更新的，推荐一篇关于 <a href='http://www.ruanyifeng.com/blog/2014/10/event-loop.html'>"事件循环队列"</a>的文章，作者是大名鼎鼎的阮一峰老师。相信读完这篇文章，你对 event loop 以及 js 这门语言会有一个新的认识。

###### 我对事件循环做了一个总结：
###### （1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
###### （2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
###### （3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
###### （4）主线程不断重复上面的第三步。主线程的执行过程就是一个 tick，而所有的异步结果都是通过 “任务队列” 来调度被调度。 消息队列中存放的是一个个的任务（task）。 规范中规定 task 分为两大类，分别是 macro task 和 micro task，并且每个 macro task 结束后，都要清空所有的 micro task。

vue 在修改数据后，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新, 下面的代码可以验证这个论点（你可可以复制到你的编辑器里，直接运行从控制台看结果）：

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">{{message}}</div>
	</body>
</html>
<script type="text/javascript" src="https://cdn.jsdelivr.net/vue/2.1.3/vue.js"></script>
<script type="text/javascript">
	var vm = new Vue({
      el: '#app',
      data: {
        message : "hello" 
      }
  	});
	vm.message = "changed";
  	console.log("textContent",vm.$el.textContent); //没有改变，依然是'hello'
  	Vue.nextTick(function(){
	    console.log("nextTick" ,vm.$el.textContent) //可以得到'changed'
	})
</script>
```
上面这的代码具体执行的流程：<br>
1. vm.message = "changed" &nbsp;  --->  &nbsp; 观察到数据改变，vue 开启一个队列，在这个队列里面缓冲在同一个事件循环中发生的数据改变（如果同一个数据 watcher 发生多次改变，它只会被放到一个缓冲队列中，这样可以避免不必要的计算）。
2. 事件队列会不断地进行更新，vue 会使用 <strong>Promise</strong> 对异步队列进行操作，如果执行环境不支持，会使用 <strong>settimout()</strong> 做向下兼容。事件循环队列清空后，在下一个队列开启前，与完成队列相关的 DOM 将会更新。
3. 如果我们需要在 DOM 确定完成更新之后，进行一个操作，这个时候就需要用到 <strong>nextTick()</strong>

<hr>
<em>后记：网上后端文章，<strong>Vue.nextTick</strong> 和 <strong>this.$nextTick()</strong> 傻傻分不清楚，把当做两个方法，让人难以置信。<strong>Vue.nextTick</strong> 的调用方式是直接调用 Vue 对象的属性, <strong>this.$nextTick()</strong> 是在 vue 实例化对象内部，调用原型链上的属性，绑定到调用它定位实例上，并不是两个方法！！！</em>