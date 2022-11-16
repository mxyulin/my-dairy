# 项目研发笔记

## 项目环境配置

### 后端配置

- 本地 java 服务器
  - 下载[Java SE Development Kit 8u351(JDK8)](https://www.oracle.com/java/technologies/downloads/#java8)并安装作为后端服务器运行环境。
  - [配置环境变量](https://www.runoob.com/java/java-environment-setup.html)。
  - ~~通过命令`java -jar fcb-api.jar`运行后端提供的 java 应用`fcb-api.jar`得到本地的后端服务器支持。~~
  - 上面命令适用于`linux`，`windows`通过命令`java -Dfile.encoding=UTF-8 -jar fcb-api.jar`运行后端提供的 java 应用`fcb-api.jar`得到本地的后端服务器支持。

- 本地数据库
  - 下载轻量级服务器环境集成工具[phpstudy](https://www.xp.cn/)并安装。
    - 软件管理中额外下载工具`redis`, `redisClient`, `SQL_Front`
      - [redis](https://redis.io/)是一个非关系数据库(NoSQL)，优点：性能极高、多用途，可移植、强大的命令提示。
    - 数据库设置
      - 数据库必须使用`mySql8.x`，数据库基本配置：字符集`utf8mb4`, 引擎`innoDB`。
      - 设置 root 用户密码后，创建数据库`fcb_college`。账号、密码都是`fcb_college`。
      - 打开`SQL_Front`工具，新建登录信息。host: `localhost`，其他空字段写`fcb_college`。
      - 通过`phpstudy`面板导入三个`sql`文件（会合并到`fcb_college`数据库）。
  - 数据库浏览器（图形化界面工具），建议用`Navicat`（最稳定、专业、强大的收费数据库浏览器），使用`SQL_Front`可能会有打不开表报错的问题（`SQL_Front`只是`PHPstudy`的一个插件软件，新手用来练习还行）。

## 问题和解决

### 开发环境问题

- vscode使用`node`和`npm`命令报错:`node(npm) : 无法将“node(npm)”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径正确，然后再试一次`。
  - 很有可能是没有通过管理员权限启动 vscode ，只有管理员权限启动的 vscode 才能使用`node`和`npm`。
  - 实在不行就试试配置[全局环境变量](https://www.jianshu.com/p/773484370e46)。
- 运行项目报错：`node:events:491 throw er; // Unhandled 'error' event`.
  - 缺少环境变量，需要在环境变量Path中添加`C:\Windows\System32`。
- 运行项目发现`Network: unavailable`.
  - 缺少环境变量，需要在环境变量Path中添加`C:\windows\System32\Wbem2`。
  - [其他可能性](https://blog.csdn.net/m0_46426161/article/details/125527310)
- 配完环境变量再重启 vscode，再重新运行项目即可。
- `node`版本问题原因可能所在：`Node.js v17.x`关闭了 SSL 提供程序中的一个安全漏洞[stackoverflow回答](https://stackoverflow.com/questions/69692842/error-message-error0308010cdigital-envelope-routinesunsupported)
  - 解决办法：最稳妥有效的办法是回退安装[Node.js 16.x](https://nodejs.org/download/release/v16.18.0/)。
- 组件不渲染：是因为需要使用`<basic-container></basic-container>`作为每个组件的容器。
- 项目内暂时不支持子组件开发，原因未知。

### 样式问题

- `el-popover`组件宽度无效问题
  - 原因：`el-popover`与`app`组件平行，故不能写`scoped`限制样式的作用域。
  - 解决办法: 单独另写一个`<style lang=scss></style>`容器给`el-popover`组件写样式。切记不要加`scoped`。
- 利用`el-col`分栏布局让两个及以上`el-form-item`表单项占据一行，这两个组建的样式会冲突，下一行`el-form-item`会增大高度遮盖住`el-col`包裹的那些表单项。从而使得分栏布局的表单项**不能被点击**。
  - 原因：饿了么UI组件样式冲突。
  - 解决办法：换个思路使用`flex`做两个及以上表单项独占一行的布局。

### 控制台报错

- `JSON parse error: Cannot deserialize value of type 'java.lang.Integer' from`
  - 原因：前端组件传输的数据字段类型和后端实体类字段不一致。
  - 解决办法：用`navicat`查看数据库字段类型，把组件传入的参数类型写正确。比如后端需要`string`，你前端不能传个`number`。
- `npm(node) : 无法将“npm(node)”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径正确，然后再试一次。`
  - 原因：环境变量被修改或者删除了。
  - 解决办法：把`npm.cmd`, `node.exe`所在的路径添加到用户环境变量`Path`字段的数据里。还可以添加`node_global`和`node_cache`所在路径。

## 建议

- [Windows查看某个端口被谁占用](https://www.runoob.com/w3cnote/windows-finds-port-usage.html)
- 由于FCB后台的上线环境是 Linux , 并且 Linux 是对大小写敏感的系统，所以 Windows 开发的代码也最好一并小写。能不用驼峰命名法就不用。
- 回调里的`this`用`let that = this`进行暂存，目的是
- 注意项目接口返回的状态码字段是`result.data.code`而不是`result.code`。
- 配置阿里云图标，建议图标仓库的字体配置能选上的都选上，以免使用时乱码。
- 根据接口返回的`code`，提示用户数据上传/获取成功与否，当然是写在**响应拦截器**中的。这样就能配置一次全局使用。类似的还有**加载进度条**也能写到**请求和响应拦截器**中。
- 重写 Elment-UI 的表单控件宽度，一般情况下建议直接写行内样式，简单快速不用考虑选择器权重大小。但如果是有相同宽度的大量表单控件需求，还是建议写一个高权重样式或样式穿透覆盖 Elment-UI 的表单控件原宽度。
