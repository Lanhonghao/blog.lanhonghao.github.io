# 幽水蓼蓝

我的个人博客：<https://blog.lanhonghao.cn>，欢迎 Star 和 Fork。

## 概览

<!-- vim-markdown-toc GFM -->

- [幽水蓼蓝](#幽水蓼蓝)
  - [概览](#概览)
  - [效果预览](#效果预览)
  - [Fork 指南](#fork-指南)
  - [致谢](#致谢)

<!-- vim-markdown-toc -->

## 效果预览

**[在线预览 &rarr;](https://blog.lanhonghao.cn)**

## Fork 指南

Fork 本项目之后，还需要做一些事情才能让你的页面「正确」跑起来。

1. 正确设置项目名称与分支。

   按照 GitHub Pages 的规定，名称为 `username.github.io` 的项目的 master 分支，或者其它名称的项目的 gh-pages 分支可以自动生成 GitHub Pages 页面。

2. 修改域名。

   如果你需要绑定自己的域名，那么修改 CNAME 文件的内容；如果不需要绑定自己的域名，那么删掉 CNAME 文件。

3. 修改配置。

   网站的配置基本都集中在 \_config.yml 文件中，将其中与个人信息相关的部分替换成你自己的，比如网站的 url、title、subtitle 和第三方评论模块的配置等。

   **评论模块：** 目前支持 disqus、gitment 和 gitalk，选用其中一种就可以了，推荐使用 gitalk。它们各自的配置指南链接在 \_config.yml 文件的 Comments 一节里都贴出来了。

   **注意：** 如果使用 disqus，因为 disqus 处理用户名与域名白名单的策略存在缺陷，请一定将 disqus.username 修改成你自己的，否则请将该字段留空。

   **Gitalk修改:**在 \_config.yml 文件的 Comments 一节里，Gitalk 需要一个 Github Application，[点击这里申请](https://github.com/settings/applications/new)。创建后，获取 `Client ID` 和 `Client Secret` 填入你的 Gitalk 参数中。然后创建blog_comment用于gitalk Issue的仓库。
```
# Gitalk 
gitalk: 
		clientID: 'clientID'    #OAuth Application
		clientSecret: 'clientID'   #OAuth Application
		repo: blog_comment仓库   #刚创建blog_comment用于gitalk Issue的仓库名称
		owner: username    #github用户名
```

4. 删除我的文章与图片。

   如下文件夹中除了 template.md 文件外，都可以全部删除，然后添加你自己的内容。

   * \_posts 文件夹中是我已发布的博客文章。
   * \_drafts 文件夹中是我尚未发布的博客文章。
   * \_wiki 文件夹中是我已发布的 wiki 页面。
   * images 文件夹中是我的文章和页面里使用的图片。

5. 修改「关于」页面。

   pages/about.md 文件内容对应网站的「关于」页面，里面的内容多为个人相关，将它们替换成你自己的信息，包括 \_data 目录下的 skills.yml 和 social.yml 文件里的数据。


## 致谢

本博客Fork自[码志](https://mazhuang.org)项目，感谢！
