---
layout: post
title: "邪恶的bash命令"
date: 2017-06-03 16:51:22
categories: linux bash
tags: linux bash
---

* content 
{:toc}


使坏必备 内心的小邪恶 

![image](http://mmbiz.qpic.cn/mmbiz_jpg/lcaq0oMjdFwR0aTT5LQcVFeUgy1ib57SNNPZvsBmbGibEbpwcdw5xxxch1JGby3LuDlZAvVbzf3xia3ca26bITkPw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)


1.可怕的默认编辑器
```
export EDitor=/bin/rm
```
editor变量用于定义系统的默认编辑器，在一些系统内置功能里面，比如编辑crontab时，会根据该变量调用默认编辑器，文件biu的一下没了。哈哈哈哈哈哈哈。

![image](http://mmbiz.qpic.cn/mmbiz_jpg/lcaq0oMjdFwR0aTT5LQcVFeUgy1ib57SN3IaZ1fBmXXhicCUJb4bcyWlTMIC8Qqo8bKibo3sXsrreQibh8ZNWohicAw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

2.猥琐的制表符

```
tset -Qe $'\t'
```
tset用于设置终端特征 ；-e参数设置擦除字符，缺省为退格字符 ；-Q表示不显示设置信息（静默）

![image](http://mmbiz.qpic.cn/mmbiz_jpg/lcaq0oMjdFwR0aTT5LQcVFeUgy1ib57SNVF0swHg72qWdia7o8mA2MK6c86ZLOFc42gfxhGVdm0x2icKs5EabiaLYQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

就酱









