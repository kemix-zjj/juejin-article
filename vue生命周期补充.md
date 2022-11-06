解答

- Vue的生命周期到底是什么
- Vue生命周期的执行顺序
- 生命周期的每个阶段适合做什么
- 我们的请求放在哪个生命周期会更合适

## 何为 Vue 的生命周期？

- 简单来说，即描述一个组件从引入到退出的全过程。
- 复杂来说，即一个组件从【创建】开始经历了【数据初始化】，【挂载】，【更新】等步骤后，最后被【销毁】。

## Vue 生命周期的执行顺序？

三大阶段，八小阶段。

- 挂载阶段
  - beforeCreate
  - created
  - beforeMounted
  - mounted
- 更新阶段
  - beforeUpdate
  - updated
- 销毁阶段
  - beforeDestroy
  - destroyed

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6a2e1b67bb2b40f0a7f944bbfedaa7ad~tplv-k3u1fbpfcp-watermark.image?)

执行顺序如下：

[官方文档介绍](https://cn.vuejs.org/guide/essentials/lifecycle.html)

生命周期执行过程翻译如下：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b5362337d3b3488d9fc1c01c7646b4f6~tplv-k3u1fbpfcp-watermark.image?)

### 挂载阶段

- beforeCreate：创建前的状态，什么信息都木有
- created：创建完成，可以拿到数据 data，但 DOM 元素获取不到。【created 已经将数据加载进来了，已经为 Vue 实例开辟了内存空间】
- beforeMount：挂载前的状态，将虚拟 DOM 转化成真实 DOM 的过程
- mounted：挂载结束，意味着虚拟 DOM 已经挂载在真实的元素上，可以拿到 DOM 元素啦。同时可以利用 `console.dir` 去打印一些需要的元素属性。

### 更新阶段

- beforeUpdate：页面元素更新前的状态
- updated：页面元素更新完成的状态

### 销毁阶段

- beforeDestory：销毁前的状态，在销毁前，我们的元素、data都是如同挂载之后的阶段一样，都是可以打印出来的。
- destoryed：销毁完成的状态，元素还是可以打印出来的。
- beforeDestroy 和 destroyed，都是我们离开这个组件才会被调用的生命周期。 

## 生命周期每个阶段需要干啥？

### created

 在Vue实例创建完毕状态，我们可以去访问data、computed、watch、methods上的方法和数据，但现在还没有将虚拟Dom挂载到真实Dom上，所以我们在此时访问不到我们的Dom元素（el属性，ref属性此时都为空） 

**【我们在此时可以进行一些简单的Ajax，并可以对页面进行初始化之类的操作 】**

### beforeMounted

它是在挂载之前被调用的，会在此时去找到虚拟Dom，并将其编译成Render 

### mounted

虚拟Dom已经被挂载到真实Dom上，此时我们可以获取Dom节点，`$ref`在此时也是可以访问的

**【我们在此时可以去获取节点信息，做Ajax请求，对节点做一些操作 】**

### beforeUpdate

响应式数据更新的时候会被调用，`beforeupdate` 的阶段虚拟Dom还没更新，所以在此时依旧可以访问现有的Dom

**【我们可以在此时访问现有的Dom，手动移除一些添加的监听事件 】**

### updated

此时补丁已经打完了，Dom已经更新完毕，可以执行一些依赖新Dom的操作 

**【但还是不建议在此时进行数据操作，避免进入死循环 】**

### beforeDestroy

在Vue实例销毁之前被调用，在此时我们的实例还未被销毁

**【在此时可以做一些操作，比如销毁定时器，解绑全局事件，销毁插件对象等 】**

## 父子组件的生命周期

- 挂载阶段：

  父组件 beforeMount -> 子组件 created -> 子组件 mounted -> 父组件 mounted

- 更新阶段：

  父组件 beforeUpdate -> 子组件 beforeUpdate -> 子组件 updated -> 父组件 updated

- 销毁阶段：

  父组件 beforeDestroy -> 子组件 beforeDestroy -> 子组件 destroyed -> 父组件 destroyed

##  补充：请求最好放在哪？

一般来说，会有两种回答：`created ` 和 `mounted`。 上文已经讲了，这两个回答，前者是数据已经准备好了，后者是连dom也已经加载完成了，那么到底哪个才是正确答案呢？

其实，两个都是可以的，但是`mounted`会更好。

原因一：因为在 created 阶段中还没有 DOM 元素，接下来要执行 DOM 渲染。若此时进行 Ajax 操作，在 Ajax 结束后会返回数据，我们会将其插入到主线程中去运行，去处理数据。但要知道【在浏览器机制中，渲染线程跟 js 线程是互斥的】，所以【有可能】我们做渲染的同时。另外一边可能要处理 Ajax 返回的数据。此时的渲染【有可能被打断】，在处理完数组后，去进行重新渲染。如果遇到多个 Ajax 请求的话，此时就很糟糕了。

原因二：同时有时候发送请求返回的数据，是需要在回调函数中去执行一些 DOM 操作。但此时的 created 阶段的真实 DOM 还未加载出来。

 

[参考文章](https://juejin.cn/post/7032881219524100132#heading-0)

 

 

 

 

