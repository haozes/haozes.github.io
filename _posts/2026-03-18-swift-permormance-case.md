---
layout: post
title: 一次很典型的 SwiftUI 性能坑
date: 2026-03-18
categories: blog
tags: [Coding]
description: swiftui dev tips
---

# 一次很典型的 SwiftUI 性能坑

在 Tooboo 实际 SwiftUI页面里，运动会话页静止不动时，只有计时数字在变化，但 CPU 仍然有 `60%~70%`。优化后降到了大约 `10%`。问题的根源是：父视图读了高频变化的 observable 属性，导致整页都跟着失效，连地图这种重组件也被拖着每秒重算。

## 先说结论

SwiftUI Observation 下有一句很重要的话：

> 谁在 `body` 里读取 observable 属性，谁就会订阅它。

所以性能优化的关键原则是：

- 高频变化的数据，不要在大容器视图里读
- 谁需要，谁在最小子树里读
- 地图、列表、图表这类重组件，只依赖自己真正关心的状态

## 错误写法：父视图提前读取高频状态

下面这个写法非常常见，也非常容易出问题：

```swift
import SwiftUI
import Observation
import CoreLocation

struct WorkoutData {
    var elapsedSec: Int = 0
    var nearIdx: Int? = nil
    var state: WorkoutState = .running
}

enum WorkoutState {
    case running
    case paused
}

@Observable
final class SessionViewModel {
    var workoutData = WorkoutData()
    var followingPoints: [CLLocation] = []
}

struct WorkoutSessionView: View {
    let viewModel: SessionViewModel

    var body: some View {
        ZStack {
            MapView(
                followingPoints: viewModel.followingPoints,
                routeGuideNearestIndex: viewModel.workoutData.nearIdx
            )

            DataPanelContainer(
                workoutData: viewModel.workoutData,
                workoutState: viewModel.workoutData.state
            )
        }
    }
}
```

看起来只是把数据传给子视图，但真正的问题是：

- `WorkoutSessionView.body` 里读了 `viewModel.workoutData`
- `elapsedSec` 每秒变化一次
- 于是整个 `WorkoutSessionView` 每秒重算一次
- `MapView` 也被迫重新求值
- 地图内部的 annotation 过滤、可见区域计算、路径处理也都被重新执行

这就是“明明只有计时器在变，为什么地图也在刷”的原因。

## 为什么这很贵

很多人以为 SwiftUI 重算 `body` 只是“轻量声明式更新”。这话只说对了一半。

如果你的子树里有这些东西，重算就不轻了：

- 地图标注过滤
- 路线点聚合
- 图表数据映射
- 图片查找
- 几何计算
- `onChange` / `onReceive` 里的附带逻辑

也就是说，问题不只是“视图重建”，而是“重建触发了一整条昂贵计算链”。

## 优化写法：把订阅边界收缩到最小子树

正确做法不是把所有数据都拆碎，而是让父视图只读地图真正需要的状态。

```swift
import SwiftUI
import Observation
import CoreLocation

struct WorkoutData {
    var elapsedSec: Int = 0
    var state: WorkoutState = .running
}

enum WorkoutState {
    case running
    case paused
}

@Observable
final class SessionViewModel {
    var workoutData = WorkoutData()
    var followingPoints: [CLLocation] = []
    var routeGuideNearestIndex: Int? = nil
}

struct WorkoutSessionView: View {
    let viewModel: SessionViewModel

    var body: some View {
        ZStack {
            MapView(
                followingPoints: viewModel.followingPoints,
                routeGuideNearestIndex: viewModel.routeGuideNearestIndex
            )

            DataPanelContainer(viewModel: viewModel)
        }
    }
}

struct DataPanelContainer: View {
    let viewModel: SessionViewModel

    var body: some View {
        DataPanelView(
            workoutData: viewModel.workoutData,
            workoutState: viewModel.workoutData.state
        )
    }
}
```

这时订阅关系变成了：

- `WorkoutSessionView` 只订阅 `followingPoints` 和 `routeGuideNearestIndex`
- `DataPanelContainer` 才订阅 `workoutData`
- `elapsedSec` 每秒变化时，只刷新面板，不刷新地图

这就是这次优化真正生效的原因。

