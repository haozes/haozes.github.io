---
layout: post
title: YaoYao-跳绳 为iOS14/watchOS7 智能化设计
date: 2020-09-23
categories: blog
tags: [Coding]
description: YaoYao iOS 14/watchOS new feature
---


如今的 iOS 变得越来越智能，iOS 提供了很多 Api 让其生态下的 App 融入iOS，成为智能的一份子。YaoYao 紧随 iOS 的发展设计了小组件，多功能表盘，Siri捷径。让我们一起来看一下，YaoYao 如何利用这些系统特性，让你的跳运动更加便捷。


### 小组件：
![widget.png](/img/post/smart/widget1.jpg)
在桌面上添加 YaoYao 的小组件，让你可以不用打开 app，随时看到本周的运动记录以及当日的目标进度，一星期都没运动了？shame on you!
#### 添加方法：
- iOS 14 长按屏幕，左上角出现加号，添加 YaoYao 的小组件即可。

### 多功能表盘:
![widget.png](/img/post/smart/watch7_cn.jpg)
你是一个跳绳重度用户，制作一个YaoYao专属「图文表盘」，最喜欢的运动模式一触即达！
watchOS7 的多功能表盘，让我们可以随意编辑自己喜欢的运动模式放在表盘上，最近一周的运动记录在表盘上也一目了然。

#### 添加方法：
- 长按手表屏幕，新建一个「图文模块」（该表盘需要SE 及series 4 及以上）
- 手动编辑选择 YaoYao 提供的表盘控件
![complication.png](/img/post/smart/complication_design.jpg)

### Siri捷径和Siri表盘
#### Siri 捷径
早晨，你对着 App Watch 说：“跳绳30分钟”，随即开始了一次目标30分钟的跳绳运动，无需打开App，设置目标。  

YaoYao 会根据你日常经常使用的运动模式生成相应快捷指令。watchOS7 将「捷径」带到了Watch 上，将快捷指令添加到 watch上，你就可以这么酷炫！
#### 添加方法：
![gen shortcut](/img/post/smart/genshortcut.jpg)
1. 从「设置」最底部处添加一个快速启动的快捷指令
2. 或在使用过后，YaoYao根据历史记录生成建议到系统的「快捷指令」中心
3. 在「快捷指令」中心，设置为在 Watch 上显示

![watch shortcut](/img/post/smart/watch_shortcut.jpg)
4. 在 watch 上打开「快捷指令」中看到这些指令，即成功，这些快捷指令都可以用语音呼唤了。



#### Siri 表盘
![siri face.png](/img/post/smart/siriface.jpg)
你是个加班狗，平时都是在晚上锻炼，晚上9点，抬腕一看，Siri表盘的建议你来「10组共3000次」的跳绳，直接点击开始，Siri就是这么懂你！

Siri表盘是所有表盘当中最能体现 watch 智能化的设计理念。系统会根据时间日历、日常使用习惯等，在当下时间显示最相关的建议。时间久了，我发现Siri表盘是我最常使用的表盘之一，你无需任何设置，系统会自动学习这一切。


### PS：YaoYao 变为免费下载啦！
原先 YaoYao 18￥，现在变为免费下载，30元升级PRO，原老用户，自动升级为PRO！喊你的朋友来试一试吧！

