---
layout: post
title: 私彩迷局
date: 2016-11-17
categories: blog
tags: [Life]
description: 赌徒谬误
---

前些天有个小伙伴迷上玩私彩，玩法是这样的：

10个数字选5个，有一个中就算中。开奖结果由官方的时时彩结果为准，50元每注，若赢赔48，平台抽2元
由于是官方（彩票站）出结果，此处私彩平台无法作弊，赔率为1.96，算是相当高了，另外还有一些网站，软件提供各种投注玩法。  
那些算法很复杂，基本都是根据前面出奖的结果来选择下次投注方式，比如倍投法吧：前面不中，则再投前面所有亏金额+50,投注金额：50，100，200，400，800. 表面上看一直用这种方式投注，直到赢为止。那么就不会输了。

这种私彩足以迷惑很多人，或者说很多人不明白平台赚钱的方式，对那种倍投方式会输钱也不解。
这个问题本质上来说叫[“赌徒谬误”](https://zh.wikipedia.org/wiki/%E8%B3%AD%E5%BE%92%E8%AC%AC%E8%AA%A4)
“这种情况可用随机游走数学定理解释。这个系统或类似的系统冒很大的风险来争取小额的回报。除非有无限的资本，这类策略才可成功。”

#### 把这个问题简化一下来解释：

10选5，中奖的概率是1/2，就是投硬币的概率，最关键是平台有抽成，所以一次投注数学期望是－2元。
如果是随机投法，由于有抽成，1000块的本钱，平均只能玩1000次。

倍投的玩法的问题比较隐藏，实际由于倍投的金额成等比数列，很大，比如出现连续7次不中概率其实不算低（1/128），就算中了，由于注数加大，一次被抽成的数额会变的更大，也就是1000块本钱，远gfqp玩不到1000次，倍投越大，输完越快。

虽然我解释了半天让他不要再浪费时间在这上面，这货还是不信，只好写点代码用运行来结果来打他脸:

```

var stat = {money: 1000, loseCount: 0};
var lotteryResult = [];
// 下注次数
var n = 0;
//出彩次数
var BetCount = 1000;
//随机1000次投注结果
function initLotteryResult(arr) {
    let getRandomInt = function (min, max) {
        return Math.floor(Math.random() * (max - min)) + min;
    };
    for (var i = 0; i < BetCount; i++) {
        var j = getRandomInt(0, 100);
        arr.push(j % 2 == 0)
    }
}

function getBetMoney(i) {
    if (i == 0) {
        return 50;
    } else {
        return getBetMoney(i - 1) * 2;
    }
}

function bet(stat) {
    var betMoney = getBetMoney(stat.loseCount);
    stat.money -= betMoney;
    var ret = lotteryResult[n];
    if (ret) {
        stat.money += betMoney * 2 - (betMoney % 50) * 2;
        stat.loseCount = 0;
    }
    else {
        stat.loseCount += 1;
    }

    return {m: betMoney, r: ret};
}

initLotteryResult(lotteryResult);

while (stat.money > 0 && n < BetCount) {
    var ret = bet(stat);
    console.log("第" + (n + 1) + "次 下注 " + ret.m + " 结果：" + ret.r + " 余钱:" + stat.money);
    n++;
}
console.log("end...");

```

实际运行结果，很多情况下，几百次就可以输光，倒霉的时候100把就能输光。

不知道有多少人还在沉迷这种私彩，看吧：

**把一个简单的问题复杂话，就可以迷惑别人**
比如把这个游戏简化成投硬币，而不是10选5，就难以迷惑别人，如果换成30选15玩法，就有更多自以为是的投注玩法让别人陷进计算里。

