---
title: gulp和webpack区别
date: 2019-01-29 09:48:44
categories: web
tags:
    - gulp和webpack区别
copyright:
keywords:
description:
---
{% cq %}我愿化身石桥，受五百年风吹，五百年日晒，五百年雨淋，只求她从桥上经过{% endcq %}
<!--more-->
>[原文地址](https://www.cnblogs.com/lovesong/p/6413546.html)
# gulp

gulp强调的是前端开发的工作流程，我们可以通过配置一系列的task，定义task处理的事务（例如文件压缩合并、雪碧图、启动server、版本控制等），然后定义执行顺序，来让gulp执行这些task，从而构建项目的整个前端开发流程。

PS：简单说就一个Task Runner

# webpack

webpack是一个前端模块化方案，更侧重模块打包，我们可以把开发中的所有资源（图片、js文件、css文件等）都看成模块，通过loader（加载器）和plugins（插件）对资源进行处理，打包成符合生产环境部署的前端资源。
PS：webpack is a module bundle

# 相同功能

gulp与webpack可以实现一些相同功能，如下（列举部分）：

|     功能  | gulp  |  webpack  |
| ------------- |:-------------:| -----:|
| 文件合并与压缩（css）| 使用gulp-minify-css模块|样式合并一般用到extract-text-webpack-plugin插件压缩则使用webpack.optimize.UglifyJsPlugin  |
| 文件合并与压缩（js） |使用gulp-uglify和gulp-concat两个模块| js合并在模块化开始就已经做,压缩则使用webpack.optimize.UglifyJsPlugin 
| sass/less预编译	| 使用gulp-sass/gulp-less 模块	| sass-loader/less-loader 进行预处理 
| 启动server | 使用gulp-webserver模块 | 使用webpack-dev-server模块
| 版本控制 | 使用gulp-rev和gulp-rev-collector两个模块 | 将生成文件加上hash值 module.exports = {plugins:[new ExtractTextPlugin(style.[hash].css")}


# 两者区别
* 虽然都是前端自动化构建工具，但看他们的定位就知道不是对等的。
* gulp严格上讲，模块化不是他强调的东西，他旨在规范前端开发流程。
* webpack更是明显强调模块化开发，而那些文件压缩合并、预处理等功能，不过是他附带的功能。

# 总结
* gulp应该与grunt比较，而webpack应该与browserify（网上太多资料就这么说，这么说是没有错，不过单单这样一句话并不能让人清晰明了）。
* gulp与webpack上是互补的，还是可替换的，取决于你项目的需求。如果只是个vue或react的单页应用，webpack也就够用；如果webpack某些功能使用起来麻烦甚至没有（雪碧图就没有），那就可以结合gulp一起用。