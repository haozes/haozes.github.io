---
layout: post
title: watchOS 上的一次 SKView 内存泄露调试
date: 2020-05-15
categories: blog
tags: [Coding]
description: 
---

前几个版本，在 YaoYao watch 端加了点 SpriteKit 的动画优化了一下效果，后来被证明是一次失败的负优化。 

### 现象
- 用户反馈YaoYao HIIT模式下偶尔程序闪退，线上并未收集到 crash Log
- 闪退的过程是在使用了 10几组 HIIT 的时候（过了一段时间）

### 调试过程
使用模拟器测试了半天，并没有内存泄露和崩溃。这让人很头疼，我又用真机开始调试，用instrument看前面 30组也并无内存泄露，直到我将手表戴在手上，后来终于出现了奇迹，内存开始涨,最终crash。

```
SKView: ignoreRenderSyncInLayoutSubviews is NO. Call _renderSynchronouslyForTime without handler
2020-05-14 22:33:13.440151+0800 YaoYao WatchKit Extension[249:11942] Execution of the command buffer was aborted due to an error during execution. Insufficient Permission (to submit GPU work from background) (IOAF code 6)
2020-05-14 22:33:13.440474+0800 YaoYao WatchKit Extension[249:11942] Execution of the command buffer was aborted due to an error during execution. Insufficient Permission (to submit GPU work from background) (IOAF code 6)
2020-05-14 22:33:13.440660+0800 YaoYao WatchKit Extension[249:11942] Execution of the command buffer was aborted due to an error during execution. Insufficient Permission (to submit GPU work from background) (IOAF code 6)
2020-05-14 22:33:13.440806+0800 YaoYao WatchKit Extension[249:11942] Execution of the command buffer was aborted due to an error during execution. Insufficient Permission (to submit GPU work from background) (IOAF code 6)

Thread 1: EXC_RESOURCE RESOURCE_TYPE_MEMORY (limit=50 MB, unused=0x0)
```

### 结论
1. watchOS 的内存上限，官方文档没有明确说明，事实上在 watchOS6, 我的 watch 4代上表现结果是 50MB
2. SKView 的这个内存泄露发生在 app 不在frontmost 的时候，手表熄屏后才发生,所以模拟器并未能测试出来
3. watchOS app 的超内存好像没有crash report，虽然我在真机上重现了，并也并未找到report，所以线上也未能收集到报告。