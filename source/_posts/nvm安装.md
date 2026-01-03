---
layout: title
title: nvm安装
date: 2026-01-03 15:28:59
categories: web
tags: ["nvm安装"]
---
{% cq %} nvm安装 {% endcq %}

<!--more-->
## mac通过<font style="color:rgb(38, 38, 38);">Homebrew安装nvm</font>
1. 下载<font style="color:rgb(38, 38, 38);">Homebrew</font>

[Homebrew官网](https://brew.sh/)

2. 安装nvm

```shell
brew install nvm
```

3. 配置nvm环境变量

<font style="color:rgb(17, 17, 17);">   安装完成后，需要配置nvm的环境变量。打开你的shell配置文件（如</font>_**<font style="color:rgb(68, 68, 68);background-color:rgb(249, 249, 249);">~/.zshrc</font>**_<font style="color:rgb(17, 17, 17);">或</font>_**<font style="color:rgb(68, 68, 68);background-color:rgb(249, 249, 249);">~/.bash_profile</font>**_<font style="color:rgb(17, 17, 17);">），并添加以下内容</font>

```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$(brew --prefix nvm)/nvm.sh" ] && \. "$(brew --prefix nvm)/nvm.sh"
```

<font style="color:rgb(17, 17, 17);"> 保存文件后，重新加载配置文件：</font>

```shell
source ~/.zshrc # 或者 source ~/.bash_profile
```

  4. 验证安装

```shell
nvm -v
```

## <font style="color:rgb(38, 38, 38);">linux安装nvm</font>
1. <font style="color:rgb(34, 34, 34);">在线安装</font>

```shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
#或者 如果wget不存在 执行yum -y install wget
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

2. <font style="color:rgb(34, 34, 34);">本地安装</font>

```shell
#从官网下载安装
官网地址：https://github.com/nvm-sh/nvm/archive/refs/tags/v0.39.1.tar.gz

#将压缩包上传至服务器，如我当前位置/nvm

#新建服务器nvm地址
mkdir /root/.nvm

#将压缩包解压至/root/.nvm
tar -zxvf nvm-0.39.1.tar.gz --strip-components 1  -C /root/.nvm
```

3. <font style="color:rgb(34, 34, 34);">在bashrc里面写下相关配置(使用在线安装跳过这步)</font>

```shell
#编辑文件
vim ~/.bashrc

#写入配置
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```

4. <font style="color:rgb(34, 34, 34);">刷新配置即可正常使用</font>

```shell
#刷新配置
source ~/.bashrc

#判断nvm是否安装
nvm -v
```

5. <font style="color:rgb(34, 34, 34);">使用nvm下载相关node版本</font>

```shell
nvm install 14.13.2

#nvm常用命令
nvm uninstall 14.13.2     // 移除 node 14.13.2
nvm use 14.13.2           // 使用 node 14.13.2
nvm ls                   // 查看目前已安装的 node 及当前所使用的 node
nvm ls-remote            // 查看目前线上所能安装的所有 node 版本
nvm alias default 14.13.2 // 使用 14.13.2 作为预设使用的 node 版本
```

## window 安装
1. 下载 nvm-setup，并安装 nvm

<!-- 这是一张图片，ocr 内容为： -->
![](/img/imageNvm.png)

2. 如果 nvm 不能下载 14.17.5 版本， 可以在 node 历史版本直接下载，下载后可以放在 nvm 目录中

node 历史版本：[https://nodejs.org/dist/](https://nodejs.org/dist/)

<!-- 这是一张图片，ocr 内容为： -->
![](/img/imageNvm2.png)

3. 环境变量配置后，需要关闭终端，重新打开。

