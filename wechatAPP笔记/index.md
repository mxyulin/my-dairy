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

## 小程序框架

微信小程序框架是不同于 vue 之类的 mvvm 框架的，它只有**逻辑层**和**视图层**。

- appid 在小程序后台👉设置👉基本设置👉下拉到底部的账号信息

### 小程序配置

- APP 冷启动是指，后台没有 APP 的进程，完全重新开始的启动。
- APP 热启动是指，后台有 APP 的进程，利用已有的 APP 进程启动，是不完全开始的启动。
- window配置项

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
