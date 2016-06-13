---
title: Google contact sync fix for MIUI7
date: 2016-05-04 13:59:17
categories:
- Android
tags:
- Android
- MIUI
- MI5
- 小米5
- Google Contact Sync
---

	如何解決小米5(MIUI 7)無法同步Google聯絡人。

同事買了小米5後，發現Google 聯絡人無法同步，找了許多方法都沒用。

包括各種Google安裝器，聯絡人說不同步就是不同步。

大家都沒辦法了，我咧，最喜歡挑戰這種無聊的事情(？)，所以也來試試看了。

 <!--more-->
 
## ** 找問題，先Google **

網路找到的解決方法不外乎，

1. 要把權限打開，允許Google聯絡人同步讀取通訊錄等等。
2. 使用Google安裝器重新安裝

但是，**小米5(MIUI 7)**就是沒用，明明別人說有用的方法就是沒用。

最後在國外網站找到方法，我重新包裝壓縮，簡單說明如下。


## ** 最終解決方法 **

> 此方法目前只適用於MIUI7以上版本

下載[Google框架安裝檔](https://goo.gl/BvM8C7) 

解壓密碼為：![Password][PASSWORD4ZIP]

如果之前有裝過Google框架的人，請移除乾淨後再裝。

1. 解壓後，安裝Google框架等四項app： Google framework, play service, play store and account manager

> 注意！安裝完成後，都還不要打開Play商店。

- com.android.vending_80310011.apk
- com.google.android.gms_6599036.apk
- com.google.android.gsf_17.apk
- com.google.android.gsf.login_17.apk

2. 接著重要的一步是，`重開機`！

重開機後再接續安裝以下兩個App：google contacts sync, google calendar sync

- Google 日曆同步處理功能-com.google.android.syncadapters.calendar-16-v4.1.2-610838.apk
- Google 聯絡人同步處理功能-com.google.android.syncadapters.contacts-19-v4.4.2-940549.apk

3. 安裝後再`重開機`！

開機後，執行Play商店登入帳號。

這個時候可以打開聯絡人看看聯絡人是否開始同步了。

若沒有，則要進入 `[安全中心] > [權限管理]` 

將Google聯絡人同步框架開啟權限。(就都改成打勾勾，允許就是了！)

若要檢查是否真的有同步成功，除了開啟聯絡人查看外，

也可以照以下設定：
 
	[設定] > [其他帳號] > [Google] > [你的google帳號] > 立即同步 


應該會看到聯絡人旁邊的圈圈一直轉，若最後底下更新時間顯示為目前就代表成功了！

如果還是不行，就重開機並再度檢查權限。


附上[Youtube教學](https://www.youtube.com/watch?v=UgdsvrtFji0)，基本上大同小異，只不過我先把聯絡人apk提取出來了而已。


## Solution for google contact sync on XiaoMi 5

	How to install google apps and get google calendar and contacts to sync on XiaoMi 5 (MIUI 7).

First download [GAPPS_MIUI_7_RE.zip](https://goo.gl/BvM8C7) 

Password：![][PASSWORD4ZIP]

1. Unzip the file, there will be 4 files, the framework, play service, play store and account manager, install all the GAPPS,

> Do not launch play store application during installation, even after it's done.

- com.android.vending_80310011.apk
- com.google.android.gms_6599036.apk
- com.google.android.gsf_17.apk
- com.google.android.gsf.login_17.apk

2. `Once you finished installing all of 4 files, reboot your device.`

Afterward, start installing google contacts sync and google calendar sync as following file name.

- Google 日曆同步處理功能-com.google.android.syncadapters.calendar-16-v4.1.2-610838.apk
- Google 聯絡人同步處理功能-com.google.android.syncadapters.contacts-19-v4.4.2-940549.apk

3. Reboot your phone before you do anything.

if it still doesn't work after you reboot your device, launch security center and give permission to google contact sync and check all the items under privacy.


## Reference:

[[Tools, Tips & Tutorials] Mi5 (miui 7) How to install google apps and get yr calendar and contacts to sync](http://54.243.194.135/thread-269874-1-1.html)

[How to Install Google Play Store on Xiaomi Mi5](http://tinyurl.com/znd3df8)


[PASSWORD4ZIP]: /img/Google-contact-sync-fix-for-MIUI7/PASSWORD4ZIP.png