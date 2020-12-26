---
layout:     post
title:      Bug记录之homebrew更新清理问题
subtitle:   mac上用brew下载软件包遇到的问题以及解决方案
date:       2020-12-25
author:     Zhao Zihao
header-img: img/the-first.png
catalog: false
tags:
    - Bug
---

记录一些自己使用brew(homebrew)遇到的问题

## Record1:更新问题

正常情况下，在终端中输入` brew`会显示：

![正确情况下](https://tva1.sinaimg.cn/large/0081Kckwly1glxu04k0hij30g70cktap.jpg)

但有时在用brew下载某个包或者macbook系统大更新后很长时间没有更新使用homebrew，可能就会报错。

如果报错内容是提示brew的**type，update**等问题，可以输入：

` brew update-reset`

然后再次向终端中输入` brew`，检查是否显示上图的内容，如果成功就运行原来的命令行

## Record2:清理问题

如果内存不够用或者想清理一下mac，可以运行命令` brew cleanup`来清理一些旧版本的软件包安装包

但有的时候会报错：

` brew cleanup: Error: Permission denied @ apply2files`

意思是没有权限无法成功

**解决方案如下：**

如果给文件大权限，是无法清理的，需要给报错文件上层文件夹777或755权限。但给上层就有可能是其他文件开始报错了

所以最佳解决方式是：**给需要清理的大目录权限**

**具体操作如下：**

我报的错是：

` Permission denied @ apply2files - /usr/local/lib/node_modules/@vue/cli/node_modules/vue/src/platforms/weex/.DS_Store`

所以我可以给vue这个大目录权限

` sudo chmod -R 777 /usr/local/lib/node_modules/@vue`
