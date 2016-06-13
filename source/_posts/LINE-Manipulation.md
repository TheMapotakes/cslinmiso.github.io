title: LINE Manipulation
date: 2014-08-23 20:10:49
categories: 
- LINE
tags:
- LINE
---

由於最近私人的案子，根據功能需求，必須得有辦法使用LINE傳送訊息，新增/刪除/封鎖好友。

後者是本來就已經寫好的，所以目前要寫的是新功能。

<!--more-->

研究了一下，網路有人整理出[LINE-Protocol](http://altrepo.eu/git/line-protocol.git/)，所以我們缺少的就是呼叫端、加解密以及LINE Token的取得。

所以我只是來這邊把一些遇到的問題記錄下來，不要太期待我會教你們怎麼寫XDDD

雖然是已經寫出來了啦，但是不知道能用多久，若是LINE大改版可能就要隨之修正。


## 轉碼問題
```
 # 取得物件後，發現物件內的中文全為亂碼，判斷為接收後需要轉碼一次
 # 於是修改以下，將readString()取得的字串，加入解碼為utf-8則正常顯示中文
 Curve.ttypes
 def read(self, iprot):
 ...
  iprot.readString()**.decode('utf-8');**
 ...
```

## Token取得方法
```
 # 使用正常帳號密碼登入後，取得訊息回傳的物件並且得到token存入header
 # 再透過curve建立連線物件
 self._headers['X-Line-Access'] 
```

## 區分使用者名單狀態
```
 // 有效使用者帳號，包含群組 (非刪除、封鎖) 
 'G+1aMBRdEa4d+ZtffdLoT4144bgM/UxktBDGh+G2YoF07toWhWzCvn9LZU5dsPev1EuAQsZ/FbG9jvZUmSTqvU5bbiNjTCLoGa+Z5iENsekt+590JIctQEH/SkIgBYxV2bGXtS8dztrcp+o98B4Xlzfqh4Y7ptRcP11B6Qf%'
```

## LINE版本位置
```
 Base Address: 00A442F0
```

