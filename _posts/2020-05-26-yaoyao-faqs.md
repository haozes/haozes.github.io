---
layout: post
title: YaoYao 常见问题(FAQS)
date: 2020-05-26
categories: blog
tags: [Coding]
description: YaoYao-Frequently Asked Questions
---

## 1. 安装后 Watch 上找不到 App
正常情况下手机 App 安装后，会同时将 Watch App 安装到手表，但电量不足，网络不稳定等情况下，手表上未定可以即时安装。 

##### 解决方案一：重启手机和 Watch

#####  解决方案二：
watchOS 6 后，可以从 Apple Watch上的 App Store 搜索: yaoyao，找到 App 安装(不会重复扣费)

#####  解决方案三：卸载重装
卸载重装时请切记保留健身记录，会弹两个框，第二个框要点保留！！！
![Delete Warnning](/img/post/faq-1_zh.jpg)


## 1. No app found on Watch after installation
Normally, the Watch App will be installed on your watch after the mobile app is installed, but the watch may not be able to be installed instantly due to low battery or unstable network. 
#####  Solution 1：Reboot
Reboot the iPhone and Watch.
#####  Solution 2: Install from watch App Store
After watchOS 6, you can search: YaoYao from the AppStore on apple watch to find the app installation.（No duplicate charges）
#####  Solution 3: Reinstall
Be sure to keep workout records when uninstall, two options will pop up and please click "save" in the second pop-up.  
![Delete Warnning](/img/post/faq-1_en.jpg)

## 2. 手机端没有同步数据记录
YaoYao 的数据是保存在 HealthKit 健康中，手机端是通过从 Apple 服务端同步健身数据。
正常情况下，几分钟之内，手机端或查看到相应的记录，如果长时间无法同步（此时使用watch自带的健身如跑步也无法同步）。请按以下步骤检查：
![Sync Problem](/img/post/faq-2.jpg)
#####  1. 健康->右上角头像->设备
#####  2. 找到无法配对的手表设备删除即可。
如果仍然无法同步，你需要重新配对你的手表


## 2.Workout data cannot synced to the iPhone.
YaoYao's workout data is stored in HealthKit, and your iPhone is synchronized with the Apple server.
Normally, within a few minutes, the corresponding records will be available on your iPhone. In case they are not synced for a period of time, please follow these steps to check.
![Sync Problem](/img/post/faq-2.jpg)
1. Health->Avatar(upper right corner)->Devices,
2. Delete the watch device that cannot be paired.

If it still doesn't sync, you may need to re-pair your watch.

## 3. 手机上版本更新了，Watch 上没有更新
有时手机 App 端更到了 2.0.1，手表上版本只有 2.0.0(比手机端版本低)
#####  1. 在手机 Watch App 上看下找到 YAOYAO 是否正在安装，或等待安装.
#####  2. 从Watch上的 App Store 检查更新
 从手表的程序坞里找到app store，点击“账户”->“更新”
 ![update from watch](/img/post/faq-3_zh.jpg)


## 3. App  version updated on iOS, but not on Watch
Sometimes, the YaoYao iOS app version is updated to 2.0.1, but the watch version is only 2.0.0.
#####  1. Check on the Watch App to see if YAOYAO is installed.
#####  2. Check for updates from the App Store on your Watch.
Find the app store from the watch's app dock and tap on "Account"->"Update", update it manually.
 ![update from watch](/img/post/faq-3_en.jpg)

## 4. 跳绳计算的次数比实际的偏多或偏少
如果误差超过 50%：
#####  1. 请重启手表
#####  2. 请检查是否勾选了“双摇”模式，双摇模式下单摇跳，会减半。
如果误差不大的情况则进行调节灵敏度：
如果YaoYao 实际计算出的跳跃次数小于实际数，将灵敏度数据调大，反之，调小。正常误差在1%~5%。
灵敏度设置：
![Sentivity](/img/post/faq4_zh.jpg)

## 4.The number of jump ropes counted is higher or lower than the actual number
If the error exceeds 50%.
#####  1. Please reboot the watch.
#####  2. Please check if the "double under" mode is checked, in double under mode, single shake will be reduced by half.

The normal error is in the 1% to 5% range, adjust the sensitivity if the error is smaller than the actual times.
Turn up the sensitivity data.  Otherwise, turn it down. 

Sensitivity settings.
![Sentivity](/img/post/faq4_en.jpg)

## 5. 卡路里数据不准？（同样跳1000次，卡路里数不一样）
YaoYao 的卡路里数据是由 watchOS 计算得出，根据心率、运动传感器强度、时间、运动类型、年龄等计算的，并不是完全线性的结果，正常在一小时消息总卡路里600卡左右。
其次动态卡路里指的是运动消耗的卡路里，不包含静态消耗。

__总消耗卡路里 = 动态消耗路里+ 静息基础消耗__

YaoYao 默认显示的是动态卡路里，详情里有总卡路里显示。
表带未戴紧会严重影响心率和卡路里数据的准确性，建议戴紧一点，心率对卡路里影响最大！

## 5. Calorie data is inaccurate? 
Firstly, YaoYao's calorie data is calculated by watchOS, based on heart rate, exercise sensor strength, time, type of exercise, age, etc., and is not a completely linear result, normally around 600 total calories in an hour.
Secondly, dynamic calories refer to calories burned during exercise and do not include base energy burned.

__Total Calories  burned = Active Calories Burned + Base Energy Burned__

YaoYao shows active calories by default, and total calories are shown in the workout details.
Not wearing the strap tightly can seriously affect the accuracy of the heart rate and calorie data, and it is recommended that you wear it tighter, since heart rate has the most impact on calories!

## 6. App 健康隐私授权(手机上没有跳绳记录原因之一)
#####  手机上
 ![health privacy](/img/post/faq6_1_cn.jpg)

#####  手表上
App 首次运行会要求对健康隐私授权，以用来读取或写入各类运动数据，如果被忽略可以手动打开
![health privacy](/img/post/faq6_cn.jpg)

## 6.App Health Privacy Authorization (one of the reasons why workout data is not recorded on the phone)
#####  on the watch
The first run of the App will require a health privacy authorization to read or write various types of motion data, which can be opened manually if ignored.
#####  Check on the iPhone
![health privacy](/img/post/faq6_1_en.jpg)
#####  Check on the Watch
![health privacy](/img/post/faq6_en.jpg)