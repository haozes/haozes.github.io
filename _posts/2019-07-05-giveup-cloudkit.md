
---
layout: post
title: 为何 OnlyTalk 放弃了使用 Cloudkit 服务
date: 2019-07-05
categories: blog
tags: [Coding]
description: Why OnlyTalk give up cloudkit
---

### 向死而生
这是一个注定是广义上”失败“的 [App](https://itunes.apple.com/cn/app/id1462516460?mt=8), IM 太多公司尝试了又失败。我本计划 OnlyTalk 的这样的小玩意，只要耗一个月时间做个 MVP 版本， 如果我上来就需要花掉我 3 个月时间，以我的鸡贼一定不会动手。生活充满了“死”字，就像砍“只狼”一样。
![death](https://pic2.zhimg.com/80/v2-d1be45ca7b4bd3e613ce2eaca944846d_hd.jpg)

### OnlyTalk 的开始架构设计
![onlytalk design](http://cdn.onlytalk.top/onlytalk_server.png)
因为穷 ，OnlyTalk 在设计的时候使用 Cloudkit 来作为后端 Baas ，用户基本资料、好友消息、消息历史，音频文件都存在 Cloudkit 上。

#### 理想化的最小化的 IM 架构 
由于 Pushkit 的推送相当实时，而且稳定，并且 Pushkit 推送可以后台运行，所以我并没有写一个聊天服务（OnlyTalk 的视频聊天服务是一个 WebRTC  服务，参考 [https://github.com/kurzdigital/Whale](https://github.com/kurzdigital/Whale)），OnlyTalk 的 Web 服务相当简单，当用户的消息发到服务端，服务端只是把消息转发给 Pushkit apns server，进行推送。

如果按照这个架构，那么我只有很少的服务端开发工作量，我只要把精力花在客户端上，关键是存储都在 Cloudkit 上，我可以省去很多流量的费用。那么作为一个小团队项目，我们只要支付很便宜的 ECS Web 服务器的费用，而这一 Web 服务功能很简单，使用 Node 开发，也能扛住比较大的流量，所以这个项目失败了代价只是一个月的时间。

___说干就干！___

### 第一次”死“：使用 Cloudkit 存储音频文件问题
OnlyTalk 编码是 8K mono 的音频，这种文件已经很小了，一般几十KB到几百KB， Cloudkit 的慢的惊人，这么小的文件用户上传下载都可能要 5s 以上，就算没有CKAsset，普通查询你依然能感受到转圈，而无论你怎么调整CKQuery 执行的优先级。
```swift
 fetchOperation.qualityOfService = .userInteractive
 fetchOperation.queuePriority = .veryHigh
```
CKAsset 文件上传的时候 Cloudkit 有一个复杂的请求过程，需要好几次请求[^ckassert]，都 9102 了，Apple 的 CDN 文件下载还是这么慢。


此后，我将音频文件迁移到阿里云 CDN 上，文件由 Web Server上传到 CDN，App 从 CDN 上下载音频小文件。


### 第二次”死“：Cloudkit 保存记录后索引重建导致实时性问题
这个问题非常鬼异，一直到我 App 发布后，有不少用户下载使用后才发现，导致 App Store 评分就是一三角形啊。
当你新增一条记录后，该记录你可以使用 RecordName 查询到，但不一定能用 CKQuery 其他方式查询到。在完成索引重建之前是查不到的，而这个索引大多是立即完成，也有可能是延时 10 几秒，倒霉情况我遇到是半小时。[^ckquery]  [^ckquery2]  [^ckquery3]  

### 其他问题

#### Server 端 SDK 的问题
如果你和我一样在 node 服务端使用 Cloudkit JS,你会发现，这是一个压根就没怎么想给你用在服务端的脚本。首先你得使用`node-fetch` 桥接一下，文档少，sample 少，这个JS文件还不开源，文件还被混淆过了，我在使用 CKAsset 上传附件后，文件死活损坏，搞不清问题出在哪，到底传什么样的格式文件。


#### 查询接口不易用
- 你想 count 一下整张表，在 Cloudkit 里也是难事，感受一下 query all 的递归写法 [^queryall]：
```javascript
function queryAll(database, query, options) {
  var allRecords = [];

  function doQuery(queryOrPreviousResponse, options) {
    return database.performQuery(queryOrPreviousResponse, options)
      .then(function (response) {
        response.records.forEach(function (record) {
          allRecords.push(record);
        });
        if (response.moreComing) {
          return doQuery(response);
        }

        return allRecords;
      });
  }
  return doQuery(query, options)
}
```
- 同样的写一个分页查询也很别拗
```javascript
function Paginator(database, query, options) {  
  var queryOrPreviousResponse = query;  
  this.next = function() {  
       return database.performQuery(queryOrPreviousResponse, options)  
            .then(function(response) {  
                 if(response.moreComing) {  
                      queryOrPreviousResponse = response;  
                 } else {  
                      queryOrPreviousResponse = null;  
                 }  
                 return response.records;  
            })  
  };  
  
  this.hasNext = function() {  
       return queryOrPreviousResponse !== null;  
  };  
}; 
```

[^ckquery]: https://developer.apple.com/documentation/cloudkit/ckqueryoperation/1515283-recordfetchedblock
 [^ckquery2]: https://stackoverflow.com/questions/48029222/cloudkit-query-returns-partial-results-no-errors
 [^ckquery3]: https://forums.developer.apple.com/thread/24679](https://forums.developer.apple.com/thread/24679
[^ckassert]: https://developer.apple.com/library/archive/documentation/DataManagement/Conceptual/CloudKitWebServicesReference/UploadAssets.html#//apple_ref/doc/uid/TP40015240-CH8-SW1
[^queryall]: https://forums.developer.apple.com/thread/11157

### 最终放弃了 Cloudkit
在使用 Cloudkit 之前，其实也对比过其他 Baas 平台，目前国内 firebase 无法使用，Parse 已死，和 Parse 最接近的 LeanCloud 每月有300 元的最低消费，针对小团队，也没有太好的选择。
现在迁移到 阿里云 MongoDB，重写这块代码只花了三天，而排查 Cloudkit 问题花了一周。