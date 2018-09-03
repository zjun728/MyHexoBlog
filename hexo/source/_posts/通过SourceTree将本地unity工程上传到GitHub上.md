---
title: 通过SourceTree将本地unity工程上传到GitHub上
date: 2018-04-04 15:21:16
tags: [git,github,sourceTree]
---
# 前言

GitHub 是基于 Git 的一个代码托管网站。开发者可以将代码在 GitHub 上和别人共享，当然也可以浏览其它项目的代码。下面介绍的是
如何通过SourceTree将本地unity工程上传到GitHub上。

<!--more-->

# 步骤

一、1、创建github网站账号（GitHub网站： https://github.com）。2、安装sourceTree软件。

二、在GitHub网站上新建一个仓库，如下图：

{% asset_img SourceTreeGit1.png 创建库 %}


三、填写库信息

{% asset_img SourceTreeGit2.png 填写库的信息 %}

四、找到项目的git地址，并复制

{% asset_img SourceTreeGit3.png 复制项目地址 %}


五、打开SourceTree将刚才在GitHub上创建得项目克隆到本地

{% asset_img SourceTreeGit4.png 克隆项目 %}


六、将本地的unity项目中的Assets和Library文件夹拉到刚才创建的SecondGit文件夹中

{% asset_img SourceTreeGit5.png SourceTreeGit5.png %}


此时返回到SourceTree中，会看到刚才拉入文件夹中的文件会显示在未暂存文件区域中去

{% asset_img SourceTreeGit6.png SourceTreeGit6.png %}


接下来提交更新

{% asset_img SourceTreeGit7.png SourceTreeGit7.png %}


提交后会看到有推送提示，如下图

{% asset_img SourceTreeGit8.png 推送（可能会有点慢） %}


推送（可能会有点慢）

{% asset_img SourceTreeGit9.png 推送（可能会有点慢） %}


此时进入GitHub网站，点进我们刚才新建的库中中，会看到我们刚才提交的更新

{% asset_img SourceTreeGit10.png 更新 %}


这个时候如果我们在网站中更改里面的东西后（点击下面的Commit Change提交更改），
在sourcetree（网站中更新后在sourcetree中会有相应的拉取提示）中只需拉取一下即可将刚才更改的东西拉取到本地

{% asset_img SourceTreeGit11.png 更新本地 %}

----------------------------------------

{% asset_img SourceTreeGit12.png 更新本地 %}


此时，打开刚才本地的工程，找到在网站中更改的代码会看到相应的更新

{% asset_img SourceTreeGit13.png 本地已更新 %}


同理，本地更新后会在sourcetree中提示有未暂存的文件，提示你提交更新

{% asset_img SourceTreeGit14.png 提示更新 %}


然后只需要暂存所选或全部，提交，再点击推送即可

{% asset_img SourceTreeGit15.png 提示更新 %}


# 如果我们想从网站中拉取已有的项目，只需执行第五步，即可将项目拉到本地。







