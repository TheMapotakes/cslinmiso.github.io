title: "Heroku-Depolyment-For-Play-Framework"
date: 2014-12-16 08:56:41
categories: 
- Heroku
tags:
- Heroku
- Play Framework
---


今天部屬Heroku的時候，出了錯誤。

由於這個專案用到**play framework**，簡單講一下怎麼設定。

 <!--more-->
 
# Deployment

先切換到專案底下，接著初始heroku

	$ cd {project}
	$ heroku create

heroku就會建立隨機名稱的app，接著只需要將專案設定好。

在專案根目錄建立**Procfile**裡面設定heroku進入點script，若要指定JDK則多加一個**system.properties**

內容如下：

	java.runtime.version=1.7

完成以上步驟後，把master push到heroku branch即可。

	$ git push heroku master

但是push上去heroku回傳訊息一直出現`NullPointerException`。

![NullPointerException][deploy-error-1]

查了一下才發現應該是sbt的plugin沒有設定好，造成heroku沒有辦法順利往下做。


位置：`project/plugin.sbt`

	addSbtPlugin("com.typesafe.play" % "sbt-plugin" % System.getProperty("play.version”))
 
而拋出`NullPointerException`的地方，應該就是在取得`play.version`的時候出現的錯誤。

將以上這段，**指定成目前使用的play版本即可。**

	addSbtPlugin("com.typesafe.play" % "sbt-plugin" % "2.2.0”)


接著編譯是通過了，但是伺服器沒起來。

![Deploy Fail..][deploy-error-2]

## All about settings...

下指令查log後發現，原來**Procfile**裡面下的指令是錯誤的。

	$ heroku logs

根據[官方文件](https://devcenter.heroku.com/articles/play-support)，原來play 2.2+要用以下方式


	Play 2.2+
	The default web process is determined by examining output from the native packager plugin. If your app only contains one process, the following default web process is generated:

	web: target/universal/stage/bin/{your project name} -Dhttp.port=$PORT


重新commit並push後，終於起來啦！



Author
========

###Trey Lin / cslinmiso

[My Site](http://cslinmiso.github.io/)

[@cslinmiso](https://twitter.com/cslinmiso)


[deploy-error-1]: /img/Heroku-Depolyment-For-Play-Framework/deploy-error-1.png
[deploy-error-2]: /img/Heroku-Depolyment-For-Play-Framework/deploy-error-2.png