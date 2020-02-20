---
layout: post
title: WatchOS开发-使用Apple Watch扬声器播放声音
date: 2017-11-26
categories: blog
tags: [Work]
description: How to play audio file use Apple Watch speaker
---

最近在大叔送孩子上培训班的时间给[YaoYao 跳绳App](http://haozes.me/blog/2017/07/01/yaoyao/)增加了一个功能：运动开始暂停语音提示，并且每跳绳100次，实时语音播报次数。这么炫酷的功能是如何做的呢？  

在Watch里使用扬声器播放声音，目前大叔测试成功有两种。

##### WKAudioFilePlayer 必须在蓝牙耳机上播放
WatchOS 2.0 添加了WKAudioFilePlayer，当你使用WKAudioFilePlayer 播时一段音频的时候，你就会发现提示你需要连续蓝牙耳机，此时Apple爸爸的意思是，你需要一个Airpods，就可以啦。这草种的！
![image](/img/post/006tKfTcgy1flvs6o4jppj307k09gaag.jpg)


#### 一、使用WKInterfaceInlineMovie来播放录制音频
这个trick，可以播放录制音频作为背景音。在Watch App 上放一个WKInterfaceInlineMovie 控件，高宽设成0，用这个视频播放器来播放音频。

```swift
  func playAudio(_ mp3Name:String){
        let soundPath = Bundle.main.path(forResource: mp3Name, ofType: "mp3")
        let soundPathURL = URL(fileURLWithPath: soundPath!)
        
        movieCtrl.setMovieURL(soundPathURL)
        movieCtrl.play()
    }
```

另外：注意WatchOS 支持的音视频格式：
	
Audio:Bit rate: 32 kbps stereo

#### 二、使用TTS 将文本合成音频并播放
如果音频是任意的，比如[YaoYao](https://itunes.apple.com/cn/app/yaoyao-jump-rope-counter/id1179393901?l=en&mt=8) 里的报数，每100个录制一个音频是不切实际的，如果再考虑多语言，音频文件数量就更多了，好消息是，Apple 爸爸在WatchOS 2.0 里引入AVSpeechSynthesizer，可以使用TTS功能。
```swift
class TTSSpeaker: NSObject {
    let synth = AVSpeechSynthesizer()
    
    override init() {
        super.init()
        synth.delegate = self
    }
    
    func speak(_ announcement: String) {
        let langCode = Locale.current.languageCode
        let regionCode = Locale.current.regionCode
        let language = "\(langCode!)-\(regionCode!)"
        
        print("speak announcement in language \(language)")
        let utterance = AVSpeechUtterance(string: announcement.lowercased())
        utterance.voice = AVSpeechSynthesisVoice(language: language)
        utterance.volume = 1.0
        utterance.rate = AVSpeechUtteranceMinimumSpeechRate
        synth.speak(utterance)
    }
    
    static func prepareAudioSession() {
        do {
            try AVAudioSession.sharedInstance().setCategory(AVAudioSessionCategoryPlayback, with: .mixWithOthers)
        } catch {
            print(error)
        }
        do {
            try AVAudioSession.sharedInstance().setActive(true)
        } catch {
            print(error)
        }
    }
}

```
此处为了兼容多语言，自动判断了language，其次在播放的时候最好设置一下AVAudioSession为混音模式,不打断其他声音播放。

#### WatchOS 应用后台运行问题
WatchOS 为了省电，普通App很难在后台运行。  
比如一个蕃茄钟应用的典型需求是，到时间点，Watch可以振动提醒，实际这个在WatchOS也难以实现，你会发现一旦手腕放下，屏幕关闭，再抬起，UI并没有即时刷新，计时器挂起，时间到了，也不会振动。    

很多Watch上的蕃茄钟应用会用很ugly的方法来实现，用消息通知，这样还用户需要把提醒通知删除。

##### 体能训练应用特权
WatchOS 应用里如果你启动了HKWorkoutSession，程序的优先级就很高，程序切换到后台也可正常运行。如果你实在需要前台运行一切代码，也可以使用这个trick。



#### 参考
[App Programming Guide for watchOS - Audio and Video](https://developer.apple.com/library/content/documentation/General/Conceptual/WatchKitProgrammingGuide/AudioandVideo.html)  

[定制 Audio Session 的 Category](http://www.samirchen.com/ios-avaudiosession-3/)
