# uni-app笔记

## 基于vscode开发环境的搭建

[环境的搭建参考文章---掘金](https://juejin.cn/post/7090532271257714695#heading-14)

### 环境搭建

- uni-app 中 Vue2.x 版的组件库和插件也比较多，稳定、问题少，可以先参考下官方文档：[通过vue-cli命令行](https://link.juejin.cn/?target=https%3A%2F%2Funiapp.dcloud.net.cn%2Fquickstart-cli.html%23install-vue-cli)
- 安装脚手架`npm i -g @vue/cli@4`
- 初始化项目`vue create -p dcloudio/uni-preset-vue project-name`
- 选择默认模板即可（其他模板视情况而定）

### 语法提示插件(暂不安装)

### 内联颜色修饰器(暂不设置)

### 增强pages.json和manifest.json开发体验

- pages.json 和 manifest.json 报红问题，是因为 json 中是不能写注释的。
  - 解决办法：打开 setting.json 添加以下配置：
  
    ```json
    "files.associations": {
    "pages.json": "jsonc",// 解决pages.json注释报红
    "mainfest.json": "jsonc",// 解决mainfest.json注释报红
    },
    ```

  - **注意**：千万不要把所有 json 文件都关联到 jsonc 中。json 就是 json，jsonc 就是 jsonc，这两个是不一样的。在 package.json 写注释 VSCode 是不报错了，但编译的时候还是会报错的，因为 package.json 就是不能写注释的。

### 路径提示

- 安装插件 Path Intellisense （很早之前已安装）
- 若使用此插件提示路径，建议关闭 vscode 自带路径提示。打开 setting.josn 关闭以下配置：
  
  ```json
  "typescript.suggest.paths": false,
  "javascript.suggest.paths": false
  ````

- 配置项目根路径以及`/src`路径别名：

  ```json
  <!-- compilerOptions 添加以下设置 -->
    "compilerOptions": {
    // *编译器开始工作的基本路径
    "baseUrl": "./",
    // *配置 src/ 的别名
    "paths": {
      "@/*": ["src/*"]
    }
  },
  ```

### 一键创建页面、组件、分包(暂不安装)

### 组件语法提示

- 如\<view>、\<button>等 uni-app 原生组件语法提示。
- 安装插件`npm i @dcloudio/uni-helper-json`即可。
- 如果是使用的 vue3.x，可以使用`uni-app-types`这个包，因为`@dcloudio/uni-helper-json`不支持 vue3.x。
  - 然后在 tsconfig.jsonorjsconfig.json 配置 compilerOptions.types 和 vueCompilerOptions，确保 include 包含了对应的 vue 文件。
- uni.scss 变量提示，安装依赖`npm i sass sass-loader@10 -D`。
  - 安装 SCSS IntelliSense 插件，就可以提示你项目中 scss 文件中定义的变量了。

### 运行、发布项目

- [运行、发布uniapp](https://zh.uniapp.dcloud.io/quickstart-cli.html#%E8%BF%90%E8%A1%8C%E3%80%81%E5%8F%91%E5%B8%83uni-app)
  - 如果嫌 npm 命令过长，还可以使用 VSCode 的一键运行 npm 脚本，[以微信小程序为例](20221028141305.gif)。
- vscode 开发小程序，需要手动导入项目。导入一次即可，往后打开微信小程序开发工具就能直接预览。
  - 导入前需要在 manifest.json 配置微信小程序 appid ，不然微信开发者工具会报错。

### 使用 vue3 创建工程(暂不需要)

### vscode 使用 DCloud 插件市场的插件

VSCode 不能像 Hbuilder X 一样一键导入插件，一般用 cli 创建的项目要使用插件，一般有两种方式，第一种是支持 npm 安装的，那就用 npm 最好，如 uViewUI，另一种不支持 npm 安装的，那就下载对应的 zip 压缩包，放到项目中，这种一般会有两个版本，我们选择非 uni_modules 版本，如 uCharts。
