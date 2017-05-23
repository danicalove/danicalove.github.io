---
layout: post
title: "拯救手残党 恢复误删除文件"
data: 2017-05-23 11:16:22
categories: 数据恢复
tags: 数据恢复
---

* content
{:toc}


前段时间做好的rna-seq数据被不小心删掉了，虽然我也不知道怎么删的，但是总之就是没了，还好报告上记录了，可以交差就行，被自己的蠢惊呆的同时，在网上也搜索了一些补救手段，以防这类事件的再次发生，颗颗。
![aa](http://mmbiz.qpic.cn/mmbiz_gif/884XsHGHibP44mapib23CLic6NcEicPlOzEibwZV2hFX3PqJXxYYWJic1KpCcK9RIJ1ibyf2A2mnMc1o6OWEMmops2XvA/0?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)








大多数linux发行版都提供一个debugfs工具，用来对Ext2文件系统进行编辑操作。在这之前，先做一些工作。

首先以只读方式挂载被误删的文件所在分区

比如以南医大生信服务器为例：
```
mount -r -n -o /storage/stu14230117
```
-r表示以只读方式挂载
-n表示不写入/etc/mtab

悲惨的是我并没有权限执行--no-mtab 命令

另外恢复

```
mount -r -n /dev/hda1/mnt/had
```

然后就可以执行debugfs

```
debugfs/dev/hda5  #假设linux在/dev/hda5
```

使用lsdel命令可以列出很多被删除的文件的信息

然后通过stat命令查看文件数据状态

再用dump指令恢复文件 

quit命令退出debugfs

另一种方法是手工编辑incode

使用mi指令后每次显示一行信息以供编辑，其他行可以直接按回车表示确认，把delection time改成c (未删除)，link count改成1 ，改好后退出debugfs

退出debugfs后用fsck检查

找到丢失的数据块放在lost-found目录里。


