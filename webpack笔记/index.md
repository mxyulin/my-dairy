# webpack笔记

webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。从 webpack(v4.0.0) 开始，可以不用引入一个配置文件。然而，webpack 仍然还是高度可配置的。

## 四个核心概念

四个核心概念分别是：入口(entry)、输出(output)、加载器(loader)、插件(plugins)

- **入口**(entry)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。实质上就是项目模块的入口文件，默认值为`./src/index.js`。
  - vue.config.js 中的配置项：`entry: './path/to/my/entry/file.js'`。
  - 可以指定一个入口起点或[多个入口起点的多页面应用](https://www.webpackjs.com/concepts/entry-points/)。
- **出口**(output)配置项告诉 webpack 在哪里输出它所创建的 bundles。实质上就是所有模块打包文件的配置项，默认输出到`./dist`中。
  - 通过配置项`output.path`和`output.filename`[配置模块打包文件输出的地方和打包文件的名字](https://www.webpackjs.com/concepts/#%E5%87%BA%E5%8F%A3-output-)
- **加载器**(loader)能够让 webpack 能够去处理那些非 JavaScript 文件，因为：**webpack 自身只能解析 JavaScript**。而额外的加载器扩展模块，能够让 webpack 具有更多的解析能力，把这些预编译语言代码（less, sass, typeScript...）解析为 ES2015, ES5, CSS3标准的代码。
  - loader 的配置是在声明**检测**（test）在某种文件类型时就**使用**（use）指定的 loader 去编译。

## webpack的生命周期

- [Loader深入解析](https://juejin.cn/post/6944349196539396133)