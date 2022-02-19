---
layout: post
title: 物尽其用，让 AirPods 也能跳绳计数
date: 2022-02-18
categories: blog
tags: [Coding]
description: YaoYao AirPods Mode
---
![Banner](/img/post/0218/banner_pod.jpg)

YaoYao 是 一款 [Apple Watch/Galaxy Watch 跳绳计数应用](https://yy.onlytalk.top/)，跳上了就停不下来，是它让少男少女提前拄上了拐，是他让中年大叔大婶成为广场最靓的仔。

如果你是 Watch 用户，因为花了 18块买了 YaoYao App，省了一根 200元的智能跳绳。那么，现在 AirPods 用户也能省下这 200了。 

过年的时候，中年程序猿失业摆摊标杆，被光没选中之人，跳绳 App 终结者汪二，在村头刷手机，发现 Apple 给 AirPods 提供了一个接口，可以获取类似手表上的运动传感器数据，AirPods 正是用这个传感器实现空间音频效果的。
![airpods+rope](/img/post/0218/banner_air.jpg)

汪二一拍大腿：这个传感器也可以用来实现跳绳计数嘛！赶紧用热水带给冻得充不上电的 Macbook 焐上，哆嗦着开始开发这个功能。


### 使用体验部分甚至超过了 Apple Watch 
![连接 AirPods](/img/post/0218/airconn.jpg)


点击首页左上角耳机图标，进入 「AirPods 模式」， AirPods 模式和手表 App 一样加入了各种运动模式：计数、计时、以及大家喜欢的 HIIT间隙跳模式。  
- 如果耳机是支持空间音频的耳机，会显示已连接。  
- 如果无法连接，YaoYao 替你验证了，该耳机来自华强北🐶。

![AirPods 模式运动中](/img/post/0218/airpodjump.gif)

🎵  首先 YaoYao 语音报数方面等同于 Watch App 上的交互体验，但戴上耳机听歌跳绳的体验，甚至超过 Apple Watch。Apple Watch 上听音乐基本没好用的，大多情况我还是用手机上的音乐App。  

其次你再也没有 Apple Watch 电量担忧。  
Apple Watch 用两三年后，到了晚上总是电量捉急，而很多人都在晚上跳绳跑步。

### 不足的地方
- AirPods 没有心率传感器，无法提供心率信息。下图前两张为 Apple Watch 版生成记录，后一张为使用 AirPods 生成的数据图。
![后者无心率图表](/img/post/0218/hr_compare.jpg)
- 需要一直打开手机 App 亮着屏   
AirPods 仅提供了传感器数据，实际数据计算部分还是在手机 App 上完成的，也就是你无法像 Apple Watch  一样，只戴一个 Watch 下楼跳绳。


### 🎧 软硬件要求
#### 当前仅 AirPods Pro、AirPods Max、AirPods（第 3 代）、Beats Fit Pro 支持   

也就是只有支持空间音频的 AirPods 才有这种运动传感器，其他第三方耳机不支持。
请在以下文档查找是否你的耳机是否支持：
https://support.apple.com/zh-cn/HT211775

如果你是 AirPods 1/2 代，又想试试这个功能。咱家又不是没这条件！压岁钱拿出来，换新款！

#### 📲  系统需要 iOS 15
开发者也是人，也要工作体验，适配那么多版本，新增功能加量不收费，如果升个级，你都嫌烦，不如相互放弃🐶。


### 下载
App Store 下载 YaoYao, 或更新 App 到最新版，即可体验该功能。
