---
layout: post
title: FlashPlayer不弹出错误信息的问题
description: ""
category: program
analytics: true
tags: [FLASH, AS3, debug]
---

首先，要安装debug版的flahsplayer
http://www.adobe.com/support/flashplayer/downloads.html

然后找到flashplayer的配置文件，位置如下：

Windows C:\\Documents and Settings\\username\\mm.cfg
OSX /Library/Application Support/Macromedia/mm.cfg
Linux home/username/mm.cfg

然后删掉所有的配置项

具体配置选项的项目请参看
http://jpauclair.net/mm-cfg-secrets/
