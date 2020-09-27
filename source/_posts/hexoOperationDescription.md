---
title: hexoOperationDescription hexo操作说明
date: 2020-09-27 11:48:23
tags: hexo
---

## hexo的简单操作说明
### 1、git的操作
```
1、git pull 拉去线上版本
2、git add . 提交本地版本到暂存区,并且会根据.gitignore做过滤. git add * 不会根据.gitignore做过滤
3、git commit -m ''  用于提交暂存区文件，引号内用于说明本次提交的说明
4、git push -u origin master 将暂存区文件提交至线上git仓库
```
### 2、hexo的命令行指令
```
1、hexo new post 'title' 创建一个名为title的文章在首页的文章列表中，之后在source/_posts目录下面，多了一个title.md的文件。
2、hexo new page 'title' 创建一个名为title的左侧菜单栏,在此之前需要在themes主题文件夹下的配置文件config.yml找到menu，在此目录下添加新的菜单栏
```
### 3、执行与部署到服务器
```
1、hexo server 运行至本地，可以进行本地浏览
2、hexo clean  清除本地缓存,防止部署时出现错误
3、hexo deploy 一键部署到github服务器上
********************************切记每次一键部署时,都要先hexo clean*************************************
```