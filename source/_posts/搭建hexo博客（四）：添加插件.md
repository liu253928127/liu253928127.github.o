---
title: 搭建hexo博客（四）：添加插件
date: 2020-12-20 13:09:53
categories: 
- 环境
tags:
- hexo
---

> 在完整的搭建了hexo博客之后，基本功能都可以使用，这时可以添加一些自定义的插件，丰富自己的博客

### 主题 - next

1. 下载主题

   目前用的hexo版本是 `5.3.0`，目前next主题 `5.0+` ，`7.0+` 和 `8.0+` 版本的地址分别为不同的地址，这里选用 7.8.0 release 稳定版本

   > 注：不同的hexo版本和next版本可能存在兼容性稳定，目前用的 `hexo 5.3.0` 和 `next 7.8.0` 版本可以正常运行

   ```http
   https://github.com/theme-next/hexo-theme-next/releases
   ```

2. 配置主题

   -  将上面下载的zip包解压至 hexo 博客目录下的 `themes/` 下

   - 将文件夹重命名 `next-7.8.0`

     ```json
     ├── themes        # 主题
     |   └── next-7.8.0    # next主题
     ```

   - 博客的配置文件 `_config.yml` 修改主题为 `next-7.8.0`

     ```yaml
     # Extensions
     ## Plugins: https://hexo.io/plugins/
     ## Themes: https://hexo.io/themes/
     theme: next-7.8.0
     ```



### 评论插件 - Valine

1. 获取APP ID 和 APP KEY

   需要先[登录](https://leancloud.cn/dashboard/login.html#/signin)或[注册](https://leancloud.cn/dashboard/login.html#/signup) `LeanCloud`, 进入[控制台](https://leancloud.cn/dashboard/applist.html#/apps)后点击左下角[创建应用](https://leancloud.cn/dashboard/applist.html#/newapp)，这个时候会生成 `app id` 和 `app key` 

2. 主题添加评论插件

   因为next主题直接集成了`Valine`插件，所以只需要配置自己的 `app id` 和 `app key` 即可

   > 注：这里更改配置的地方为 next 主题内的 `_config.yml`

   ```yaml
   # Valine
   # For more information: https://valine.js.org, https://github.com/xCss/Valine
   valine:
     enable: true
     appid: # Your leancloud application appid
     appkey: # Your leancloud application appkey
     notify: false # Mail notifier
     verify: false # Verification code
     placeholder: Just go go # Comment box placeholder
     avatar: mm # Gravatar style
     guest_info: nick,mail,link # Custom comment header
     pageSize: 10 # Pagination size
     language: # Language, available values: en, zh-cn
     visitor: false # Article reading statistic
     comment_count: true # If false, comment count will only be displayed in post page, not in home page
     recordIP: false # Whether to record the commenter IP
     serverURLs: # When the custom domain name is enabled, fill it in here (it will be detected automatically by default, no need to fill in)
     #post_meta_order: 0
   ```



### 统计插件 - leancloud_visitors

1. 获取APP ID 和 APP KEY

   需要先[登录](https://leancloud.cn/dashboard/login.html#/signin)或[注册](https://leancloud.cn/dashboard/login.html#/signup) `LeanCloud`, 进入[控制台](https://leancloud.cn/dashboard/applist.html#/apps)后点击左下角[创建应用](https://leancloud.cn/dashboard/applist.html#/newapp)，这个时候会生成 `app id` 和 `app key` 

2. 配置

   同样因为next主题集成了相关的插件，这里只需要将`app id` 和 `app key` 添加到配置文件即可

   > 注：这里更改配置的地方为 next 主题内的 `_config.yml`

   ```yaml
   # Show number of visitors of each article.
   # You can visit https://leancloud.cn to get AppID and AppKey.
   # AppID and AppKey are recommended to be the same as valine's for counter compatibility.
   # Do not enable both `valine.visitor` and `leancloud_visitors`.
   leancloud_visitors:
     enable: true
     appid: # Your leancloud application appid
     appkey: # Your leancloud application appkey
     # Required for apps from CN region
     server_url: # <your server url>
     # Dependencies: https://github.com/theme-next/hexo-leancloud-counter-security
     # If you don't care about security in leancloud counter and just want to use it directly
     # (without hexo-leancloud-counter-security plugin), set `security` to `false`.
     security: true
   ```



### 遇到的问题

1. 现象：配置了next主题，无法显示 `categories` `tags` 等选择栏

   解决：手动创建页面，并配置好`index.md`

   ```shell
   hexo new page "tags"
   ```

   配置 `tags` 的 `index.md`

   ```markdown
   ---
   title: tags
   date: 2020-12-19 22:38:35
   type: tags
   layout: tags
   ---
   ```

   ```shell
   hexo new page "categories"
   ```

   配置 `categories` 的 `index.md`

   ```markdown
   ---
   title: categories
   date: 2020-12-19 22:32:35
   type: categories
   layout: categories
   ---
   ```

   