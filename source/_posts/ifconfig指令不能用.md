---
title: ifconfig指令不能用
date: 2021-11-15 14:21:47
categories: 
- [虛擬環境]
tags: [ VirtualBox , Linux , CentOS , 虛擬機 ]
---
# Linux環境安裝

重頭開始安裝後，發現`ifconfig`

這指令完全無法使用，於是上網google找資料
<!-- more -->

起初是因為想要練習虛擬機上面可不可以自己架起一個環境存放資料

如果可以的話，不像windows的圖形介面多多少少能抵擋一些電腦基礎的使用者開到一些想要加密的檔案或權限不同的檔案

例如之前待在某個社區，所有的會議資料，預算金額都直接放在桌面上門戶大開，其實現在回想起來真的捏一把冷汗

再跟著google的大神們指導下，從安裝oracle vm virtualbox

到安裝PHP版本，mariaDB資料庫，經過了許多波折，還碰到一個目前還沒辦法解決掉的vsftpd的config設定讓FileZilla會530無法連線(網路文章太多不知道哪筆才對)

這次近來是`ifconfig`指令不能使用，其實這東西可以用`ip addr`來找ip

內文中還有一個wget指令，但目前還不知道作用，所以沒去使用

`ifconfig`指令不能用的原因是它是附屬於其他套件底下的，要查詢哪個套件的話

就要輸入指令`yum search ifconfig`或`yum search all ifconfig`

除了一些python的東西外你會看到一個叫做net-tools的套件

這就是你要安裝的東西了，詳細資訊可以在看看這個[參考網站](https://blog.yowko.com/ifconfig-command-not-found/)