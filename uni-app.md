## 环境搭建

## 页面外观配置

## 数据绑定

## uni-app 的生命周期

### 应用生命周期

uni-app 支持如下应用生命周期函数：

- onLaunch：当uni-app 初始化完成时触发（全局只触发一次）
- onShow：当 uni-app 启动，或从后台进入前台显示
- onHide：当 uni-app 从前台进入后台
- onError：当 uni-app 报错时触发
- onUniNViewMessage：对 nvue 页面发送的数据进行监听，可参考 nvue 向 vue 通讯
- onUnhandledRejection：对未处理的 Promise 拒绝事件监听函数（2.8.1+）
	 onPageNotFound	：页面不存在监听函数
- onThemeChange：监听系统主题变化

```js
 // 只能在App.vue里监听应用的生命周期
    export default {
        onLaunch: function() {
            console.log('App Launch')
        },
        onShow: function() {
            console.log('App Show')
        },
        onHide: function() {
            console.log('App Hide')
        }
    }
```

### 页面生命周期

uni-app 支持如下页面生命周期函数：

- onLoad：监听页面加载，其参数为上个页面传递的数据，参数类型为Object（用于页面传参）
	 onShow：监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面		
	 onReady：监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发		
	 onHide：监听页面隐藏		
	 onUnload：监听页面卸载		
- onResize：监听窗口尺寸变化
	 onPullDownRefresh：监听用户下拉动作，一般用于下拉刷新	
- onReachBottom：页面滚动到底部的事件（不是scroll-view滚到底），常用于下拉下一页数据。
- onTabItemTap：点击 tab 时触发，参数为Object
- onShareAppMessage：用户点击右上角分享
	 onPageScroll	：监听页面滚动，参数为Object 
- onNavigationBarButtonTap：监听原生标题栏按钮点击事件，参数为Object
- onBackPress：监听页面返回，返回 event = {from:backbutton、 navigateBack} ，backbutton 表示来源是左上角返回按钮或 android 返回键；navigateBack表示来源是 uni.navigateBack ；详细说明及使用：onBackPress 详解。支付宝小程序只有真机能触发，只能监听非navigateBack引起的返回，不可阻止默认行为。
- onNavigationBarSearchInputChanged：监听原生标题栏搜索输入框输入内容变化事件
- onNavigationBarSearchInputConfirmed：监听原生标题栏搜索输入框搜索事件，用户点击软键盘上的“搜索”按钮时触发。
- onNavigationBarSearchInputClicked：监听原生标题栏搜索输入框点击事件
- onShareTimeline：监听用户点击右上角转发到朋友圈
- onAddToFavorites：监听用户点击右上角收藏

### 组件生命周期

uni-app 组件支持的生命周期，与vue标准组件的生命周期相同。这里没有页面级的onLoad等生命周期：

- beforeCreate：在实例初始化之后被调用。		
	 created：在实例创建完成后被立即调用。		
	 beforeMount：在挂载开始之前被调用。		
	 mounted：挂载到实例上去之后调用。 注意：此处并不能确定子组件被全部挂载，如果需要子组件完全挂载之后在执行操作可以使用$nextTickVue官方文档		
- beforeUpdate：数据更新时调用，发生在虚拟 DOM 打补丁之前。
	 updated：由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。	仅H5平台支持	
	 beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。		
	 destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。		

## uni-app 中样式学习

## 在 uni-app 中使用字体图标和开启 scss

## 条件注释跨端兼容

## uni 中的事件

## 导航跳转

## 组件创建和通讯，及组件的生命周期

## uni-app 中使用 uni-ui 库
