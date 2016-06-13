title: "Why you can't use comments on Reusable block"
date: 2015-01-09 11:56:41
categories: 
- Java
tags:
- Play Framework
- Java
---

最近在做網頁的時候發現一個誤用play template comment的問題，以下是示意圖。

	Note:
	- Using Java
	- Play framework 2.2.2
	- Scala 2.10

 <!--more-->
 
![Normal page][html-image-1]

這張圖是正常的網頁，可以看到header是正常切齊的，CSS也都正常沒有跑掉，下面開啟開發人員工具對照原始碼一下。

接著第二張圖就是今天的重點。

![Page went wrong.][html-image-2]

Header與正常版對照明顯有差異，且CSS也跑掉了。
仔細看一下原始碼，會發現應該在head標籤內的js與css，甚至title等都跑到body裡面了，其中也包括header。

為什麼會造成這樣的結果呢？

找了才發現是這頁的template有個地方宣告的reusable block造成問題。

先說明這頁template的構成。

```
**order.scala.html**
	
@(some variables)
@views.html.main("title") {
	...本頁要include的內容...
}
```
	
呼叫 main - 包含主要CSS,JS以及header,footer。

將本頁定義好的html include進去。

有問題的code見下圖：

![Problematic line][code-image]

就是標出來的這一行有問題，根據google到的說法，`@* *@`這種註解方式不適用於reusable block上，因為這並非scala正確語法，而且`@{ something }`裡面的內容會被當scala code解讀。

[Why can I not mix Scala code with HTML here in Play Framework 2 views?](http://stackoverflow.com/questions/21658000/why-can-i-not-mix-scala-code-with-html-here-in-play-framework-2-views)

```
The problem with your code is the use of @{...}. **The thing is, all inside that block is treated as scala code. That is why @* *@ comments do not work, they are not valid scala syntax. Scala has xml literals which allow for the html tags inside.
**
...

So use @ for simple expressions. Use @{...} for not so simple expressions, but keep in mind that everything inside the block is scala code (so no further @). The places where you cannot use the @ are within scala code.
```

所以判斷可能play framework在根據template 構成畫面的時候，因為這個原因造成錯誤，原本的head以及header被誤判為include的內容，所以一併被放到body內。

解決方法就是刪除本行，並且記住，若要註解**reusable block**的話，需要直接刪除。

或者使用 `@{ /* something */}` 來註解區塊內的scala code。

不能使用`@* *@`來註解，這將會造成模板構成的錯誤。

以上。


[html-image-1]: /img/Why-you-cant-use-comments-on-Reusable-block/html-image-1.png
[html-image-2]: /img/Why-you-cant-use-comments-on-Reusable-block/html-image-2.png
[code-image]: /img/Why-you-cant-use-comments-on-Reusable-block/code-image.png