---
layout:     post
title:      Bug记录之mac上完成删除mysql
subtitle:   删除mysql及相关的配置文件
date:       2020-12-27
author:     Zhao Zihao
header-img: img/tag-bg.jpg
catalog: false
tags:
    - Bug
---


有时我们在安装新版本的mysql之前，需要从电脑上删除旧版本的mysql

其中mac从官网[https://www.mysql.com/downloads/](https://www.mysql.com/downloads/)下载的mysql默认路径是` /url/local/mysql`

可以在系统偏好设置里看到：

![](https://img-blog.csdnimg.cn/img_convert/0d31557536180840b2b22297703271a5.png)

如果直接点图标，点击uninstall卸载，卸载并不完全，本地还会残留一些配置文件

![](https://img-blog.csdnimg.cn/img_convert/068eefa0dc88b9ce440058cacc050067.png)

想彻底删除mac上的mysql可以进行以下操作：

打开终端，输入命令行，进入mysql的文件目录

```markdown
cd /usr/local/
```

依次逐行执行下列命令：

```markdown
sudo rm /usr/local/mysql
sudo rm -rf /usr/local/mysql*
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
rm -rf ~/Library/PreferencePanes/My*
sudo rm -rf /Library/Receipts/mysql*
sudo rm -rf /Library/Receipts/MySQL*
sudo rm -rf /var/db/receipts/com.mysql.*
```

执行完毕后，mysql就完全删除了
