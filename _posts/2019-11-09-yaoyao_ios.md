---
layout: post
title: YaoYao iPhone 臂包版以及 小米手表是否值得一试
date: 2019-10-12
categories: blog
tags: [Coding]
description: YaoYao-跳绳 iPhone 臂包版
---

YaoYao Apple Watch版(https://apps.apple.com/cn/app/id1179393901)可能是目前最好用的跳绳计数 App，他有计时、计数、HIIT各种模式，也有实时报时、报数。相对于市面上的智能跳绳，无法记录心率、卡路里多半是瞎算的，YaoYao 使用 Apple Watch 提供的心率和卡路里数据也更为准确。(实际上卡路里很难真实计算准确，虽然 Apple 说它根据心率，传感器运动强度等复杂计算，我看也只能作为一个参考)

#### YaoYao微信小程序版以及其局限
但很多人没有Apple Watch，所以上个月，我写了一个微信小程序版的 YaoYao(https://haozes.me/blog/2019/10/12/yaoyao_mini/), 配戴一个手机臂包，像跑步一样戴着跳绳，经过我自己测试，使用体验并不差，远比放在裤子口袋舒服。但微信版的有个局限，当微信切到后台，屏幕手头锁屏后，程序就不能计算了。

Apple Watch 专门为运动模式，开了后门，有个专门的运动会话模式，无论 App 在前台，后台，App 可以一直运行，而且是最高优先级的，在微信上做不到这一点，所以我打算做个免费版的 App 版。


#### YaoYao 臂包版
YaoYao 此次的臂包版 App，用了一个 workaround，类似于导航软件，使用位置实时更新的方法后，app 终于可以实现类似于 WatchOS的运动会话模式，而在审核的时候，我也和Apple 的审核反复交涉了几次，最后审核还是放过了。所以不要放弃，如果遇到被拒，据理力争还是有用的，以下是我的申诉内容，为了怕审核人员看不懂中文，我将内容写了一份中文，一份英文的内容：
```
  
YaoYao is a jump rope repetition counting application that compute the real-time sensor data from CoreMotion. Our users need the app can counting whether app in the foreground or in background. Just like our Apple Watch version of YoaYao - Jump Rope App (https://apps.apple.com/cn/app/id1179393901)

Unfortunately, iOS does not have a workout session mode similar to WatchOS. When the App accidentally switches to the background or lock screen, it cannot compute the sensor data, which is obviously not what the user needs.

Currently we approach the workout session mode with features like navigation app. So we only need to use real-time update location information during the use of the app, which users need.

So we need UIBackgroundModes location support, please consider this nicely.

If you have other questions, you can call me.


YaoYao 是一个跳绳计数应用，它通过实时计算 CoreMotion 传感器数据来实现跳跃的计数，我们的用户需要 App 无论在前台或后台都可以使用计数功能，就像我们的另一款Apple Watch 产品YaoYao watch版（https://apps.apple.com/cn/app/id1179393901）一样。很遗憾，iOS没有类似WatchOS 的 workout session 模式，当 App 不小心切换到后台或者锁屏，就无法计数传感器数据，这显然不是用户需要的。
目前我们通过类似导航软件一样的特性能来接近workout session 模式。所以我们仅需要在App 使用期间使用实时更新位置信息， 用户很需要这个特性。
所以，请考虑允许我们使用 UIBackgroundModes 的位置更新特性。

如果有其他问题，可以打电话给我。  

```

#### 关于小米手表和 WearOS
11/05，小米发布了一款搭载 WearOS 版的手表，我觉得还是一个很好的起点，我不太看好那种专门运动的手表，毕竟大部分国人，并不是很热衷于运动，就算是运动也只是生活的一部分，场景只是一天的少量时间。

WearOS 一直没有一个能打的手表，小米手表可能只是个开头，虽然抄袭严重，但至少是往正确的方式抄。比如：大多 Android 手表都是圆形的，这可能是一种思维定势或不敢挑战传统设计，传统手表是圆的，但对于数字设备来说，使用一个圆形界面，是很别扭的，给 App 的设计开发人员，都会带来很多痛苦，想想吧，现实世界的图片都是矩形的，在圆形显示器怎么好展示。

我订了一块小米的手表，还没拿到手，我对它也不抱有多大期待，第一代产品，一定也问题多多。我会试试在上面开发 YaoYao。

##### 为什么我认为 WearOS 值得一试
Apple 的很多产品（Mac、iPhone、Apple Wath）依然是 Best Product，但历史也一遍遍的在重演，从 Mac和PC，iPhone 与 Android 的竞争,到 Apple Watch 和WearOS，WearOS 可能还是一样会成为不是最好，却会是最多人用的手表系统。

为什么我同样觉得开发人员应该尝试智能手表呢，因为同样的历史也一遍遍在重演，从PC到智能手机，从智能手机到手表，屏幕越来越小，使用越来越方便，随着语音和芯片计算能力提升，可穿戴设备更多代替手机这是必然趋势。我们这代程序猿先是感觉智能手机从只是一个玩具不可能代替PC，到眼看着PC流量一点一点跌到现在这样的。


#### 最后欢迎试用YaoYao臂包版
![screenshot](cdn.onlytalk.top/blog/20191109101250-screen.jpg)

下载地址：https://apps.apple.com/cn/app/id1485959492



