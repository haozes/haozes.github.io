---
layout: post
title: Node+Mongodb 架构常见性能问题总结
date: 2019-01-30
categories: blog
tags: [Coding]
description: Node+Mongodb 架构常见性能问题总结
---


## 简介
目前的我们的一个项目，后端使用 node+mongodb+redis 搭建，已运行 2 年，目前日 pv 在 100W 左右。
#####  配置：
两台阿里云 ECS (2 vCPU 4 GB )
一个阿里云 mongodb。（4核8G,节点数,三节点）

此文由近两年来实际血泪经验，无教科书式说教。

## 常见现象1：Web 服务超时，node 服务内存占用高。Mongodb CPU🔥，IOPS🔥 高
正常情况下单个 node 服务占用内存在 100-200M 左右，此时内存可能涨到500-600M以上，Mongodb CPU 超过90%。web 服务失去响应。
![正常情况下 node 服务内存很少](https://tva1.sinaimg.cn/large/006tNc79ly1fzoeyrg559j30go046wez.jpg)
### 问题原因：
node 每增加一个回调/promise 异步任务，都会创建 一个`microtask`到执行队列，由于太多的`microtask`等待处理完成,新的`microtask` 在任务队列的尾端，得不到处理，web QPS 也因此迅速下降，造成 web 服务内存占用高。这种情况一般是后端 Mongodb 处理不及时拖累 node 服务，这是最常见的性能问题。  

### 调试方法：
登录 mongo 执行执行执行 `db.currentOp()`,查看是否有执行慢的的任务。如：


    mgset-2286:PRIMARY> db.currentOp()
    {
	"inprog" : [
		{
			"desc" : "conn33170850",
			"threadId" : "47003141687040",
			"connectionId" : 33170850,
			"client" : "10.24.141.149:49804",
			"active" : true,
			"opid" : -1695682558,
			"secs_running" : 0,
			"microsecs_running" : NumberLong(9),
			"op" : "command",
			"ns" : "admin.$cmd",
			"query" : {
				"currentOp" : 1
			},
			"numYields" : 0,
			"locks" : {

			},
			"waitingForLock" : false,
			"lockStats" : {

			}
		},
		{
			"desc" : "conn33150143",
			"threadId" : "47003176310528",
			"connectionId" : 33150143,
			"client" : "10.24.141.149:55818",
			"active" : true,
			"opid" : -1695713531,
			"secs_running" : 148,
			"microsecs_running" : NumberLong(148290542),
			"op" : "remove",
			"ns" : "mydb.bhd_record",
			"query" : {
				"recType" : 9,
				"content.id" : ObjectId("5a044ed48162b041725e3435")
			},
			"numYields" : 283277,
			"locks" : {
				"Global" : "w",
				"Database" : "w",
				"Collection" : "w"
			},
			"waitingForLock" : false,
			"lockStats" : {
				"Global" : {
					"acquireCount" : {
						"r" : NumberLong(283278),
						"w" : NumberLong(283278)
					}
				},
				"Database" : {
					"acquireCount" : {
						"w" : NumberLong(283278)
					}
				},
				"Collection" : {
					"acquireCount" : {
						"w" : NumberLong(283278)
					}
				}
			}
		}
	],
	"ok" : 1
}

_secs_running_  ,_microsecs_running_  反映了某些执行语句执行时间，如上问题上mydb.bhd_record 查询时字段未能命中索引，查询需要全表扫描。		  
使用`db.currentOp` 可以检查出常见的索引问题，以及错误的执行了 MapReduce 函数导致的性能问题。

### 优化策略：
1. 优化索引：如上可能需要对 recType 和 content.id 字段加索引，Monogo 里建议更多使用复合索引，复合索引的排序顺序是：粗->细，如recType 是对表的粗分，假如表有1KW 条数据，recType 有 10 种， content.id 是唯一的，那么复合索引的排列顺序是，recType -> content.id。
2. 尽量避免在 生产环境上使用 MapReduce，MapReduce 是服务器级全局读写锁，aggregate 可以一定程度代替 mapreduce 。
3. 使用如 Redis 缓存将 必须使用 mapreduce或慢查询结果缓存。

在实际生产环境上，服务器挂掉两次是因为 Redis  服务器配置错误，缓存失效慢查询被并发。

## 常见现象2：Web 服务超时，node 服务内存略高，Mongodb CPU,IOPS 都正常

如果在此时多配 node 服务负载均衡会有一定的效果。或者把 node 服务重启，速度会马上提升，但很快就不行了。

### 问题原因：
这种情况下数据库并没有太大压力，但依然响应慢。  
这个问题依然要回到 node 的执行机制， node 服务里很多执行时间比较久的`microtask`没有完成，造成任务队列迅速累积，由于代码逻辑流程问题，但不一定是因为数据库响应不及时。  
如一个订单复杂的处理流程，要去不同的服务接口较验或者每个数据库操作也很短，但等待有多个步骤完成，整个流程累计执行时间较长。


### 优化策略：
将执行时间久的任务，放到任务队列组件中执行。node 任务队列组件推荐 [Kue](https://github.com/Automattic/kue),[Bull](https://github.com/OptimalBits/bull), Kue 和 Bull 都使用 Redis 做后端。

    var kue = require('kue')
     , queue = kue.createQueue();
    
    queue.process('email', function(job, done){
      email(job.data.to, done);
    });
    
    function email(address, done) {
      if(!isValidEmail(address)) {
        //done('invalid to address') is possible but discouraged
        return done(new Error('invalid to address'));
      }
      // email send stuff...
      done();
    }

总之，你的 node web 服务应该执行短快的任务，否则大量并发会使性能迅速下降。 

## 常见现象3：大的循环下，处理速度越来越慢。
    var a = 0, b = 10000000;
    
    function numbers() {
      while (a < b) {
        console.log("Number " + a++);
      }
    }
    
    numbers();

这是一个比较 tricky 的问题，遍历打印一个 1000W 的 数，最后内存暴涨，越来越慢，最后：  

    FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - JavaScript heap out of memory
     1: node::Abort() [/Users/haozes/.nvm/versions/node/v8.1.4/bin/node]

### 问题原因：
node 是单线程执行，在这个同步代码中，GC 没有机会运行。这种情况在我经常写脚本遍历整个数据库的时候也经常发生。
### 优化方法：
    var a = 0, b = 10000000;
    
    function numbers() {
      var i = 0;
      while (a < b && i++ < 100) {
        console.log("Number " + a++);
      }
      if (a < b) setImmediate(numbers);
    }
    
    numbers();

将函数执行放在 event loop 尾部执行，让主线程喘息一下。  
这样写代码还是很拗口，我在遍历数据库时经常这样做：

    var fork = require('child_process').fork;
    // 同时允许几个子线程处理
    let MAX_CHILD = 2;
    let childProcCount = 0;
    var progress_count  = 0；
    
    // 在子进程里处理
    function forkChild(guid) {
    
        var child = fork('./tool/user_stats/child',[guid]);
    
        child.on('message', (msg) => {
            console.log('        msg:',childProcCount, msg);
        });
        child.on("close", function () {
            childProcCount--;
        });
    }
    
    async function main() {
    
        console.log("start,from:",progress_count);
    
        while(true){ // 遍历所有用户，直到完成
            if(childProcCount < MAX_CHILD){
                var users = null;
                var collection = await getCollection('user'); 
                if((users = await collection.find({}).skip(progress_count).limit(1).toArray()).length < 1){
                    break
                }
                console.log(">>> ",progress_count);
                // 丢到子线程处理
                forkChild(users[0].guid);
                childProcCount++;
                progress_count++;
    
            } else {
            	// 等待子线程处理完再下一个
                await sleep(100);
            }
        }
    
    
    
        console.log("All data processed!");
    }
    
    main();

## 其它：
### Mongodb 读写分离及注意事项
在配置读写分离后，假如是主库写，从库读后，容易造成从库读的不一致性。注意majority 属性:  
    
    recordCollection.insert(rec, {writeConcern: {w: "majority"}})



## Reference
macrotask与microtask  
http://www.ayqy.net/blog/javascript-macrotask-vs-microtask/  
理解事件循环一(浅析)  
https://github.com/ccforward/cc/issues/47  
理解事件循环二(macrotask和microtask)  
https://github.com/ccforward/cc/issues/48  
