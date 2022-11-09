# Git 使用助手

## Git核心理念

- 整个Git逻辑空间分为：
  - 工作区---也就是我们编写代码的区域，将自己负责的模块进行**追踪**(track)，被追踪的文件将自动**添加**到暂存区。
  - 暂存区---提交前暂时存放代码的区域，只有被追踪的文件才能手动**提交**到本地仓库。
  - 本地仓库---顾名思义，是存储在本地的代码仓库，本地仓库其实就是**本地版本区**。
  - 远程仓库---就是码云、github这类**代码托管平台**，一个项目有多少开发者理论就有多少分支，每个开发人员负责自己的那一部分模块，开发完毕即可**推送**至远程仓库，这就算是一次版本更新。
- 核心命令
  - `clone`将整个项目克隆到工作区，这对中途加入的开发人员很有效。
  - `add`添加（追踪）工作区文件至暂存区。
  - `commit`提交暂存区文件至本地仓库。
  - `push`推送本地仓库文件至远程仓库。
  - 重点命令：
    - `pull`拖拽这是一个**不能随便**用的命令，它会将你工作区的代码完全更新为远程仓库版本。
    - `fetch`提取和`diff`对比这俩命令就是真正安全的命令，而不是`pull`那样粗暴。先将远程仓库的版本提取到本地，然后与工作区的代码进行`diff`对比区别，**确认无误**后再`git pull`将远程库合并到工作区（这将替换所有原有的文件）。

## 使用流程

- [config]:配置
  - 先切到工作区，然后`git config --global user.name "用户名"`，用户名有空格需要加上引号。
  - 如果需要邮箱的话`git config --global user.email "邮箱名"`。
- [init]:初始化
  - 初始化：`git init`，得到[.git](隐藏文件夹，一半不需要写入什么)
  - 快捷创建文件：`echo "复习git1" > yu1.md`，双引号内是文件内容。
- [status]:状态/下一步
  - 查看当前文件状态`git status`，可以看到当前所处分支、未追踪(untracked)文件有哪些。
- [add]:添加/追踪
  - 添加文件到缓存区，使用`git add 文件名`进行追踪。
- [commit]:提交
  - 将缓存区文件（已追踪文件）提交到本地仓库`git commit -m '说明信息'`。`-m`修饰符表示需要追加说明信息。
  - `git commit -a -m '说明信息'`-a`修饰符表示`add`追踪未追踪的文件（不包括新建的未追踪文件）。
  - 追踪并提交简写：`git commit -am '说明信息'`
  - **状态并存**：未添加前修改文件后使用`git add`，添加前的内容会被追踪。添加后修改文件如果没有`git add`，那此时的`status`是未追踪和追踪两种状态，分别对应修改后和修改前的内容。
- [log]:日志
  - 查看提交日志：`git log`
  - `git reflog`查看参考日志，`reflog`即`Reference logs`，记录分支提示和其他引用在本地存储库中更新的时间。
- [reset]:版本回退
  - 先`git reflog`查看需要回退的版本。
  - 再`git reset --hard 版本号`回退到该版本代码状态。
- **忽略文件名单**：输入命令`touch .gitignore`可以生成忽略名单文件，鼠标右键新建也行。
- [branch]:分支
  - 创建分支`git branch 分支名`，可用破折号连接单词。
  - 进入分支`git checkout 分支名`。
    - 切换本地分支`git checkout branchName`
    - 如果切换不成功，说明本地没有这个分支，那就需要`git pull origin branchName`先把该分支拉到本地库。
  - 删除分支(需要退出要删除的分支)
    - 删除已合并分支`git branch -d 分支名`。
    - 强制删除分支（不论是否合并）`git branch -D 分支名`（**工作中别用！**）。
    - 创建新分支并立刻切换到新分支`git checkout -b 分支名`。
  - 合并分支（把别的分支合并到当前所处的分支上）
    - `git merge 分支名`把别的分支合并到当前所处的分支上。
    - 分支修改后的内容与主分支不一样是正常情况，需要自己对修改部分做取舍。或者使用`merge`工具来操作。
- [远程仓库]:github新建远程仓库
  - 登录github，点击右上角的`+`号，新建仓库。
  - 创建文件并设置名字、主题、描述。
  - 克隆到本地：
    - 直接下载仓库的压缩文件是没有版本历史记录和[.git](文件夹)的。
    - 使用命令行克隆：复制`https`的仓库地址。使用`git clone 仓库地址`把远程仓库下载到本地（粘贴快捷键`shift + ins`）。
  - 细节问题：github仓库主支名是`main`，而本地仓库默认为`master`。
  - 本地仓库推送到远程仓库：
    - 首先查看和哪些远程仓库有连接`git remote -v`（从远程克隆到本地的仓库默认自带远程仓库的地址）。
    - **重点**：拿到gihub`token`访问远程仓库。然后`git push`
    - 推送代码时提示`git push --set-upstream origin your-branch`是因为当前分支没有与远程分支关联，所以 git 让你去关联分支。
      - 可以`git push origin your-branch`强制推送到指定分支。
      - 或者`git push --set-upstream origin your-branch`先关联，这样以后推送就不会再让你关联分支了。
  - [fetch和diff]:从远程库拉取分支到本地库
    - 直接`git fetch`即可，工作区不反映变化，因为在本地仓库里。
    - 查看本地仓库和远程仓库区别`git diff origin/远程分支名`。
  - `git restore`

## 常见报错

- `OpenSSL SSL_read: Connection was reset, errno 10054`
  - 首先，造成这个错误很有可能是网络不稳定，连接超时导致的，如果再次尝试后依然报错，可以执行`git config --global http.sslVerify "false"`，作用是解除SSL认证。
  - CMD窗口输入`ipconfig /flushdns`清除DNS缓存。
  - 有可能代理节点废了，建议用国内的网直连，应该都行。
  - [解决办法---CSDN](https://blog.csdn.net/m0_51269961/article/details/123709195)
- `! [rejected] ... error: failed to push some refs to`
  - 使用`git pull --rebase origin master`，再推送代码。
  - [解决办法--CSDN](https://blog.csdn.net/qq_45893999/article/details/106273214)