## 很多人会困惑的一点

“我把 `workoutData` 改成 `viewModel` 传给子视图，子视图不还是会失效吗？”

对，子视图还是会失效。

但这正是我们想要的结果。

重点不是“完全不要失效”，而是“只让需要更新的那一小块失效”。计时面板每秒刷新是合理的，地图每秒跟着刷新才是不合理的。

## 第二个关键点：把高频字段和低频字段解耦

还有一个容易忽略的坑：

```swift
MapView(
    followingPoints: viewModel.followingPoints,
    routeGuideNearestIndex: viewModel.workoutData.nearIdx
)
```

如果地图依赖的导航索引也藏在 `workoutData` 里，那父视图为了拿这个值，依然会订阅整个 `workoutData`。

更稳妥的做法是把它单独拆出来：

```swift
@Observable
final class SessionViewModel {
    var workoutData = WorkoutData()
    var routeGuideNearestIndex: Int? = nil
}
```

这样地图依赖的导航状态，就和每秒变化的计时、心率、卡路里脱钩了。

## 用一句话理解这次优化

旧写法：

- 父视图先读了 `viewModel.workoutData`
- 所以父视图订阅了高频变化状态
- `elapsedSec` 每秒变化
- 整个页面跟着重算

新写法：

- 父视图不再读 `viewModel.workoutData`
- 只有面板子视图在读
- `elapsedSec` 每秒变化
- 只有面板子树重算，地图保持稳定

## 一个更完整的最小 Demo

下面这个 Demo 能更直观说明问题。

### 优化前

```swift
import SwiftUI
import Observation

@Observable
final class DemoViewModel {
    var elapsedSec: Int = 0
    var points: [Int] = Array(0..<10_000)
}

struct BadDashboardView: View {
    let viewModel: DemoViewModel

    var body: some View {
        VStack {
            ExpensiveChart(points: viewModel.points)
            Text("Time: \(viewModel.elapsedSec)")
        }
    }
}
```

这里 `BadDashboardView` 同时读取了：

- `points`
- `elapsedSec`

只要 `elapsedSec` 每秒变化一次，整个 `VStack` 都会重新计算，`ExpensiveChart` 也会被拖进去。

### 优化后

```swift
import SwiftUI
import Observation

@Observable
final class DemoViewModel {
    var elapsedSec: Int = 0
    var points: [Int] = Array(0..<10_000)
}

struct GoodDashboardView: View {
    let viewModel: DemoViewModel

    var body: some View {
        VStack {
            ExpensiveChart(points: viewModel.points)
            TimerText(viewModel: viewModel)
        }
    }
}

struct TimerText: View {
    let viewModel: DemoViewModel

    var body: some View {
        Text("Time: \(viewModel.elapsedSec)")
    }
}
```

这次变化很小，但效果很大：

- `GoodDashboardView` 不再读取 `elapsedSec`
- 计时变化时，只重算 `TimerText`
- `ExpensiveChart` 不会每秒跟着刷新

## 可以直接复用的判断标准

遇到 SwiftUI 页面性能异常时，先问自己这几个问题：

- 父视图是不是读了高频变化的 `@Observable` 属性？
- 地图、图表、列表这些重组件，是不是被放在这个父视图下面？
- 子视图真正需要的只是一个字段，我是不是却把整个大对象传上去先读了一遍？
- 有没有“为了方便取值”，把多个更新频率完全不同的字段塞进同一个 observable 对象里？

只要这几个问题里有两个答“是”，大概率就已经踩坑了。

## 一条最好记的经验

不要让高频变化的数据穿过大容器视图。

换句话说：

> 谁显示，谁读取；谁不关心，谁就不要订阅。

这类优化通常不需要改算法，不需要上多线程，也不需要做复杂缓存。很多时候，单纯把 Observation 的依赖边界切对，性能就会立刻下来。

## 结尾

这次案例非常典型，因为它看上去像“地图性能差”，但真正的问题并不在地图本身，而在 SwiftUI 的状态订阅范围。

如果你的页面里有这些组合，就值得优先检查一遍：

- `@Observable`
- 高频变化字段
- 大容器视图
- 地图、图表、列表等重子树

很多性能问题，最终不是算法问题，而是依赖关系问题。
