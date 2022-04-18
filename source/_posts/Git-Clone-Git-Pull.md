---
title: Git Clone / Git Pull
date: 2021-12-02 16:47:22
excerpt: 還有切換branch的差異
index_img: /img/S__67633243.jpg
categories: 
- [Git]
tags: [ Git, Vscode]
---
# <font color=#FF6600>Git Clone</font>

![](/img/S__67633243.jpg)

`git clone <url>`是把整個路徑拉下來，在vscode的powershell裡面，會在位置內多建立一個資料夾拉下你的專案

由於整個專案拉下來，所以你會看到很多不同分支

<font color=#0000FF>**Q**</font>:可是我拉下來看 `git branch` 蝦咪巄博

因為`git branch`是只能看到自己目前本地的分支的

所以為了確認遠端的分支，指令應該是`git branch -a`

這樣就可以看到整個專案出現的所有分支囉

接下來用`git checkout <branch>`就可以切換到遠端的分支了

同時你的vscode也會有所改變

`git pull <name> <url> <branch>` 因為有設定branch就是只拉了那一個分支下來去做處理

自然無法像clone一樣隨意切換專案分支，但是抓東西下來比對的時候還是pull會方便一些
