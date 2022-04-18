---
title: VM網頁無法執行，卻可修改內容
date: 2021-12-23 14:54:15
index_img: https://picsum.photos/250/250/?random=12
categories: 
- [虛擬環境]
tags: [ 疑難雜症 , VirtualBox, CentOS]
---

昨天為了執行vue跟laravel並行去改了一個VM裡面的`/etc/nginx/nginx.conf`的檔案

但因為很急所以一開始用`mkdir`創了一個`nginx.conf`的**資料夾**，然後用`rm -rf`刪除了這個目錄之後再用`touch`去創了這個文件
結果產生了一個`nginx.conf.ewq`之類的隱藏檔案~~是不是ewq不確定~~ 

最後造成我VM打開啟動不了自己的網頁，卻可以進入VM修改檔案

看來以後看到隱藏檔案出現出錯的時候還是要小心刪除(˘•ω•˘)