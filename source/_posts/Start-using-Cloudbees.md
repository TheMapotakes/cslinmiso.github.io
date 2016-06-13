title: Start using Cloudbees
date: 2014-08-02 20:28:18
categories:
- Cloud
tags:
- Cloud
- CloudBees
---

　　最近因為女友的需求，做了個系統放在網路上。但又因為是實驗性的系統，於是不想找付費的部屬平台。

 <!--more-->

　　所以就開始使用[CloudBees](http://www.cloudbees.com)，CloudBees的免費帳號可以在2 cells, 256MB的平台設置不同的container，因為平時都是使用glassfish比較多，剛好這邊也有支援。不過應用程式，若是兩小時內沒有人使用，則會進入**冬眠模式**，直到下次請求才會再度被喚醒，耗時約三分鐘。

　　同時，CloudBees也提供**免費MySQL資料庫(ClearDB 5MB / 10 connections)**，不過只有2x5MB的大小，但算是夠用了。

　　一開始嫌5MB太小，於是就在[db4free](http://www.db4free.net)建了個資料庫，雖然速度慢了一點，但是容量可以達到100MB也沒有限制流量。缺點就在於速度實在有點差強人意，於是一陣掙扎之後，還是回來使用[CloudBees](http://www.cloudbees.com)的DB。

　　不過今天轉換DB到CloudBees時，設定Hibernate遇到了一個問題。
本來以為只需要更換db url跟帳號密碼就好，但是在查詢中文欄位的時候，怎麼查就是查不出來，永遠都是查不到東西。

　　仔細查了一下，原來MySQL有分兩種資料庫引擎： InnoDB和MyISAM，而CloudBees的ClearDB是預設使用InnoDB，在hibernate.dialect的參數必須使用 `org.hibernate.dialect.MySQLInnoDBDialect`

　　隨之一改完，重新編譯程式並部署就一切正常了！


　　但真正疑惑的是，當我使用db4free時，預設的引擎也是InnoDB，而當時我使用的dialect是`org.hibernate.dialect.MySQLDialect`，很奇怪的卻沒有這個問題..............

　　於是在轉換DB時，就接續著用而造成了這個問題。 T_T


　　以上就是最近用Paas平台遇到的問題，若有興趣可以一起討論！


##參考網站

http://log-cd.iteye.com/blog/431281

[CloudBees](http://www.cloudbees.com)