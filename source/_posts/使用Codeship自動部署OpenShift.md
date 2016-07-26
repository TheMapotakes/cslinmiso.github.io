title: "使用Codeship自動部署OpenShift"
date: 2016-07-25 17:00:41
categories: 
- CI
tags:
- Codeship
- OpenShift
- CI
- GIT
---




這篇文章介紹了如何使用Codeship自動部署OpenShift。

此文不深入探討如何建立或者使用Codeship及OpenShift。

 <!--more-->
 
> Codeship 簡稱為 CS，OpenShift簡稱為OS

要完成部屬，你需要有：

- 要部屬的Git Repo
- 已建立好的OS應用程式。(這很重要因為你必須推送部屬程式碼至OS的Git Repo)
- 已開通的CS project。
- 託管於Github或Bitbucket的Repo。

為了確保可以順利上傳程式碼至OS，我們必須將CS的公鑰(public key)加入至OS環境中。

	Codeship > Project settings > General > SSH public key

取得CS公鑰後，將其加入至OS。

	rhc sshkey add codeship codeship_ssh_pk.pub

接著關鍵設定即是CS的部署腳本

	Project settings > deployment
	
此處可以選擇要部屬的Branch，並根據你的需求修改腳本。

整個流程是這樣：

	Push Commits to Remote > Codeship [Run Test] > Target Branch [Deployment]

所以打個比方，今天我在CS監聽部屬的Branch是 `production` ，當你push commit到遠端Repo時，執行完測試腳本後就會直接執行部屬腳本，而部屬腳本會先Git Pull目前的Branch至Local。

以下分享一下正在用的腳本，由於我這個專案是在伺服器才編譯CSS,JS，所以多了前面一段設定：

	#----------OpenShift Deployment---------
	# dependencies installation
	npm install --dev
	
	# Start building assets
	npm run build
	
	# You need to set git credentials otherwise it won't be able to commit
	git config --global user.email "codeship@codeship.com"
	git config --global user.name "Codeship Server"
	
	# Add your file
	git add .
	
	# Commit with special --skip-ci command which indicates to codeship to ignore this commit. To avoid infinite build.	
	git commit -m "Codeship update assets --skip-ci"
	
	# Add remote for deployment
	git remote add openshift [YOUR OPENSHIFT APP GIT REPO]
	
	# prevent 'error: failed to push some refs' from happening.
	git fetch --unshallow
	
	# Deploy built assets to OS
	# CI BRANCH is exactly the branch you set to watch.
	git push openshift HEAD:master -f


主要的關鍵是這一段，如果少了這一段就不會成功。

	# Add remote for deployment
	git remote add openshift [YOUR OPENSHIFT APP GIT REPO]
	
	# prevent 'error: failed to push some refs' from happening.
	git fetch --unshallow
	
	# Deploy built assets to OS
	git push openshift HEAD:master -f```


這三行就是將要部屬的Branch上的commit強制合併至OS的Git Repo來觸發整個部屬流程。

有人可能會認為，我們只是簡單地將本地的Repo上傳至OS。

然而中間經過了CS能使我們確保每個commit都能先通過單元測試，才部屬至正式環境。一旦測試環節失敗，將不會繼續進行後面的步驟，包括部屬流程，從而確保正式環境的穩定。

歡迎留言討論。謝謝。

> 此為翻譯文章，加入了一些個人使用經驗與看法。


Reference
========
[Deployment-a-openshift-con-codeship](http://juanpabloaj.com/2014/07/23/Deployment-a-openshift-con-codeship/)