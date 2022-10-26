# 使用less遇到的问题

- 在浏览器中使用 less.js 需要满足以下几点：
  1. 引入 less.js 之前需要先引入 xxx.less ，因为 less.js 执行时会去找已有的 xxx.less 文件。
  2. 需要特别注意的的是：浏览器端 less.js 使用的是`ajax`来拉取 xxx.less 文件，所以直接以本地浏览器（file://）打开的 html 文件是拉取不到 xxx.less 的，所以需要用**Live Server**等轻量级服务器插件进行处理。
  3. `@import "./less/variable.less;"`引入 less 文件请注意用`;`结束。
- 理解 less 语法规则：
  - mixin 混入是一种方法（可以看作一个动词），用于把一组样式规则应用在另一个选择器中。
    - 混入带`()`意味着不会被编译，从而不会出现在最终编译后的 CSS 文件中。
    - 混带`()`意味会被编译，会出现在最终编译后的 CSS 文件中。
  - less 的伪类`:extend()`用于当前选择器下合并另一个选择器，这样就能使用另一个选择器提供的样式了。[less的extend语法](https://lesscss.org/features/#extend-feature)
    - 由于`:extend()`不支持加了`()`的选择器，所以被混入的选择器应该不加`()`。
  - less 的函数还不少，建议需要的时候再学习使用[less函数](https://less.bootcss.com/#%E5%87%BD%E6%95%B0functions)
- less 的配置是很绕的:
  1. 首先需要掌握[加载器的配置-loaderConfiguration](https://www.webpackjs.com/concepts/loaders/#%E9%85%8D%E7%BD%AE-configuration-)
  2. 然后是掌握[模块规则数组-UseEntries](https://www.webpackjs.com/configuration/module/#module-rules)
  3. 再是掌握`UseEntry`的核心-加载器配置项[UseEntry.options](https://webpack.docschina.org/loaders/less-loader/#options)
  4. 最后最核心的是 Less 的可选项[Less命令行可选参数](https://less.bootcss.com/usage/#command-line-usage)，需要注意的是**需要将破折号（dash-case）转换为驼峰值（camelCase）后传递它们**。
- 代码示例：

  ```js
    // less 配置
  module: {
    rules: [
      {
        // 打包之前检测到 less 文件，就应用 use 配置项
        test: /\.less$/,
        // 应用less 文件的配置，通过 less-loader 把 less 转换成普通 CSS
        use: ["vue-style-loader", "css-loader", {
          loader: "less-loader",// 为 less文件 指定 loader
          // 一个 UseEntry.options 对象
          options: {
            // *Less 的可选项，最核心的配置项
            lessOptions: {
              // 所有 CSS 静态资源的根路径
              rootpath: '/src/assets'
            },
            // 生成 source map
            sourceMap: true,
            // 此项可能会使得 aliases 和以 ~ 开头的 @import 规则失效
            //// webpackImporter: false,
          }
        }],
      },
    ],
  },
  ```

- [less即学即用---掘金高赞文章](https://juejin.cn/post/6844903688444739592)
