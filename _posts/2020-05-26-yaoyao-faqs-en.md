---
layout: post
title: YaoYao FAQS
date: 2020-05-26
categories: blog
tags: [Coding]
description: YaoYao-Frequently Asked Questions
---
<script type="text/javascript" src="//translate.google.com/translate_a/element.js?cb=googleTranslateElementInit"></script>
<div id="google_translate_element"></div>

<script type="text/javascript">
function googleTranslateElementInit() {
  new google.translate.TranslateElement({pageLanguage: 'en'}, 'google_translate_element');
}
</script>

* [1. No app found on Watch after installation](#NoappfoundonWatchafterinstallation)
* [2.Workout data cannot synced to the iPhone.](#WorkoutdatacannotsyncedtotheiPhone.)
* [3. App  version updated on iOS, but not on Watch](#AppversionupdatedoniOSbutnotonWatch)
* [4.The number of jump ropes counted is higher or lower than the actual number](#Thenumberofjumpropes)
* [5. Calorie data is inaccurate?](#Caloriedataisinaccurate)
* [6.App Health Privacy Authorization (one of the reasons why workout data is not recorded on the phone)](#AppHealthPrivacyAuthorizationon)
* [7. Force quit YaoYao on watch to avoid rebooting  watch.](#ForcequitYaoYao)

* [8. Which platforms YaoYao currently supports ](#platform)
* [9. Any other questions?](#Anyotherquestions)


##  <a name='NoappfoundonWatchafterinstallation'></a>1. No app found on Watch after installation

Normally, the Watch App will be installed on your watch after the mobile app is installed, but the watch may not be able to be installed instantly due to low battery or unstable network. 
#####  Solution 1: Close WIFI,switch to  4G and reinstall app.
#####  Solution 2: Install from watch App Store
After watchOS 6, you can search: YaoYao from the App Store on apple watch to find the app installation.（No duplicate charges）
#####  Solution 2：Reboot / Reinstall
Reboot the iPhone and Watch, or reinstall app


##  <a name='WorkoutdatacannotsyncedtotheiPhone.'></a>2.Workout data cannot synced to the iPhone.
1. Check App Health Privacy Authorization (FAQ 6)  
2. YaoYao's workout data is stored in HealthKit, and your iPhone is synchronized with the Apple server.
Normally, within a few minutes, the corresponding records will be available on your iPhone. In case they are not synced for a period of time, please follow these steps to check.
![Sync Problem](http://cdn.onlytalk.top/blog/faq-2.jpg)
1. Health->Avatar(upper right corner)->Devices,
2. Delete the watch device that cannot be paired.

If it still doesn't sync, you may need to re-pair your watch. In addition, the watch with too low a charge may also cause records to be out of sync.

##  <a name='AppversionupdatedoniOSbutnotonWatch'></a>3. App  version updated on iOS, but not on Watch
Sometimes, the YaoYao iOS app version is updated to 2.0.1, but the watch version is only 2.0.0.
#####  1. Check on the Watch App to see if YAOYAO is installed.
#####  2. Check for updates from the App Store on your Watch.
Find the app store from the watch's app dock and tap on "Account"->"Update", update it manually.
 ![update from watch](http://cdn.onlytalk.top/blog/faq-3_en.jpg)

##  <a name='Thenumberofjumpropes'></a>4.The number of jump ropes counted is higher or lower than the actual number
If the error exceeds 50%.

#####  1. Please check if the "double under" mode is checked, in double under mode, single shake will be reduced by half.
#####  2. Don't using YaoYao both with other workout app together.
#####  3. Please reboot the watch/ Reinstall YaoYao App.

The normal error is in the 1% to 5% range, adjust the sensitivity  on watch app setting if the error is smaller than the actual times.
Turn up the sensitivity.  Otherwise, turn it down. (Just for reference, if your speed is greater than 180/min , it is recommended to set to 5, if the speed is less than 90/min, you can set to 2.)

Sensitivity settings.
![Sentivity](http://cdn.onlytalk.top/blog/faq4_en.jpg)

##  <a name='Caloriedataisinaccurate'></a>5. Heart rate/Calorie data is inaccurate? 
Firstly, YaoYao's calorie data is calculated by watchOS, based on heart rate, exercise sensor strength, time, type of exercise, age, etc., and is not a completely linear result, normally around 600 total calories in an hour.
Secondly, dynamic calories refer to calories burned during exercise and do not include base energy burned.

You should wear watch more tighter to make the electrical and optical heart sensors work well. Otherwise, it will lead to inaccurate heart rate and calorie data, such as: the watch app shows the heart rate gray, workout results detailed data has flat heart rate curve.

__Total Calories  burned = Active Calories Burned + Base Energy Burned__

YaoYao shows active calories by default, and total calories are shown in the workout details.
Not wearing the strap tightly can seriously affect the accuracy of the heart rate and calorie data, and it is recommended that you wear it tighter, since heart rate has the most impact on calories!


##  <a name='AppHealthPrivacyAuthorizationon'></a>6.App Health Privacy Authorization (one of the reasons why workout data is not recorded/Heart rate not show on the phone)
The first run of the App will request  health privacy authorization to read or write various types of workout data to Apple Health, which can be opened manually if ignored.
#####  Check on the iPhone
Open 「Setting」->「Privacy」->「Health」-> Find「YaoYao」->「Turn all Categories On」
#####  Check on the Watch
Open 「Setting」->「Health」->App-> Find「YaoYao」->「Turn all Categories On」
![health privacy](http://cdn.onlytalk.top/blog/faq6_en.jpg)

##  <a name='LiveCount'></a>7. Music is easy to be interrupted while jumping  and listening to songs with headphones, 
- Place your iPhone on a table or stand while jumping.  
- Open YaoYao App on iPhone.
- Start workout with YaoYao Watch App.  
Use 「live count」 feature, the deer on iPhone will jump with you, not only displaying the number of jumps in real time, but also announcing loudly. Or if you like to listen to music while jumping, whether it is headphones connected to Watch or iPhone, you will hear the announcement more clearly.
More details about Live Count feature:

![livecount](https://cdn.sspai.com/2020/05/12/edf762cb42cef688570528e37c4a175e.gif)
https://zhuanlan.zhihu.com/p/139867684

##  <a name='ForcequitYaoYao'></a>8. Force quit YaoYao on watch to avoid rebooting  watch.
Completely exit YaoYao on the watch when YaoYao open (press and hold the long button on the bottom right side of the watch, wait for the "POWER OFF" to appear, then press and hold the top digital crown button)

## <a name='platform'></a>9. Which platforms YaoYao currently supports 
YaoYao now works on Apple Watch(all series), Samsung Galaxy Watch(1,2,3),Planing to support Huawei HarmonyOS watch

##  <a name='Anyotherquestions'></a>10. Any other questions?
You can send us e-mail,bug report in YaoYao app setting, and we will respond to your feedback within one day. or you can contact us at:

E-mail: [haozes@gmail.com](mailto:haozes@gmail.com)  
Twitter: [haozes](https://twitter.com/haozes)  
Wechat: haozes  
Telegram Channel: [YaoYaoNow](https://t.me/yaoyaonow)   


