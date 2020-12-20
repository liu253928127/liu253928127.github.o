---
title: 搭建hexo博客（三）：同时部署多个托管网站hexo+github+gitee
date: 2018-11-25 16:46:48
categories: 
- 环境
tags:
- hexo
---

> 上一节介绍了如何发布静态网页到github，但考虑到国内网速原因，想同时时间github和gitee多个托管网站，方便访问

### 同时发布到github和gitee

上一篇已经介绍如何发布到`github`，这里先介绍新建`gitee`项目

1. 在`gitee`上创建名为`xxx`的项目（与github有所区别），`xxx`为自己的`gitee`用户名

2. 设置项目的类型为`JavaScript`

3. 配置`deploy`多个源,官方的格式是
```
# 官方的格式
# deploy:
#   type: git
#   message: [message]
#   repo:
#     github: <repository url>,[branch]
#     gitcafe: <repository url>,[branch]

# 我的配置
deploy:
  type: git
  repo: 
    github: git@github.com:coofive/coofive.github.io.git,master
    gitee: git@gitee.com:coofive/coofive.git,master
```

### 将hexo提交到github，实现多台电脑使用

1. 在本地创建分支`hexo`
```
git checkout -b hexo
```

2. 查看分支状态
```
git branch
```

3. 本地`commit`后，提交本地分支到远程分支
```
git add .
git commit -m "hexo博客"
git remote add origin git@github.com:liu253928127/liu253928127.github.io.git
git push -u origin hexo
```

4. 有可能发布后样式加载不出来，一般是css路径访问不对，所以在gitee创建项目的时候按照固定格式来最好