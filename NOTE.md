Git 学习笔记
==
#### 用户配置
```
git config --global user.name "Name" 
git config --global user.emali "email@example.com"
```
--global 参数设置机器上所有Git仓库都使用该用户名和Email地址。
#### 初始化Git仓库
`git init`     #将当前所在目录初始化为Git仓库    
初始化的目录可以不为空
#### 添加文件到Git仓库
```
git add <file>    #添加到暂存区（stage）
        -f <file>  #强制添加至暂存区（可用于添加被忽略的文件）
git commit  -m "提交说明"  #从暂存区提交至版本库
```
git add 命令可以多次使用，添加多个文件。
#### 时光穿梭机
* 查看状态、差异
```
git status    #查看工作区状态
git diff [file]    #比较工作区和暂存区的差异
git diff --cached [file]    #比较暂存区和版本库的差异
git diff HEAD [file]    #比较工作区和版本库的差异
```
* 版本切换
```
git log    #查看提交历史
git log -1   #查看最后一次提交信息（-2 查看最后两次提交信息）
        --pretty=oneline    #单行显示提交历史
        --graph    #显示分支合并图
        --abbrev-commit    #显示简写的commit_id
git reflog    #查看所有操作信息，包括删除操作的commit_id
git reset --hard commit_id    #切换到指定版本
git reset --hard HEAD^    #回退到上一版本
# HEAD表示当前版本   HEAD^表示上一版本   HEAD^表示上上一版本 HEAD~100表示往上100个版本
```
* 撤销修改
```
git checkout --file    #撤销工作区的修改 
# 工作区文件修改后还未提交至暂存区，撤销修改则回到版本库中的状态；
# 工作区文件已经提交至暂存区，又重新做了修改，撤销修改则回到暂存区中的状态；
# 文件回到最近一次git commit 或者git add 的状态
git reset HEAD file    #撤销暂存区的修改
# 将暂存区的修改撤销掉，使其回到版本库的状态，并且将修改回退到工作区
```
撤销工作区中某个文件的修改，使用命令 git checkout --file    
撤销已经add到暂存区中的某个文件的修改，首先使用命令git reset HEAD file将暂存区的修改回退到工作区，再使用命令git checkout --file丢弃修改    
撤销已经commit到版本库但是没有推送到远程的修改，使用命令git reset --hard commit_id进行版本回退

