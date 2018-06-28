###### react 生态圈目前非常繁荣，Facebook 目前专注于 react 功能的开发，把与之相配套的中间件及工具的开发交给了社区， 全世界的 react 开发者们做出了非常大的贡献。围绕着 react 国内多家大厂推出了自己的类 react 框架，会 react 的童鞋对那些框架的上手会非常快，这里重点介绍一下：

这几天遇到一个奇怪的 bug，就是点击切换路由组件的的时候，遇到 react 红色的警告：
<p><code style='color:red;'>Warning: setState(...): Can only update a mounted or mounting component. This usually means you called setState() on an unmounted component. This is a no-op. Please check the code for the undefined component."</code></P>
<p>我使用的使用的是 <strong>react v16.2.0</strong> | <strong>react-hot-loader v4.3.3</strong> | <strong>react-dom v16.2.0</strong> | <strong>react-router v4.2.0</strong></p>
<p>通过网上搜索发现，这个问题出现的主要原因为： react 组件已经从 DOM 中移除，但是 由于 setState 是异步的，当你切换组件的时候，可能在组件已经被移除了，但是 setState 操作可能在组件卸载后仍在执行。</p>
<p>原因虽然找到，但是我查了一下自己代码，将出现问题组件中的所有 setState 操作注释之后，依然发现这种问题。因为点击切换组件，用到了react 的路由，所以到这一步，我把问题锁定在路由的问题上，在我将路由跳转的代码注释之后，警告信息消失了。原来是路由的问题，react router 4.0 开始采用单代码仓库模型架构，将整个路由的功能，拆分成若干独立的组件:</p>
<li><code>react-router</code>&nbsp; React Router 核心</li>
<li><code>react-router-dom</code>&nbsp; 用于 DOM 绑定的 React Router</li>
<li><code>react-router-native</code>&nbsp;  用于 React Native 的 React Router</li>
<li><code>react-router-redux</code>&nbsp; React Router 和 Redux 的集成</li>
<li><code>react-router-config</code>&nbsp; 静态路由配置的小助手</li>
<br>
<p>定位到这个问题之后，我一直怀疑是用的 router 组件版本不对，在反复切换了多个 router 版本之后，这个问题依然没有得到解决，这个时候我有了一个思路：既然警告提示出现的原因在理论上我们已经知道，是与异步 setState 有关系，那么接下来就应该从跟路由相关的 setState 入手。为了实现热更新，且不丢失 state 和 redux 的状态，我们使用了 <strong>react-hot-loader</strong> ，并且为了实现组件的按需加载，我们通过 <strong>Bundle</strong> 结合 webpack 插件 <strong>bundle-loader</strong> 实现了组件的按需加载，在 <strong>Bundle</strong> 文件中，我们有 setState 操作，组件的按需加载和热更新之间是关联起来的，为什么不从他们两个入手呢？说干就干，我们检查了一遍 <strong>Bundle</strong> 文件，没有发现代码问题，接下来我去 <a href='https://github.com/'>GitHub</a> 上面查了一下 <strong>react-hot-loader</strong> 的版本情况，发现自己用的是 <strong>v4.0.0</strong> 的版本，而它最新的版本已经更新到 <strong>v4.3.3</strong>，所以我用 <strong>npm install react-hot-loader@latest -D</strong> 将它更新到了最新版本，启动程序，报错没有了！</P>

###### 后记：从这次遇到的问题中，我深刻地明白了 react 生态圈目前的一个状况，开发方案非常多，但是这也造成了某种层度上的混乱，如果你使用了社区里的一些开发方案，往往会遇到 react 之外的问题，而且这些可能并不是你的问题。面对这种情况，我们需要的回到 react 的核心价值上来，只有基础打牢了，才能遇到 bug，有能力去应付它!