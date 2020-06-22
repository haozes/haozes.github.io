---
layout: post
title: YaoYao 常见问题(FAQS)
date: 2020-05-26
categories: blog
tags: [Coding]
description: YaoYao-Frequently Asked Questions
---

[toc]

## 1. 安装后 Watch 上找不到 App
正常情况下手机 App 安装后，会同时将 Watch App 安装到手表，但电量不足，网络不稳定等情况下，手表上未定可以即时安装。 

##### 解决方案一：重启手机和 Watch

#####  解决方案二：
watchOS 6 后，可以从 Apple Watch上的 App Store 搜索: yaoyao，找到 App 安装(__不会重复扣费__)

#####  解决方案三：卸载重装
卸载重装时请切记保留健身记录，会弹两个框，第二个框要点保留！！！
![Delete Warnning](http://cdn.onlytalk.top/blog/faq-1_zh.jpg)


## 2. 手机端没有同步数据记录
YaoYao 的数据是保存在 HealthKit 健康中，手机端是通过从 Apple 服务端同步健身数据，手表电量太低有可能导致记录不同步。
正常情况下，几分钟之内，手机端或查看到相应的记录，如果长时间无法同步（此时使用 watch 自带的健身如跑步也无法同步）。请按以下步骤检查：
![Sync Problem](http://cdn.onlytalk.top/blog/faq-2.jpg)
#####  1. 健康->右上角头像->设备
#####  2. 找到无法配对的手表设备删除即可。
如果仍然无法同步，你需要重新配对你的手表。


## 3. 手机上版本更新了，Watch 上没有更新
有时手机 App 端更到了 2.0.1，手表上版本只有 2.0.0(比手机端版本低)
#####  1. 在手机 Watch App 上看下找到 YAOYAO 是否正在安装，或等待安装.
#####  2. 从Watch上的 App Store 检查更新
 从手表的程序坞里找到 App Store，点击“账户”->“更新”
 ![update from watch](http://cdn.onlytalk.top/blog/faq-3_zh.jpg)


## 4. 跳绳计算的次数比实际的偏多或偏少
如果误差超过 50%：
#####  1. 请重启手表
#####  2. 请检查是否勾选了“双摇”模式，双摇模式下单摇跳，会减半。
如果误差不大的情况则进行调节灵敏度：
如果 YaoYao 实际计算出的跳跃次数小于实际数，将灵敏度数据调大，反之，调小。正常误差在 1%~5%。
灵敏度设置：
![Sentivity](http://cdn.onlytalk.top/blog/faq4_zh.jpg)


## 5. 卡路里数据不准？（同样跳1000次，卡路里数不一样）
YaoYao 的卡路里数据是由 watchOS 计算得出，根据心率、运动传感器强度、时间、运动类型、年龄等计算的，并不是完全线性的结果，正常在一小时消息总卡路里600卡左右。
其次动态卡路里指的是运动消耗的卡路里，不包含静态消耗。

__总消耗卡路里 = 动态消耗路里+ 静息基础消耗__

YaoYao 默认显示的是动态卡路里，详情里有总卡路里显示。
表带未戴紧会严重影响心率和卡路里数据的准确性，建议戴紧一点，心率对卡路里影响最大！


## 6. App 健康隐私授权(手机上没有跳绳记录原因之一)
#####  手机上
 ![health privacy](http://cdn.onlytalk.top/blog/faq6_1_cn.jpg)

#####  手表上
App 首次运行会要求对健康隐私授权，以用来读取或写入各类运动数据，如果被忽略可以手动打开
![health privacy](http://cdn.onlytalk.top/blog/faq6_cn.jpg)


## 7. 强制退出手表端 YaoYao，避免重启手表
彻底退出手表上的YaoYao （首先长按手表右侧下方的长形按钮，等出现关机按钮后，再长按上面圆形按钮）

## 8.还有其他问题
你可以在 App 的邮件反馈、Bug 反馈中联系我们。
或者在以上渠道联系到我们： 

E-mail:89208242@qq.com  
QQ群：207527095  
微信: haozes  
Twitter: haozes
Telegram 群组: YaoYaoNow  
微信公众号: YaoYaoNow   


