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
