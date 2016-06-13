title: Node.js deployment on Cloudbees
date: 2014-08-06 16:09:44
categories: 
- Node.js
tags:
- Node.js
- Cloud
- CloudBees
---

今天要說一下如何在[CloudBees](http://www.cloudbees.com)環境中部屬**Node.js**應用程式。

<!--more-->

# **新增應用程式**
首先必須先申請 [CloudBees](http://www.cloudbees.com) 的帳號，接著進入**主控台** [console.cloudbees.com](https://console.cloudbees.com/)

點選**Apps**後，按下上方 **Add Application**

![Add Application][add-app-1]

輸入應用程式名稱，**名稱必須全為小寫**。選擇應用程式 **Stack** 為 **Node.js**

![Choose Stack as Node.js][add-app-2]

按下確定後，即可在主控台看見剛剛新增的應用程式。

![Deploy][deploy-1]

重頭戲來了，開始部署應用程式。

# **部署應用程式方式**

這邊有兩種方式部署：

- **透過網頁介面上傳** (沒有嘗試過)
- **透過CloudBees SDK上傳** (本文將會展示流程)

根據 [CloudBees SDK](http://developer.cloudbees.com/bin/view/RUN/BeesSDK) 教學文件中，其實已經有詳述如何部署Node.js至平台[nodejs-clickstart](https://github.com/CloudBees-community/nodejs-clickstart)。

下面部分文章將會取用官方文件並翻譯之。


# **透過CloudBees SDK上傳**

前置作業當然是要將應用程式包好並**壓縮成ZIP格式**，以下提供我的壓縮檔結構。

![Zip][deploy-2]


其中要注意的是，**`package.json`, `setup`以及`main.js`**這三個檔案必須包含在壓縮檔內。
(setup shell可以在 [nodejs-clickstart-Github文件](https://github.com/CloudBees-community/nodejs-clickstart) 下載得到)

簡單來說，CloudBees在伺服器端是透過`setup`，這個shell來**執行啟動伺服器的動作**。

而shell內預設執行的js檔名為：**`main.js`**

這邊要提到一點，嘗試上傳的過程，我試過不包括node.js的modules就上傳，但是伺服器無法啟動。
查看伺服器log提示**無法載入module**，才發現CloudBees不會幫我們安裝module。
我還天真地以為他會幫我們根據package.json中的設定到npm下載module。T_T

於是我的解決方法就把module包在壓縮檔內。

到這邊前置作業完成，開始部屬。

# **開始部屬，終於**

部屬應用程式必須下載[CloudBees SDK](http://cloudbees-downloads.s3.amazonaws.com/sdk/cloudbees-sdk-1.5.2-bin.zip)，下載後解壓得到一個資料夾。
執行資料夾內的**Bees Console**。
~~說穿了就是個命令提示字元~~，他...就是個**Console**。


部屬的完整指令如下：

> bees app:deploy -t nodejs target/app.zip

指令說明：
若要驅動SDK的指令是`bees`，而部屬的參數是 `app:deploy`
`-t nodejs` 平台將會使用最新版本的Node.js
當你設定版本參數： `-R`, `-t` 以及 `-D` 時，只需要設定一次，平台將會記住你的版本選擇。

```
 cd sdk  --進入SDK的資料夾
 bees app:deploy -t nodejs target/app.zip  --部屬應用程式，程式最好放同一層
```

執行指令後，SDK會先開始更新，**接著會要求你輸入帳號密碼以及要部屬的應用程式名稱。**

![Console Deployment][deploy-3]


圖中因為我已經輸入過帳號密碼，於是下次就不必再輸入。
**紅線框起部分為應用程式名稱**，上次不小心打錯，結果竟然就創了一個**新應用程式**。
這點要注意一下，免費用戶只能建立兩個實體。(?)

看到最後一行，會顯示應用程式名稱以及程式對外網址。

若要查詢對外網址，也可以在**主控台** [console.cloudbees.com](https://console.cloudbees.com/) 內查看，Location下面的欄位。

![Location][deploy-1]

若是網頁無法跑出來，代表程式有問題，就必須點選下面的Log頁籤，查看伺服器Log。


今天就先講到這邊，若有問題或錯誤，請留言發問/指正。


# **官方網站**

[Node.js ClickStack](https://developer.cloudbees.com/bin/view/RUN/NodeJS)

[nodejs-clickstart-Github文件](https://github.com/CloudBees-community/nodejs-clickstart)


> Written with [StackEdit](https://stackedit.io/).



[add-app-1]: /img/Nodejs-deployment-on-Cloudbees/cloudbees-add-app-1.png
[add-app-2]: /img/Nodejs-deployment-on-Cloudbees/cloudbees-add-app-2.png
[deploy-1]: /img/Nodejs-deployment-on-Cloudbees/cloudbees-deploy-1.png
[deploy-2]: /img/Nodejs-deployment-on-Cloudbees/cloudbees-deploy-2.png
[deploy-3]: /img/Nodejs-deployment-on-Cloudbees/cloudbees-deploy-3.png