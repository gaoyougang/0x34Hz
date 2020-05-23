---
title: hexo+github搭建个人博客(基于Archlinux)
date: 2020-05-15 10:45:48
author:
img:
top:
cover:
coverImg:
password:
toc:
mathjax:
summary:
categories: 技术
tags: hexo 
reprintPolicy:
---

# hexo+github搭建个人博客

## 环境搭建(基于archlinux)
### 安装搭建node.js和hexo环境
1. 利用yay安装：`yay -S nodejs`
2. 查看是否安装成功：`node -v`
3. 查看包管理器版本：`npm -v`
4. 修改npm源为国内源：`sudo npm install -g cnpm --registry=https://registry.npm.taobao.org`
5. 安装hexo(全局)：`sudo cnpm install -g hexo-cli`
6. 查看hexo是否安装成功：`hexo -v`
7. 建立博客文件夹：`mkdir blog`
8. 进入blog并初始化：`cd blog && sudo hexo init`
9. 启动预览博客：`hexo s`
10. 新建一篇文章：`hexo n "我的第一篇博客"`
11. 用编辑器编写生成**source/_posts/**下对应的.md文件：`cd source/_posts` &&
`vi 我的第一篇博客.md`(我使用的是nvim)
13. 书写采用markdown语法格式，详细请访问：[Markdown](https://www.runoob.com/markdown/md-tutorial.html)
14. 回到blog目录下：`cd -`或者`cd ../..`
15. 清理缓存并重新生成：`hexo clean && hexo g`
16. 运行浏览：`hexo s`

### 配置使用git
请参照上一篇文章[git 学习笔记](http://gaoyougang.gitee.io/myblog/2020/05/15/git-xue-xi-bi-ji/)
### 部署在github
1. 创建一个仓库和用户名一致，形式如：*username.github.io*
2. 回到blog目录，安装git插件：`cnpm install --save hexo-deployer-git` 
3. 然后复制仓库的SSH地址(这里我用的git是ssh钥匙链接的)或http地址
4. 修改blog文件下的\_config.yml，在底部添加：
```
deploy:
  type: git
  repo: SSH的地址或者http地址
  branch: master
#更多修改请谷歌查阅
```
5. 部署到远端：`hexo d`
6. 每次生成新内容，运行：`hexo clean && hexo g`
7. 本地查看:`hexo s`
8. 没问题推送到github：`hexo d`

## 更换博客主题
1. 访问[hexo](https://hexo.io/zh-cn/)官网有许多主题，选择自己喜欢的
2. 克隆主题到：*blog/themes*下
3. 修改blog文件下的\_config.yml文件,找到theme，修改：
`theme: 克隆到themes文件下的主题文件名`
4. 然后重新生成预览，没问题推送到github上.

***不同主题的美化请谷歌查询***

