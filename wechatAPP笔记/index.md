# 微信小程序笔记

## 小程序开发指南

### 小程序和网页开发的区别

- 小程序开发和网页开发有很大的区别，主要在于应用程序运行的**平台不同**，前者是在微信中，后者是在浏览器中。
- 网页开发脚本线程和渲染渲染线程是**互斥的**，原因下面会说。网页开发者可以使用到各种浏览器暴露出来的 DOM API，进行 DOM 选中和操作。小程序的逻辑层和渲染层是分开的，逻辑层运行在 JSCore 中，并没有一个完整浏览器对象，因而缺少相关的DOM API和BOM API。
- 上述区别导致了前端开发非常熟悉的一些库，例如 jQuery、 Zepto 等，在小程序中是无法运行的。同时 JSCore 的环境同 NodeJS 环境也是不尽相同，所以一些 NPM 的包在小程序中也是无法运行的。
- 渲染线程和脚本线程的概念：
  - [渲染线程](https://fe.azhubaby.com/Browser/%E6%B8%B2%E6%9F%93%E8%BF%9B%E7%A8%8B%E4%B8%AD%E7%9A%84%E7%BA%BF%E7%A8%8B.html#gui-%E6%B8%B2%E6%9F%93%E7%BA%BF%E7%A8%8B)
    - 具体指的是 GUI 渲染线程，负责渲染页面(解析HTML, CSS, 构建 DOM 树和 RenderObject 树，布局和绘制等)
  - [脚本线程](https://fe.azhubaby.com/Browser/%E6%B8%B2%E6%9F%93%E8%BF%9B%E7%A8%8B%E4%B8%AD%E7%9A%84%E7%BA%BF%E7%A8%8B.html#js-%E5%BC%95%E6%93%8E%E7%BA%BF%E7%A8%8B)
    - 就是 JS 内核，负责解析和执行 JavaScript 脚本程序（例如 V8 引擎）
- 网页开发中渲染线程和脚本线程是互斥的，为什么互斥？
  - 是因为 JS 能够操作 DOM ，如果渲染线程和脚本线程同时执行，那么 DOM 的最终状态不可预测。从而使得渲染线程渲染前后获得的 DOM 不一样。

### 小程序脚手架文件介绍

- JSON 配置文件
  - app.json 是当前[小程序的全局配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html)，统一配置页面路径、界面表现、网络超时时间、底部 tab 等。
  - page.json 是[小程序某个页面页面的配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/page.html)，用于单独指定某一页面的样式效果等。
  - **注意**：JSON 文件中不能写注释，写了注释会报错。因为过多的注释，影响了文件本身的数据载体的目的。
- page.wxml 是微信小程序的标记语言文件，就是相当于 HTML 文件
  - WXML 提供了：数据绑定、列表渲染、条件渲染、template 标签模板等[WXML语法](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/)，还有很多成熟完善的[微信小程序组件](https://developers.weixin.qq.com/miniprogram/dev/component/)供开发者使用。
- page.wxss 是微信小程序样式表文件，具有大部分 CSS 特性，相当于 CSS 文件。
  - 相比 CSS 新增了**尺寸单位**`rpx`，用于兼容不同移动设备屏幕宽度和像素比。由于是浮点运算，可能结果会和预期有一点点的偏差。
  - app.wxss 文件作为全局样式应用，局部页面样式 page.wxss 仅对当前页面生效。
  - **注意**： WXSS 仅支持部分 CSS 选择器，请参考[WXSS语法](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html)。
- page.js 是小程序的逻辑交互文件，相对于PC网页开发不同之处在上文《小程序和网页开发的区别》已说明。
  - 微信小程序也提供了一套自己的 [WeiXinScript](https://developers.weixin.qq.com/miniprogram/dev/reference/wxs/) 语法，和原生 JavaScript 有区别。
  - 当然最重要的是微信小程序提供的各种[API](https://developers.weixin.qq.com/miniprogram/dev/api/)。

### 小程序源代码允许上传的文件类型

- 微信小程序`.js`, `app.json`, `.wxml`, `.wxss`文件会经过编译后再上传，上传后且无法再访问到。
- `.wxml`, `.wxss`文件必须是在`app.json`的`pages`选项配置了的才会被编译上传。
- 除了上面的文件类型外，还有 18 种文件可被编译上传，组成小程序的一部分: `.wxs`, `.png`, `.jpg`, `.jpeg`, `.gif`, `.svg`, `.json`, `.cer`, `.mp3`, `.aac`, `.m4a`, `.mp4`, `.wav`, `.ogg`, `.silk`, `.wasm`, `.br`, `.cert`.

### 小程序 sitemap 配置

- `sitemap.json`用来配置小程序及其页面是否允许被微信索引，简而言之就是设置哪个页面会或者不会被其他人搜到。完整配置参考[小程序 sitemap 配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/sitemap.html)

## 小程序框架

### 场景值

- 小程序场景值：场景值用来描述用户进入小程序的路径。完整场景值的含义请查看[场景值列表](https://developers.weixin.qq.com/miniprogram/dev/reference/scene-list.html)。
- **注意**：由于 Android 系统限制，目前还无法获取到按 Home 键退出到桌面，然后从桌面再次进小程序的场景值。对于这种情况，会保留上一次的场景值。
- 对于小程序，可以在 App 的`onLaunch`和`onShow`，或[wx.getLaunchOptionsSync](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/life-cycle/wx.getLaunchOptionsSync.html)中获取上述场景值。

### 逻辑层的构成

- [小程序的注册](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html)：在`app.js`中注册小程序实例，然后绑定各种生命钩子，事件监听，提供全局数据等。
- [页面的注册](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html)：在`page.js`使用`Page`构造器注册页面。类似于小程序实例注册，每个页面注册也得提供单独的数据，生命钩子，事件监听，事件回调等。
  - 页面可以引用[behaviors模块](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/behaviors.html)。`behaviors`模块可以用来让多个页面有相同的数据字段和方法。
  - 简单页面适合`page`构造器，复杂页面适合[component构造器](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html)。两者主要区别在于后者的方法需要写在`methods: {...}`里面。
- 页面生命周期钩子
- 页面的路由
  - 和`vue`一样，小程序路由也是框架的[路由器对象](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Router.html)进行管理。
  - `this.pageRouter`获得的路由器对象具有更好的基路径稳定，使用`this.pageRouter.navigateTo`代替`wx.navigateTo`是更优的。
  - 使用`this.pageRouter`和`this.router`在自定义组件与官方组件中进行路由跳转是不同的。前者相对于**自定义组件所在的页面**来进行路由跳转，而后者相对于自**定义组件自身的路径**。
  - [页面栈](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html)
  - [路由触发和生命钩子](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html)

### 视图层的构成

- [WXML](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/)
  - 数据绑定、列表渲染、条件渲染、模板、引用
  - [官方基础组件](https://developers.weixin.qq.com/miniprogram/dev/component/)
  - [官方事件系统](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)
- WXSS
  - 相比 CSS 扩充了尺寸单位`rpx`，`1rpx = 0.5px = 1物理像素`。
  - 相比 CSS 扩充了样式导入（模仿的less, sass的`@import "...";`）。
  - 动态内联样式`<view style="color:{{color}};" />`。
  - 仅支持`class`选择器, `id`选择器, 元素选择器, 分组选择器, 伪元素选择器`::after`和`::before`。
- [WXS](https://developers.weixin.qq.com/miniprogram/dev/reference/wxs/)
  - WXS 的运行环境和其他 JavaScript 代码是隔离的，WXS 中不能调用其他 JavaScript 文件中定义的函数，也不能调用小程序提供的API。
  - WXS 函数不能作为组件的事件回调。
  - WXS 的主要作用是处理页面组件的数据（好比`vue`里的`data`, `computed`属性）。
- [数据的双向绑定](https://developers.weixin.qq.com/miniprogram/dev/framework/view/two-way-bindings.html)和`vue`几乎一致，表单控件仅支持绑定一个数据字段。二者在写法上有所区别，微信小程序是通过插值语法和`model:value`进行数据的双向绑定。
  - 微信小程序数据双向绑定的写法限制，其实可以那样写，但是官方不建议属于非法写法。
- [获取节点信息](https://developers.weixin.qq.com/miniprogram/dev/framework/view/selector.html)
- [响应屏幕旋转](https://developers.weixin.qq.com/miniprogram/dev/framework/view/resizable.html)
- 分栏模式（了解）
- [CSS过渡和动画](https://developers.weixin.qq.com/miniprogram/dev/framework/view/animation.html)（进阶）
- [WX简易动画](https://developers.weixin.qq.com/miniprogram/dev/api/ui/animation/wx.createAnimation.html)（进阶）
- [用户体验优化之初始渲染缓存](https://developers.weixin.qq.com/miniprogram/dev/framework/view/initial-rendering-cache.html)（进阶）
  - APP 冷启动是指，后台没有 APP 的进程，完全重新开始的启动。
  - APP 热启动是指，后台有 APP 的进程，利用已有的 APP 进程启动，是不完全开始的启动。

### 不同系统的运行时（运行环境）

- IOS 系统：逻辑层 => JS Core，视图层 => WKWebView。
- 安卓系统：逻辑层 => 谷歌V8引擎，视图层 => 基于 Mobile Chromium 内核的微信自研 XWeb 引擎。
- Windows系统（微信PC版）：逻辑层和视图层 => Chromium内核。
- 开发工具中：逻辑层 => NW.js，视图层 => Chromium Webview。

### 自定义组件

- [组件模板和样](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/wxml-wxss.html)
- [组件构造器Component](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html)
- [组件间通信与事件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/events.html)
- [组件生命周期](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/lifetimes.html)
- [公共代码混入behaviors](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/behaviors.html)
- [组件间关系](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/relations.html)
- [数据监听器](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/observer.html)
- [性能优化纯数据字段](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/pure-data.html)（进阶）
- [抽象节点](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/generics.html)（进阶）
- [自定义组件扩展](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/extend.html)（进阶）
- 更多：开发第三方自定义组、单元测试、获取更新性能统计信息、占位组，请阅读[自定义组件介绍](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/)。

## 小程序配置项

```json
{
  "pages": [
    "pages/index/index",
    "pages/logs/logs"
  ],
  "subPackages": [],
  "window": {
    // 导航栏样式
    "navigationBarTextStyle": "black",
    "navigationBarTitleText": "我的应用",
    "navigationBarBackgroundColor": "#07C160",
    "backgroundColor": "#F8F8F8",
    // ios 导航栏样式
    "backgroundColorTop": "#F8F8F8",
    "backgroundColorBottom": "#F8F8F8",
    // 下拉样式
    "backgroundTextStyle": "dark",
    "enablePullDownRefresh": true,
    // home 按钮（左上）
    "homeButton": true,
    "onReachBottomDistance": 50,
    "pageOrientation": "auto",
    "restartStrategy": "homePageAndLatestPage"
  },
  "usingComponents": {},
  "tab": {
    "list": [
    ]
  }
}
```

## 一些需要注意的点

- appid 在小程序后台👉设置👉基本设置👉下拉到底部的账号信息。
