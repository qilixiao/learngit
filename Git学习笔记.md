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
  > 撤销工作区中某个文件的修改，使用命令 git checkout --file     
撤销已经add到暂存区中的某个文件的修改，首先使用命令git reset HEAD file将暂存区的修改回退到工作区，再使用命令git checkout --file丢弃修改    
撤销已经commit到版本库但是没有推送到远程的修改，使用命令git reset --hard commit_id进行版本回退
* 删除文件
1. 方法一
```
rm file     #删除工作区文件
git add file    #将修改添加至暂存区
git commit -m "说明"    #将修改提交至版本库
```
2. 方法二
```
git rm file 
git commit -m "说明"  
```
 > rm file 和git rm file 的主要区别：    
rm file 仅仅删除了物理文件，并没有将其从git的记录中删除，必须使用git add 命令将修改添加至暂存区    
git rm file 会同时删除工作区和git记录中的文件
#### 远程仓库
* 创建SSH Key
` ssh-keygen -t -rsa -C "email@example@com"`
* 添加远程仓库
```
git remote add origin git@service-name:path/repo-name.git    #添加远程仓库
git remote    #查看远程仓库信息
          -v  #查看详细信息
git push -u origin master    #第一次推送master分支的内容至远程
# 除了第一个推送，后续推送不需要加-u 
```
* 从远程仓库克隆
```
git colne git@server-name:path/repo-name.git    #克隆远程仓库至当前目录
git pull origin    #拉去远程仓库的内容
```
#### 分支管理
```
git branch    #查看现有分支
git branch name    #创建分支
git checkout name    #切换分支
git checkout -b name    #创建并且切换至该分支             
git mrege name    #合并某分支到当前分支
          --no-ff name    #禁用fast-foward 快速合并
          --no-ff -m "提交说明" 
git branch -d name    #删除分支
           -D name    #强制删除分支，（常用于丢弃一个未合并的分支）
git stash    #保存当前工作环境（包括工作区和暂存区）
git stash list    #查看保存的工作列表
git stash apply [stash@{X}]    #恢复工作状态，但是不删除stash内容
git stash pop [stash@{X}]    #恢复工作状态，并且删除stash内容
git stash drop [stash@{X}]    #删除stash内容
```
* 多人协作
 > 使用git push origin branch-name 将本地分支推送至远程，若推送失败则需要先抓取远程的提交
```
##error: failed to push some refs to ...
git pull    #抓取远程的新提交，并且试图合并
#如果合并有冲突，则解决冲突，并在本地提交
```
  > 当git pull 命令失败，则说明本地和远程分支的链接关系没有创建
```
##error:no tracking information
git branch --set-upstream branch-name origin/branch-name    #建立本地分支和远程分支的关联
```
`git checkout -b branch-name origin/branch-name    #在本地创建与远程分支对应的分支，并且本地分支和远程分支的名字最好一致` 
#### 标签管理
```
git tag    #查看现有标签
git tag name    #在当前所在的commit新建一个标签
git tag commit_id    #给指定commit新建标签
git tag -a tag_name -m "标签说明" commit_id    #给指定commit新建标签，并且添加标签名字和说明等详细信息
        -s tag_name -m "标签说明" commit_id    #用GPG私钥签名
        -d tag_name    #删除标签
git show tag_name    #显示标签信息
git push origin tag_name    #将标签推送至远程
git push origin -tags    #将全部未推送过的标签推送至远程
git push origin:refs/tag/tag_name    #删除一个远程标签（先将本地标签删除，再使用该命令删除远程标签）
```
#### 自定义Git
```
git config --global color.ui true    #设置全局颜色显示
git config --global alias.alias_name command_name    #设置别名
```
* 忽略特殊文件
 > 创建.gitignore文件
 > 主要内容示例如下：
 ```
 # Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa
 ```
`git add -f file    #强制添加被忽略的文件`    
`git check-ignore -v <file>  #查看忽略该文件的规则`
> 规则有错时常用上述命令查找定位
* 配置别名列表
```
git config  --global alias.config 'config --global'
git config alias.st status
git config alias.co checkout
git config alias.ci commit
git config alias.br branch
git config alias.unstage 'reset HEAD'
git config alias.last 'log -1'
git config alias.lg 'log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit'
```
