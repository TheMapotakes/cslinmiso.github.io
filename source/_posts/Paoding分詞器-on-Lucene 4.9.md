title: Paoding分詞器 on Lucene 4.9
date: 2014-08-05 21:28:18
categories:
- Java
tags:
- Java
- Lucene
- Paoding
---
　　由於目前準備開發的專案會使用到搜尋引擎，想必然 Java 的領域內第一個想到的搜尋引擎就是 [Lucene](https://lucene.apache.org/) ，目前最新版本已經到 4.9 了，值得注意的是4.9版開始**支援Java 8，不支援 Java 7u55以下版本。**

 <!--more-->

　　跟著網路上的一些先進的文章做了一些範例之後，發現若是要將資料有效率的統整必須要有強大的**分詞器**，本來是想使用[IKAnalyzer](https://code.google.com/p/ik-analyzer/)，但考量到此分詞器自2012年開始後，就沒有再更新了，這個時候找到了[Paoding分詞器](https://code.google.com/p/paoding/)，但是作者也沒有再根據新版 [Lucene](https://lucene.apache.org/)  進行相容性的修改，現在也可以說是**荒廢了(?)**

　　幸運的是，讓我找到了大陸一位高手將Paoding改寫向上支援到4.x版：[Paoding分詞器，基於Lucene4.x](http://www.oschina.net/code/snippet_259382_14635)。

　　可是！**事情往往沒那麼簡單**，套用後發現一些錯誤。


##痛苦的開始

　　我使用的是最新的，Lucene 4.9版，首先遇到的錯誤是：
`NoSuchMethodError: org.apache.lucene.store.Lock.release()V`

　　有經驗的朋友都可以從錯誤訊息中讀出，這個訊息是說`release`這個方法並不存在，於是我就找到了Lucene 4.9的API： [Lock.html#close()](http://lucene.apache.org/core/4_9_0/core/org/apache/lucene/store/Lock.html#close())
　　得知從**Lucene 4.7**開始，release這個方法已經被拿掉由`close`取代。

　　所以解決方法是將`net\paoding\analysis\knife\PaodingMaker.java` 內的`lock.release`改為`lock.close()`。
　　
　　好啦，改完開心地跑下去，結果又發生另外一個錯誤。**囧>**
　　
　　接著又出現的錯誤訊息指出`MostWordsTokenCollector`這個類別內，找不到`org.apache.lucene.analysis.Token`的問題。
　　
　　查詢後懷疑應該是編譯有問題，所以把 `net\paoding\analysis\analyzer\impl\MostWordsTokenCollector.java` 重新編譯壓回jar檔後，執行成功。


　　留下此文章來提醒一下遇到相同問題的各位。

　　經取得作者同意後，放上我的github以供各位參考：
​　　https://github.com/cslinmiso/paoding-analysis

> **NOTE:**
> 由於我編譯過的Paoding是以**JDK 7**來編譯的，而Lucene 4.9 **只支援 Java 7u55以上版本，包含Java 8**
>若要使用於4.7以下版本則需要重新使用JDK 6 編譯Paoding成jar後方可使用。否則將會出現`NoClassDefFoundError: >org/apache/lucene/analysis/Token<init>`的錯誤。

##資料來源

Paoding分詞器原作者
https://code.google.com/p/paoding/

[Paoding分詞器，基於Lucene4.x](http://www.oschina.net/code/snippet_259382_14635)

