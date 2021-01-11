---
title: Git以及Hexo博客使用经验
date: 2021:1:11 21:54
categories: 软件使用经验
---

## Github Pages + Hexo搭建个人网站
1. GitHub Pages和Hexo的介绍
   - GitHub： Pages 是用来托管Github上静态网页的免费站点
   - Hexo：hexo是一个简单快速强大的静态博客框架
2. 安装Node.js、Git、Hexo  
3. 进行Hexo的初始化配置
4. 在github中创建github.io远程仓库
5. 将本地的Hexo文件更新到Github库中
   - 在_config.yml文件中修改repository，添加远程链接

## 连接本地仓库与github远程仓库
1. 终端输入```git init ```
2. 使用git bash时，需要git config配置个人信息
   - ```git config --global user.name "用户名"```
   - ```git config --global user.mail 邮箱@163.com```
3. 上传代码至暂存区 
   - ``` git add * ```  提交所有文件
   - ```git add 文件名``` 提交当前文件
   - ```git commit -m "提交说明"```
4. 过程中可以使用指令来查看当前状态
   - ```git status``` 查看当前操作状态
   - ```git log```   查看提交记录
   - ```git diff``` 查看工作区与暂存区的差异
5. 创建分支
   - ```git branch name``` 创建新的分支
   - ```git branch```  查看分支状态
   - ```git checkout name``` 切换分支
   - ```git merge name``` 合并分支
   - ```git branch -d name``` 删除分支
   - 每次在切换分支前，提交当前分支
6. 提交至远程仓库
   - git push github main:main
7. 操作结束 

## 从远程仓库拉取项目到本地
- 本地需要初始化仓储
- ```git pull + 远程仓库链接```

## 从远程仓库克隆项目到本地
- ```git clone + 远程仓库链接```
- pull和clone一样，一般常使用pull，`pull只会做相应的合并，而多次使用clone会覆盖本地内容`
