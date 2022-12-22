---
layout: post
title: 将YaoYao跳绳记录同步到三星健康（android）
date: 2022-12-22
categories: blog
tags: [Coding]
description: sync YaoYao workout record to Samsung Health
---
![Banner](/img/post/221222//banner.jpg)
三星健康有一套类似于健康圆环一样的激励机制。一直有用户提--希望可以将YaoYao跳绳数据同步到三星健康中，但三星健康并不开放给一般的厂商集成。YaoYao 在 Android 上之前集成了 Google Fit ，但想用YaoYao的健身数据填充三星的健康圆环依然不行。  

只到 Google 今年发布了[Health Connect](https://developer.android.com/guide/health-and-fitness/health-connect?hl=zh-cn)，Health Connect 旨在像 Apple HealthKit一样，打通各种健康平台。理想情况下，只要第三方 App，写数据到 Health Connect后，像三星健康，Google Fit，也会从 Health Connect 中读取数据，而达到统一数据接口的目标。

目前 YaoYao （Google Play版本）已经集成了 Health Connect。

### 使用方法（海外版）
以下假设你熟练掌握如何使用 Google Play 下载软件，并且使用海外版本的三星健康。
#### Step 1 安装Health Connect
Google Play 搜索：Health Connect

#### Step 2 在三星健康设置中启用 Health Connect
![samsung health setting](/img/post/221222/samsung_health_setting.jpg)

#### Step 3 在 YaoYao 设置中启用Health Connect
![YaoYao setting](/img/post/221222/yaoyao_setting.jpg)

#### Step 4 使用手表产生一条YaoYao的跳绳数据，打开YaoYao App，即可同步
![Sync to Sumsung Health](/img/post/221222/sync_finish.jpg)

##### PS:三星健康同步延迟问题
![Sync Status](/img/post/221222/sync_status.jpg)
打开 Health Connect 可以看到，YaoYao 什么时候写入了健身数据，其他 App 又读取了数据。测试发现，Google Fit 打开即能读取到YaoYao写入的数据，但三星健康似乎有个定时的机制去读取，有时是 5分钟，有时是几十分钟，并不能立即展现出来。

### 国内如何使用
Health Connect 在国行版本的三星健康上未开放（找不到上面第 2步那个选项），我咨询了三星的开发团队，他们也没有计划什么时候开放。以下是我用一个Hack 方法开启了这个选项。

##### Step 1 从淘宝上买一张出国旅游用的SIM卡
一般20元包邮，收到不需要用开通，有没有流量无所谓，卡尸体都行。
##### Step 2 从Google Play 下载三星健康，使用该卡即可跳过App国家验证。
插卡后，登录三星健康即可（如果你没有这张外国卡，会提示里不支持该国家。用同样的方法，可以在国内玩Tiktok）
##### Step 3 在三星健康设置中，你将可以看到Health Connect选项，成功！

