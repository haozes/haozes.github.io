---
layout: post
title: 将YaoYao跳绳记录同步到三星健康（android）
date: 2022-12-22
categories: blog
tags: [Coding]
description: sync YaoYao workout record to Samsung Health
---
![Banner](/img/post/221222//banner.jpg)
Samsung Health has an incentive mechanism similar to the Apple Watch Activity Rings which offers suggestions and encouragement to help people close it.

Some users have been asking -- hoping to synchronize YaoYao jump rope data to Samsung Health, but Samsung Health is not open to general manufacturer. YaoYao on Android integrated with Google Fit, but want to use YaoYao workout data to fill the Samsung health circle still not possible before Health Connect integrated.

Only until Google released [Health Connect](https://developer.android.com/guide/health-and-fitness/health-connect?hl=zh-cn) this year, Health Connect aims to connect various health platforms. Ideally, as long as third-party apps write data to Health Connect, like Samsung Health and Google Fit, they will also read data from Health Connect to achieve the goal.

Currently YaoYao (Google Play version) has integrated Health Connect.

### How to Use 

#### Step 1 Install Health Connect
Google Play search: Health Connect

####  Step 2 Enable Health Connect in Samsung Health settings
![samsung health setting](/img/post/221222/samsung_health_setting.jpg)

####  Step 3 Enable Health Connect in YaoYao settings
![YaoYao setting](/img/post/221222/yaoyao_setting.jpg)

####  Step 4 Use YaoYao watch app to generate a jump rope data of YaoYao, and open the YaoYao App to synchronize
![Sync to Sumsung Health](/img/post/221222/sync_finish.jpg)

#### PS: Samsung health synchronization delay problem
![Sync Status](/img/post/221222/sync_status.jpg)
Open Health Connect to see when YaoYao has written workout data and other apps have read the data. Testing found that Google Fit can read the data written by YaoYao when it is opened, but Samsung Health seems to have a timing mechanism to read it, sometimes it is 5 minutes, sometimes it is several minutes, and it does not show up immediately.
