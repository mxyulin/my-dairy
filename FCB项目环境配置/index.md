# 项目研发笔记

## 项目环境配置

### 后端配置

- 本地 java 服务器
  - 下载[Java SE Development Kit 8u351(JDK8)](https://www.oracle.com/java/technologies/downloads/#java8)并安装作为后端服务器运行环境。
  - [配置环境变量](https://www.runoob.com/java/java-environment-setup.html)。
  - 通过命令`java -jar fcb-api.jar`运行后端提供的 java 应用`fcb-api.jar`得到本地的后端服务器支持。

- 本地数据库
  - 下载轻量级服务器环境集成工具[phpstudy](https://www.xp.cn/)并安装。
    - 软件管理中额外下载工具`redis`, `redisClient`, `SQL_Front`
      - [redis](https://redis.io/)是一个非关系数据库(NoSQL)，优点：性能极高、多用途，可移植、强大的命令提示。
    - 数据库设置
      - 设置 root 用户密码后，创建数据库`fcb_college`。账号、密码都是`fcb_college`。
      - 打开`SQL_Front`工具，新建登录信息。host: `localhost`，其他空字段写`fcb_college`。
      - 通过`phpstudy`面板导入三个`sql`文件（会合并到`fcb_college`数据库）。

## 问题和解决

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

## 建议

- [Windows查看某个端口被谁占用](https://www.runoob.com/w3cnote/windows-finds-port-usage.html)
- 不要使用 node 升级工具`n`，`n`不支持
