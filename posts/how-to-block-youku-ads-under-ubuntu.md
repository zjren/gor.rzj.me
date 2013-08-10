---
title: ubuntu下屏蔽youku广告
date: '2013-08-11'
description:ubuntu下屏蔽youku广告
categories:
- Tips
tags:
- Tips
---

今天你发现一个屏蔽youku广告的方法，mark一下

编辑系统下的/etc/hosts文件，添加以下记录

    127.0.0.1       atm.youku.com
    127.0.0.1       fvid.atm.youku.com
    127.0.0.1       html.atm.youku.com
    127.0.0.1       valb.atm.youku.com
    127.0.0.1       valc.atm.youku.com
    127.0.0.1       valf.atm.youku.com
    127.0.0.1       valo.atm.youku.com
    127.0.0.1       valp.atm.youku.com
    127.0.0.1       vid.atm.youku.com
    127.0.0.1       walp.atm.youku.com
    127.0.0.1       lstat.youku.com
    127.0.0.1       speed.lstat.youku.com
    127.0.0.1       static.lstat.youku.com
    127.0.0.1       urchin.lstat.youku.com
    127.0.0.1       stat.youku.com

添加完后，测试广告已经被屏蔽，但是[原文](http://blogread.cn/it/article/6189 "彻底屏蔽优酷广告")分了两步来设置，但是我用的是chromium,没有找到第二部中提到的文件，但是仍然可用，不知为何。

*本文使用 [Cmd](http://ghosertblog.github.io/mdeditor/ "中文在线 Markdown 编辑器") 编写*

