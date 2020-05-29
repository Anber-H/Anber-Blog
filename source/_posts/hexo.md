---
title: 快速搭建个人博客
date: 2020-05-29 16:13:40
tags:
---
# 全局安装hexo
```
npm install -g hexo
```

# 初始化项目
```
hexo init   //自动构建项目
hexo s  // 本地运行
```
<!--more-->
# 部署到github
修改根目录下的_congif.yml
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/Anber-H/Anber-H.github.io.git
  branch: master
```
# 去github新建仓库
    注：Repository name 必须是 name.github.io    (name必须和Owner一样)
### 用hexo-deployer-git推到仓库
```
npm install hexo-deployer-git --save
```
### 部署到github
```
hexo clean

hexo generate

hexo deploy
```