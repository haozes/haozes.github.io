---
layout: post
title: YaoYao 常见问题(FAQS)
date: 2020-05-26
categories: blog
tags: [Coding]
description: YaoYao-Frequently Asked Questions
---
* [1. 安装后 Watch 上找不到 App](#install_error)
* [2. 手机端没有同步数据记录](#sync_error)
* [3. App 健康隐私授权(手机上没有跳绳记录、心率N/A)](#auth_error)
* [4. 跳绳计算的次数比实际的偏多或偏少](#jumps_error)
* [5. 心率或卡路里数据不准？（同样跳1000次，卡路里数不一样）](#ca_error)
* [6. 手机上版本更新了，Watch 上没有更新](#Watch)
* [7. 边跳绳边用耳机听歌，音乐容易被打断](#LiveCount)
* [8. 强制退出手表端 YaoYao，避免重启手表](#YaoYao)
* [9. YaoYao 目前支持哪些平台](#platform)
* [10. 还有其他问题](#misc)

##  <a name='install_error'></a>1. 安装后 Watch 上找不到 App
正常情况下手机 App 安装后，会同时将 Watch App 安装到手表，但电量不足，网络不稳定等情况下，手表上未定可以即时安装。 

#####  解决方案一：关闭手机 WIFI，使用 4G 网络下载
#####  解决方案二：
从 Apple Watch上的 App Store 搜索: yaoyao，找到 App 安装(__不会重复扣费__)

##### 解决方案三：重启手机和 Watch，或卸载重装


##  <a name='sync_error'></a>2. 手机端没有同步数据记录
1. 检查健康隐私数据读取授权，见常见问题 5（ App 健康隐私授权）
2. YaoYao 的数据是保存在 HealthKit 健康中，手机端是通过从 Apple 服务端同步健身数据，手表电量太低有可能导致记录不同步。
正常情况下，几分钟之内，手机端或查看到相应的记录，如果长时间无法同步（此时使用 watch 自带的健身如跑步也无法同步）。请按以下步骤检查：
![Sync Problem](http://cdn.onlytalk.top/blog/faq-2.jpg)
#####  1. 健康->右上角头像->设备
#####  2. 找到无法配对的手表设备删除即可。
如果仍然无法同步，你需要重新配对你的手表。


##  <a name='auth_error'></a>3. App 健康隐私授权(手机上没有跳绳记录、心率N/A)
App 首次运行会要求对健康隐私数据授权，以用来读取或写入各类运动数据，如果被忽略可以手动打开

#####  手机上
1. 打开 「设置」->「隐私」->「健康」-> 找到「YaoYao」->「打开所有类别」

#####  手表上
1. 打开 「设置」->「健康」->「App」-> 找到「YaoYao」->「打开所有类别」
![health privacy](http://cdn.onlytalk.top/blog/faq6_cn.jpg)

##  <a name='jumps_error'></a>4. 跳绳计算的次数比实际的偏多或偏少
如果误差甚至超过 50%：
#####  1. 请重启手表/重装App
#####  2. 请不要在使用 YaoYao 时，同时运行其他手表上的运动 App
#####  3. 请检查是否勾选了“双摇”模式，双摇模式下单摇跳，会减半。

如果误差不大的情况则进行调节灵敏度：
如果 YaoYao 实际计算出的跳跃次数小于实际数，将灵敏度数据调大，反之，调小。正常误差在 1%~5%。
灵敏度设置：
![Sentivity](http://cdn.onlytalk.top/blog/faq4_zh.jpg)


##  <a name='ca_error'></a>5. 心率或卡路里数据不准？（同样跳1000次，卡路里数不一样）
YaoYao 的卡路里数据是由 watchOS 计算得出，根据心率、运动传感器强度、时间、运动类型、年龄等计算的，并不是完全线性的结果，正常在一小时消息总卡路里600卡左右。  
__要让触感引擎和电极式心率传感器及光学心率传感器等功能实现最佳效果，需要你戴紧表带，否则会导致心率和卡路里数据不准__，常见心率检测失败，如：手表app 上显示心率读数灰色，运动结果详细数据中心率曲线平直，

其次动态卡路里指的是运动消耗的卡路里，不包含静态消耗。  
总消耗卡路里 = 动态消耗路里+ 静息基础消耗  
YaoYao 默认显示的是动态卡路里，详情里有总卡路里显示。


##  <a name='Watch'></a>6. 手机上版本更新了，Watch 上没有更新
有时手机 App 端更到了 2.0.1，手表上版本只有 2.0.0(比手机端版本低)
#####  1. 在手机 Watch App 上看下找到 YaoYao 是否正在安装，或等待安装.
#####  2. 从Watch上的 App Store 检查更新
 从手表的程序坞里找到 App Store，点击“账户”->“更新”
 ![update from watch](http://cdn.onlytalk.top/blog/faq-3_zh.jpg)

##  <a name='LiveCount'></a>7. 边跳绳边用耳机听歌，音乐容易被打断
- 跳绳的时候可把 iPhone 放在桌上或支架  
- 打开 iPhone 端的 YaoYao App  
- 在 YaoYao Watch App 上开始  
这样 iPhone 上小鹿将会和你同步跳跃 ，不仅实时显示跳跃数，还能大声播报。或者如果你喜欢边跳边听音乐，不论是耳机连接Watch，或者 iPhone，听到播报声音将更加清晰。 

Live Count功能详细参考：
![livecount](https://cdn.sspai.com/2020/05/12/edf762cb42cef688570528e37c4a175e.gif)
https://zhuanlan.zhihu.com/p/139867684

##  <a name='YaoYao'></a>8. 强制退出手表端 YaoYao，避免重启手表
打开YaoYao App, 首先长按手表右侧下方的长形按钮，等出现关机按钮后，再长按上面圆形按钮,即彻底退出手表上的YaoYao 进程

##  <a name='platform'></a>9.YaoYao 目前支持哪些平台？
目前支持 Apple Watch（所有型号）, 三星 Galaxy Watch（1，2，3代）手表，另支持微信小程序。 

##  <a name='misc'></a>10.还有其他问题
你可以在 App 的邮件反馈、Bug 反馈中联系我们。
或者在以上渠道联系到我们： 

E-mail: [haozes@gmail.com](mailto:haozes@gmail.com)  
Twitter: [haozes](https://twitter.com/haozes)  
Wechat: haozes  
Telegram Channel: [YaoYaoNow](https://t.me/yaoyaonow)   
QQ群：207527095  
微信: haozes  
微信公众号: YaoYaoNow   

