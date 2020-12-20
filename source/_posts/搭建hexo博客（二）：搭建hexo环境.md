---
title: 搭建hexo博客（二）：搭建hexo环境
date: 2018-11-25 11:23:10
categories: 
- 环境
tags:
- hexo
---

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

### 环境搭建

#### 准备工作
 - 下载node.js并安装（官网下载安装）,默认会安装npm。

 - 下载安装git（官网下载安装）。

#### 安装hexo

1. hexo官网（https://hexo.io）

2. 设置npm镜像源为淘宝镜像，从国外镜像源下载很容易失败
```
npm config set registry http://registry.npm.taobao.org/
```

3. hexo安装
```
npm install -g hexo-cli
```

hexo的基本使用可查看官网文档或者官方示例`hello-world`

#### 本地搭建hexo静态博客

1. 新建一个文件夹，如`blog`

2. 进入该文件夹内，右键运行`cmd`，输入
```
hexo init
```

3. 初始化后，文件夹的目录如下
```
.
├── .deploy       # 需要部署的文件
├── node_modules  # Hexo插件
├── public        # 生成的静态网页文件
├── scaffolds     # 模板
├── source        # 博客正文和其他源文件等都应该放在这里
|   ├── _drafts   # 草稿
|   └── _posts    # 文章
├── themes        # 主题
├── _config.yml   # 全局配置文件
└── package.json
```

4. 加载node依赖
```shell
npm install
```

5. 本地运行，即可访问http://localhost:4000 hexo首页
```shell
hexo s
```

#### 发布到github

1. 在`github`上创建名为`xxx.github.io`的项目，`xxx`为自己的`github`用户名

2. 打开本地的`blog`文件夹项目内的`_config.yml`的配置文件，将其中的`type`设置为`git`
```
deploy:
  type: git
  repository: git@github.com:coofive/coofive.github.io.git
  branch: master
```

3. 安装`deploy`插件
```
npm install hexo-deployer-git –save
```

4. 本地生成静态文件
```
hexo g
```

5. 将本地静态文件推送至`github`,或者生成和部署一行命令`hexo g --d`
```
hexo d
```

6. 访问`http://xxx.github.io`就能看到刚刚本地部署的静态网页