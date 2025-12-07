---
layout: post
title: watchOS 开发点滴
date: 2025-12-07
categories: blog
tags: [Coding]
description: watchOS dev tips
---

作为 [YaoYao 跳绳](https://apps.apple.com/us/app/yaoyao-jump-rope/id1179393901)、[Tooboo（徒步骑行](https://apps.apple.com/us/app/tooboo-hiking-trail-guides/id6736378337)）、[DunDun（深蹲）](https://apps.apple.com/us/app/dundun-squat-counter/id1348285355)的开发者，我开发的这些运动健身类产品，基本是以 Apple Watch app 为核心产品形态，而不是 iPhone app，以下我这些年来一些 watchOS 零零散散的开发经验，也希望对你有用。

# watch app 和 iOS app 协同问题

如果 watch app 和 iOS app 需要协同，手机和手表系统版本不一致，或两边 app 版本不一致都有可能导致很多问题。

## watchOS 和 iOS 版本不一致的问题

比如用户的 iOS 版本是 26.1，watchOS 版本是 26.0，这个可能会导致一系列问题：

* 手表端 app 无法安装：
  新用户在安装手机 app 后，手表 app 未能同步自动安装

* Watch 端 HKWorkoutSession 结束后，手机端“健身” app 中看不到数据，健身圆环无法填充。
  如果是健身 app，非常依赖 HealthKit 存储，这个问题相当普遍。

* Watch Connectivity 通信失败，手表和手机 app 无法使用 WCSession 通信。&#x20;

## watch app 和 iOS app 版本不一致的问题

比如用户的 iOS app 是 1.1，watch app 是 1.0，

* Watch Connectivity 通信可能失败，WCSession 相关方法会失效。&#x20;

# watch app 和 iOS app 互相唤起

## watch app 唤起 iOS app

使用 watch 端  WCSession sendMessage 给 iOS app 发消息可直接唤起 iOS app。

如果 iOS app 未启动，会直接被拉起。否则 iOS app 会在后台运行。
这是一个极其强大的功能，比如 watchOS 并不支持 SFSpeechRecognizer，[甚至可以把语音流发到手机，在手机上完成语音转写，间接实现手表上的实时语音转写效果。](https://x.com/haozes/status/1710624500973519325)

YaoYao 和 Tooboo 也是使用这种方法，发送需要语音合成文本给手机 app ，然后手机 app 使用语音合成再在手机上播报语音，这样用户可以在运动时一边在手机上听音乐，一边听运动语音提醒。

## iOS app 唤起 watch app

可使用 HKHealthStore 的 startWatchApp(with:) 唤起 watch app

```javascript
  class func launchEmptyWatchApp(onComplete: ((Bool,Error?) -> Void)? = nil){
        let workoutConfiguration = HKWorkoutConfiguration()
        workoutConfiguration.activityType = .other
        workoutConfiguration.locationType = .outdoor
       
        let healthStore = HKHealthStore()
        healthStore.startWatchApp(with: workoutConfiguration) { (success, error) in
            print(">>> startWatchApp,launchEmptyWatchApp: \(success),error:\(String(describing: error))")
            onComplete?(success,error)
        }
    }
```

在 watch 端：

```plain&#x20;text
 func handle(_ workoutConfiguration: HKWorkoutConfiguration) {
      // 此处不一定非要启动 HKWorkoutSession
 }
```

注：该方法仅可在 app category 为 Healthcare & Fitness 才可用，否则审核很可能驳回。

# watch app 和 iOS app 数据同步

| App Groups         | X | watchOS 从 2.0 开始已经不支持这种方式                                                                                               |
| ------------------ | - | ----------------------------------------------------------------------------------------------------------------------- |
| CloudKit           | ✓ | 支持类似 SwiftData 和老的 CloudKit 相关功能                                                                                        |
| iCloud Document    | X | watchOS 并不支持 iCloud Ubiquitous Containers                                                                               |
| iCloud key-value   | ✓ | 支持 NSUbiquitousKeyValueStore，并较为推荐                                                                                      |
| Watch Connectivity | ✓ | 使用 WCSession 发送消息或文件是相对可靠稳定即时的方法同步两边的数据，较为推荐。更多参考：https://developer.apple.com/documentation/watchconnectivity/wcsession |

注：

* WCSession  transferFile 发送文件方法在模拟器不支持，需要真机测试

# 异常重启与恢复

## 修改 iPhone app 配置会导致 watch app 立即重启

在 iPhone 端 app 修改任意隐私权限，如通知，位置，健康隐私权限会直接导致手表端 app 立即被 SIGKILL。

这个问题在一般情况下问题不大，但当你的 watch app 使用 Watch Connectivity 和手机实时通信时，用户在手机 app 上操作，正好碰到需要授权时，watch app 会被杀掉，会非常影响体验。

## 运动会话 watch app 的异常重启与恢复

使用 HKWorkoutSession 的 app，在运动过程中异常崩溃了，可以按照 Apple 的文档通过 WKExtensionDelegate 的 handleActiveWorkoutRecovery 方法重启运动会话，

1. &#x20;handleActiveWorkoutRecovery 方法不会在重启手表时被调用。
   但比如像 Tooboo 这样的应用，用户可能一直用到没电，在充满电后，再打开手表，无法确保这个方法被调用。

2. 建议在 applicationDidFinishLaunching 中 检测 HKHealthStore().recoverActiveWorkoutSession

3. recoverActiveWorkoutSession 仅能恢复HKWorkoutSession 定义的那些一般数据(时间、距离、卡路里等）app 自己定义统计的指标需要自己备份和还原。

# 内存泄露问题

无论是 iOS / watchOS app 在关闭时只是 freeze 而不是真正 restart，这会导致内存泄露问题非常隐蔽。会累积使用很多次后，才会导致 app 因内存使用过大被 watchdog kill。

#### watchOS 上TabView 嵌套会导致内存泄露！

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        TabView {
            TabView {
                Text("Page V1")
                Text("Page V1")
            }.tabViewStyle(.verticalPage)
            Text("Page H2")
        }
    }
}
```

在 watch app 可能你需要类似的架构布局，但目前会导致内存泄露，需要避免嵌套使用 TabView。

![](/img/post/251207/image.png)

# 电量 优化

使用 HKWorkoutSession 的 app 可以获得相对其他 app 无法拥有的权限：后台运行。同时 app 的责任也更大，如果 UI 有高频刷新的动画、甚至普通数据驱动的 UI 刷新长时间运行都会消耗大量电量。

#### 主要优化思路

1. isLuminanceReduced 为 true 时减少刷新

用户在使用手表的时候，大部分时间手腕是放下的，不可能一直抬着手。这时候系统会将屏幕变暗，也就是 isLuminanceReduced 值会变为 true，仅在抬腕时高频刷新，其他时候尽可能降低刷新频率。

* App 不在前台时 减少刷新
  监听 NotificationCenter.default.publisher(for: WKApplication.willResignActiveNotification)，当 app 不在 active 状态时减少刷新。

#### &#xA;Tooboo 的性能优化

在 Tooboo 这种徒步场景下，用户可以连续导航 12 小时(GPS、心率传感始终工作)，低电量模式下可以使用 16 小时（Apple Watch Ultra 2 测试）。

![](/img/post/251207/image-1.png)

Tooboo watch app 主要有3个线程， 地图的渲染， UI 界面的主线程，导航算法。&#x20;

每次 GPS 的刷新会触发导航算法的判断，是否偏离路线或者要不要转弯，但这时候地图不一定会渲染，地图的磁贴也不会下载，地图的下载渲染仅在用户的抬腕时，当在后台或者亮度降低时高昂的 UI 操作都会是非常低频的。

在 UI 界面渲染方法，Tooboo 并没有使用 SwiftUI 的默认的数据驱动渲染方式，因为在运动过程中，运动的数据指标很多（心率、卡路里、距离、速度、位置等等），任意一个指标变化都会导致 UI 重绘，这个刷新频率会在每秒有几次。&#x20;

Tooboo 使用了 TimelineSchedule 定时刷新的方式，如果抬腕亮屏将 Schedule 频率调高 到 1hz，否则降低到 10-20 秒 刷新一次，这样也会大大减少 CPU 的消耗。       &#x20;


```swift
public struct MetricsTimelineSchedule: TimelineSchedule {
    var startDate: Date
    var isPaused: Bool
    var lowInterval:Double

    public init(from startDate: Date, isPaused: Bool,lowInterval:Double? = nil) {
        self.startDate = startDate
        self.isPaused = isPaused
        self.lowInterval = lowInterval ?? Double(AppSetting.shared.lowRefreshInterval)
    }

    public func entries(from startDate: Date, mode: TimelineScheduleMode) -> AnyIterator<Date> {
        var baseSchedule = PeriodicTimelineSchedule(from: self.startDate, by: (mode == .lowFrequency ? self.lowInterval : 1.0))
            .entries(from: startDate, mode: mode)
        
        return AnyIterator<Date> {
            guard !isPaused else { return nil }
            //print("MetricsTimelineSchedule next()")
            return baseSchedule.next()
        }
    }
}
```

```swift
struct Metric:View {
    @EnvironmentObject var viewModel:WorkoutViewModel
    @Environment(\.isLuminanceReduced) var isLuminanceReduced
    var body: some View {
        TimelineView(MetricsTimelineSchedule(from: self.viewModel.workoutData.startDate ?? Date(), isPaused: (self.viewModel.workoutState == .paused || isLuminanceReduced == true))) { timeline in
            ZStack(alignment:.topLeading){
                
                VStack(alignment: .leading,spacing: 0){
                    Spacer()
                    Text(viewModel.elapsedSec.shortFormatted).foregroundStyle(Color.yellow)
                           // other metric data
                    Spacer()
                }
                .frame(maxWidth: .infinity, alignment: .leading)
                .ignoresSafeArea(edges: .bottom)
                .scenePadding()
            }
        }
        
    }
}
```

比如这里在 app 运动暂停或 isLuminanceReduced 为 true 时，app 是低频刷新的。

# 小结

* &#x20;Apple 从 watchOS 6 时支持了 SwiftUI 来开发 watch app 界面，watchOS 已经没有任何理由使用之前类似于 UIKit 的 watchkit 方式来开发。

如果你是新产品，建议从 watchOS 9+ 开始支持。

* 相对 iOS app 的调试，watchOS 的 app 调试相对困难，另外使用 Xcode > Window > Organizer > Crash 收集到的 crash 报告会是 iOS 收到的日志的 1/10-1/20 。

Watch app 最好配合使用 app 本地写日志的方式记录问题，通过 WCSession 将日志发送到手机再收集。

* 在产品的部署方面，由于大量用户并不熟悉 watch 的 app 的安装方式，加上系统版本不一致可能导致 app 不能安装，致使这类产品在第一步就会遇到困难。

让用户方便联系上开发者，提供支持非常有必要。

***

[Haozes](https://www.yaoyaojumprope.com/about):  是 [YaoYao跳绳](https://apps.apple.com/us/app/yaoyao-jump-rope/id1179393901)、[Tooboo（徒步骑行](https://apps.apple.com/us/app/tooboo-hiking-trail-guides/id6736378337)）、[DunDun（深蹲）](https://apps.apple.com/us/app/dundun-squat-counter/id1348285355) 等健康健身类 app 的作者，致力于用身体去抵抗这个时代的丧。

如果你是热爱户外，热爱 Apple Watch 这类的健康可穿戴产品的设计师，欢迎交流。

