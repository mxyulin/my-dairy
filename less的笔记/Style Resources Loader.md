# Style Resources Loader

使用该`webpack`扩展，可以让样式资源（包括变量、函数、混入）自动导入各个组件，`css`, `sass`, `scss`, `less`, `stylus`均可。

## 效果

- 所有组件共享这类样式样式资源（包括变量、函数、混入），无需手动`@import '样式文件'`导入。
- 可以覆盖重写其他UI库的样式，比如`element-ui`, `ant-design`等。
