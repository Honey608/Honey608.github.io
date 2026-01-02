---
title: 使用github和hexo搭建个人博客
date: 2019-1-24 14:16
categories:	['hexo的使用']
tags: 
  - hexo
  - nexT
  - 搭建自己博客
  - 解决next主题搜索loadding问题
copyright: true
---

{% cq %} 所谓博客，都是孤芳自赏 {% endcq %}

现在越来越多的人喜欢利用Github搭建静态网站，原因不外乎简单省钱。本人也利用hexo+github搭建了本博客，用于分享一些心得。在此过程中，折腾博客的各种配置以及功能占具了我一部分时间，在此详细记录下我是如何利用hexo+github搭建静态博客以及一些配置相关问题，以免过后遗忘，且当备份之用。
<!--more-->
# 准备工作

 下载node.js并安装（官网下载安装），默认会安装npm。
 下载安装git（官网下载安装）
 下载安装hexo。方法：打开cmd 运行npm install -g hexo

# 本地搭建hexo静态博客

 新建一个文件夹，如MyBlog
 进入该文件夹内，右击运行git，输入：hexo init（生成hexo模板，可能要翻墙）
 生成完模板，运行npm install（目前貌似不用运行这一步）
 最后运行：hexo server （运行程序，访问本地+localhost:4000可以看到博客已经搭建成功）

# 站内搜索配置

首先安装hexo-generator-searchdb插件
> npm install hexo-generator-searchdb --save

编辑博客根目录下的博客本地目录/_config.yml站点配置文件，新增以下内容到任意位置，search顶格放否则可能没效果：

    search:
      path: search.xml
      field: post
      format: html
      limit: 10000

编辑博客本地目录/themes/next/_config.yml 主题配置文件，启用本地搜索功能,将local_search:下面的enable:的值，改成true
可以输入以下命令，先清理缓存，然后本地部署调试
>hexo clean 
hexo s

## 搜索无效、一直loading的问题
按F12可以查看请求命令的状态，状态码200表示请求成功。但是搜索动画还是一直在转。
1.文章中包含特殊字符，文件编码时出错
<img src="/img/Garbled.png">

# 部署github
MyBlog文件夹安装 npm install --save hexo-deployer-git
 找到 *_config.yml*文件修改一下文件
deploy:
type: git
repo: https://github.com/Honey608/Honey608.github.io.git
branch: master
<img style="width:600px;height:200px" src="/img/urlimg.png" class="full-image" /> 最后运行hexo clean(清除) hexo g(生成) hexo d(部署)
点击查看效果: https://zhanghuaxiao.github.io/
<img style="width:600px;height:200px" src="/img/github-pages.png" class="full-image">
# 使用nexT主题
 安装 [git clone](https://github.com/iissnan/hexo-theme-next themes/next) 
 修改_config.yml文件中 theme:next
 运行
 >hexo clean 
 hexo s
# hexo和nexT中文网

 [hexo中文网](https://hexo.io/zh-cn/docs/helpers)
 [nexT中文官网](http://theme-next.iissnan.com/getting-started.html)

# nexT主题参考文章

 [对nexT主题设置](https://segmentfault.com/a/1190000009544924#articleHeader2)
 [seo优化](https://www.jianshu.com/p/86557c34b671)
 [Hexo博客Next主题站内搜索模块相关，解决搜索无效、一直loading的问题](https://www.jianshu.com/p/02afabcae502)