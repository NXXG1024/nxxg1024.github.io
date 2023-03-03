---
title: reverse-shell
date: 2023-03-01 21:17:10
tags:
---
## (写在前面)
### 0.为什么想写这个
今天尝试搭建了vulhub靶场（CentOS），搭建过程中出了n个问题，但都在网上找到了答案，~~然后这个过程似乎好像可以再水一期博客~~，在查找的过程中发现了一个靶场的玩法是可以做反弹shell的，而且正巧，之前，在上课的过程中，老师也提到过反弹shell的知识，正好可以复习一下~~（现在看来，我上课压根就没听明白老师讲的反弹shell :{ ）~~，还好，还有时间去学。


### 1.什么是反弹shell（reverse shell）
反弹shell，就是攻击机监听在某个TCP/UDP端口为服务端，目标机主动发起请求到攻击机监听的端口，并将其命令行的输入输出转到攻击机。
[参考链接似乎好像没太看明白，emmm。。。](https://cloud.tencent.com/developer/article/1818091)
那咱换个说法（虽然也是个参考的，但我觉得讲的真的很不错）
- 先弹shell就是正向shell，后连接
- 后弹shell就是反向shell，先监听

**正向shell：客户端想要获得服务端的shell**
- 假设小黑是一名黑客
- 他悄咪咪溜进了知乎总部大楼，发现一台刘看山的电脑，但是刘看山出门了
- 于是小黑通过nc将这台电脑的控制权通过23333端口发射出去
- 小黑做了一系列的操作，又悄咪咪的原路返航
- 回到家后，通过nc连上刘看山的电脑
- 后来，刘看山的隐私泄露了~
- （这里小黑是客户端，刘看山是服务端，服务端发射shell）

**反向shell：服务端想要获得客户端的shell（也就是反弹shell）**
- 假设小黑是一名黑客
- 他悄咪咪溜进了知乎总部大楼，发现一台刘看山的电脑，但是刘看山出门了
- 小黑发现电脑里有一个有趣的文件（刘看山的秘密）
- 小黑下载这个文件到自己的U盘里，又悄咪咪的原路返航
- 回到家后，小黑打开这个文件
- 发现电脑被刘看山控制了~
- （这里小黑是客户端，刘看山是服务端，客户端发射shell）
这样一看，反弹shell似乎好像没那么难了（感谢博主）
这里附上链接：[参考链接](https://zhuanlan.zhihu.com/p/166165803)
### 2.漏洞复现：
1. 实验环境：CentOS7(VMware)，docker，vulhub/bash/shellshock
![图例1](/image/reverse-shell.png)
2. 实验过程：
	1. 首先,通过浏览器访问到对应页面![图例2](/image/first-step.png)
	2. 使用burpsuit抓包![图例3](/image/second-step.png)
	3. 修改User-Agent( () { :;};echo ; echo; echo $(/bin/ls /);)![图例4](/image/third-step.png)
	4. 发送修改后的报文：![图例5](/image/fourth-step.png)
	5. 注意观察收到的回复：（这就是漏洞）![回复结果](/image/response.png)
	6. 之后才是正题，大家可以期待一下哦:-(|)[提前感谢一下大佬的倾情奉献](https://blog.csdn.net/weixin_43071873/article/details/110233234?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-110233234-blog-124807426.pc_relevant_3mothn_strategy_recovery&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-110233234-blog-124807426.pc_relevant_3mothn_strategy_recovery&utm_relevant_index=1)
	7. 