## **1.- Linux 命令**

- cd 文件夹名称			进入文件夹
- ls					查看当前文件夹的文件和文件夹
- pwd					显示当前路径地址
- mkdir 文件夹名称		创建文件夹
- touch 文件名称		创建文本文件
- echo 内容 > 文件名称	将内容添加到指定文件中 会进行覆盖
- echo 内容 >> 文件名称	将内容追加到指定文件中
- cat 文件				查看文件内容

## **2.- Git 介绍**

### **2.1- 下载安装**

官网： [Git (git-scm.com)](https://git-scm.com/)

Mac系统安装教程： [Git - Downloading Package (git-scm.com)](https://git-scm.com/download/mac)

Windows 系统中安装完成之后，在开始菜单的git目录中，会有三个工具：

Git Bash 为 linux / unix 风格终端

Git CMD 为 window 风格终端

Git GUI 为图形化工具

建议使用 Git Bash 终端。

### **2.2- 初始化配置**

- 安装完成git，第一次使用之前，需要配置一下全局用户名和邮箱地址：

- - 每台电脑只需要初始化一次 除非远程仓库地址修改，否则不需要修改

```js
$ git config --global user.name "John Doe"        // 用户名中间没有空格则不需要引号包裹
$ git config --global user.email johndoe@example.com
```

- 用户名是随意的，如果需要连接远程仓库，则邮箱地址必须与远程仓库的授权邮箱一致。（如果使用github，邮箱通常为github的注册邮箱，如果使用 gitee ，则需要在个人资料中查看邮箱地址）
- 常用辅助命令：

```js
# 查看配置信息
$ git config --list
# 查看指定命令的完整帮助信息
# git help 命令名称, 如
git help config
# 查看指定命令的简短帮助信息
# git 命令名称 -h, 如
git config -h
```

### **2.3- git 文件类型**

git 将文件分为三种类型：

- 第一类： 没有被git管理的文件

 	这类文件，一旦删除就再也无法恢复

- 第二类： 暂存文件

 	这类文件是纳入git暂存区的文件，可以通过 git 进行添加到仓库或撤销git管理操作

- 第三类： 纳入版本库的文件

 	同一个文件可以被存储为多个版本，可以通过git命令在各版本之间随意切换。

### **2.4- git 仓库**

- 要使用git，需要先创建git仓库，可以是一个通过初始化命令创建的全新仓库，也可以是从远程服务器中克隆的已有仓库。

- - 每个项目创建的时候都要初始化

- `git init： 初始化一个git仓库`

该命令用于创建一个新的git仓库，如果在一个空文件夹中执行该命令，将创建一个空仓库。

- **git常用命令**：

- - \1. `git add` ：提交到暂存区

  - - 提交全部文件 **git add .**

 	将工作区中未被管理的文件添加到暂存区

- - \2. `git commit -m` “说明” : 将暂存区中的文件提交到 git 仓库

  - - 每一次修改文件内容 都需要重新提交 >> 暂存区 >> 提交库

注意：说明一定要有意义

每一次 commit 提交，都会产生一个新的提交记录，一次提交记录，可以看作一个新的文件版本。

此时，一旦修改了文件，要通过git status查看一下被修改的是哪个文件

- - `**git status` :**

 	查看工作区中的文件状态

- - git log ： 提交日志
  - git rm 文件名 ：将文件从库移出，并删除文件。

如果不想删除本地文件，可以添加 --cached 参数，只从暂存区中移除而不删除文件。

git add 和 git rm 命令之后，都要 git commit 一次，git status 的记录才会变化

- - git diff 文件： 查看文件改动信息。

如果想查看文件与上一次 commit 有什么不同之处，可以使用该命令。

- - 版本回退：

git reset --hard HEAD^ 回退到上一个版本（commit之前的版本），

回退到上上一个版本 HEAD^^ 

如果回退步数较多，可以使用数字指定具体步数：HEAD~100 

- - 版本前进：

git reset --hard id(这里的id 指的是 get commit 后的数字字母)

可以通过 git reset --hard HEAD commit id(这里的id 指的是 get commit 后的数字字母)实现

## **3.- 远程仓库**

- 远程仓库是指将 git 仓库存放在远程服务器中，方便共享与协同开发。

- - 远程仓库连接需要授权，常规的连接方式有两种： https 和 ssh
  - https 每次连接时都需要提供用户名和密码。
  - ssh 只需要在远程服务器配置授权，即可免密连接。

- 将项目克隆到本地

- - git clone 黏贴 地址 ...

- **将本地文件提交到git**

- - 1.git add 文件名称 >> 暂存区
  - ​    git commit -m   '说明' >> 提交到本地库
  - \2. git push 提交远程仓库
  - \3. git pull 拉取 更新

### **3.1- 建立信任关系**

- 要使用 ssh 连接，需要配置 ssh 密钥来建立信任关系。
- 步骤：

1，生成密钥

 打开 git-bash 终端，输入 ssh-keygen -t rsa -C 邮箱 ，一路回车，直到看到光标在 $ 后面闪烁。

 邮箱为远程仓库的连接邮箱（即上面 git config 命令配置的 user.email ）。

2，进入用户主目录中找到.ssh目录

打开id_rsa.pub文件，复制里面的内容

将内容粘贴到gitHub或者gitee上

setting => SSH and GPG keys => add new => 输入标题并粘贴内容

与远程服务器建立信任关系之后，有两种情况：

1，将本地仓库的已有文件提交到远程的新仓库

这种情况要求远程仓库只能初始化

2，将远程仓库中的文件克隆到本地

这种情况是将远程仓库的文件下载到本地，不能克隆到本地已有的git仓库目录中

### **3.2- 提交本地仓库**

- 创建本地仓库

```js
/* 
    创建全新的本地仓库 （工作中大概率是没有权限自己创建的，都是由项目负责人创建好，我们只需要根据分配的仓库地址，使用 git clone 命令将远程已有仓库克隆到本地即可）

        1，创建仓库文件夹 （新建文件夹： mkdir 文件夹名称）

        2，进入仓库文件夹  （跳转目录：  cd 文件夹名称）

        3，初始化仓库
            git init

        4，添加文件到仓库
            git add .
            git commit -m '初始化项目'
*/
```

- 将本地仓库提交到远程
  - ==记得更名主分支 main==

```js
        /* 
        
        将本地的全新仓库提交到远程仓库中。
        
        前提： 远程仓库是一个没有初始化的默认仓库
            （gitee, github来说，就是没有点初始化readme.md 按钮）
        
        
        1，将本地仓库与远程仓库建立关联
        
            git init 初始化的分支默认名称是 master ，需要改成 main 才能与 github 的仓库名称对应上
                git branch -M main    这个是重命名的命令
        
            git remote add origin 远程仓库地址
        
            如：
            git remote add origin git@gitee.com:ickt-wl/new-git.git
        
        2，将本地仓库的内容提交到远程仓库
        
            gitee.com:
        
                git push -u origin master
        
            github.com:
                git push -u origin main
        */
```

- 创建和切换分支

```js
        /* 
        
        1，查看所有分支：  git branch
        
        2，创建新分支：   git branch 分支名称
            创建一个名称为 dev 的分支： git branch dev
        
        3，切换分支：   git checkout 分支名称
            从当前分支切换到 dev 分支：  git checkout dev
        
        4，合并写法：
            创建一个新分支，并且切换到这个新分支上
                git checkout -b 新分支名称
            
            如：
            创建一个名为 newDev 的分支，并切换到 newDev 分支
                git checkout -b newDev
        
        5，删除分支： git branch -d 分支名称
            如： 删除 dev 分支
                git branch -d dev
            -d 参数的 d 可以是大写，也可以是小写
        */
```

- 分支操作

```js
    <!-- 

    git branch 新分支名称
    git checkout 新分支名称

    或

    git checkout -b 新分支名称


    创建新分支 相当于是将项目文件夹复制出一个副本。
    在分支中开发，相当于是在副本项目中进行开发，所有的操作都不会影响原来分支。
    (所有删除新增都需要提交)
    工作中，建议都创建属于自己的新分支进行开发。
 -->
```

- 合并分支

```js
/* 
    项目主分支： 项目上线的分支，是合并了所有功能的分支（通常是 master 或 main 分支）

    开发分支 ： 开发人员在开发功能时，自己创建的分支

    合并分支：  在功能开发完成之后，将开发分支合并到项目主分支的操作。

    将其它分支合并到当前分支：
        git merge 要合并的分支名称

    比如： 将 dev 分支合并到 master 分支
        1，切换到 master 分支
        2，git merge dev

    合并过程中，如果合并过来的分支内容与当前分支已有的内容有交集关系（合并分支有与当前分支同名的文件，并且文件有修改，添加，或删除操作），在合并时，需要填写合并原因，具体操作步骤：
        1，按 "i" 键进入编辑状态 （编辑状态左下角会有 "--INSERT--" 标记）
        2，填写编辑说明  （行首有 # 号表示提示文字，不会作为编辑说明部分保存到提交日志）
        3，在英文输入法状态下，按  shift + 冒号，进入命令模式（左下角会有 : 和闪烁的光标提示）

        4，输入 wq （ w 保存   q 退出）


    工作中，合并分支通常是由专人负责。

*/
```

- 删除分支

```js
/* 

    删除分支： 在功能开发结束之后， 可以将无用的分支删除

        git branch -d 要删除的分支名称
*/
```

