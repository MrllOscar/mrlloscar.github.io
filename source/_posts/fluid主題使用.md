---
title: Hexo - fluid主題
date: 2021-11-03 11:03:31
excerpt: 拜託你不要再壞掉了，我真的很怕阿
categories: 
- [hexo]
tags: [ fluid, Hexo使用]
---

#### 前言
這東西真的搞得我快瘋掉
每次弄這個都失敗，前前後後重新安裝了大概10來次，補上新的東西他就給你跳故障，弄得自己頭超痛

不過！就在今天！終於發現自己錯誤的一個點，特別來記錄一下
![](https://memeprod.ap-south-1.linodeobjects.com/user-gif/53629eede0fb322983836d87096661ac.gif)

# fluid主題使用

其實到目前為止都還在熟悉中，雖然是全中文主題但是根本看不懂他到底在寫什麼阿阿阿！(吶喊)
我只是想要好好地寫一個文章記錄我的文章，結果碰到這個真的是關關難過但關關不說。
所以這邊有[中文參考網站](https://hexo.fluid-dev.com/docs/guide/#%E5%85%B3%E4%BA%8E%E6%8C%87%E5%8D%97)

**注意** 如果再開啟hexo server狀態下執行hexo clean會導致整個檔案的public都被清除！hexo會把整個public資料夾當作已產生的靜態檔案給清除。
[前人痛苦經歷](https://s81679.github.io/2020/02/25/hexo-theme-fluid/)

#### 錯誤事項注意
* [官方文件](https://fluid-dev.github.io/hexo-fluid-docs/start/#%E6%90%AD%E5%BB%BA-hexo-%E5%8D%9A%E5%AE%A2)

> Hexo 5.0.0 版本以上，推荐通过 npm 直接安装，进入博客目录执行命令：
`npm install --save hexo-theme-fluid`
然后在博客目录下创建 _config.fluid.yml，将主题的 _config.yml (opens new window)内容复制过去。

每次看到這段的時候就會直些下意識的把資料夾下方的_config複製一遍然後改檔名，而簡體文獻中 **主題** 跟 **博客** ~~可不可以寫得好分辨一點~~這兩個位置真的很容易讓人搞錯
所以如果你用路徑安裝的話，實際位置會在 `hexo目錄/node_modules/hexo-theme-fluid/_config.yml`
這才會是他這段說要的東西
真的是一步錯步步錯，這東西從我練乙級前(9月)到現在(11月)才搞懂，終於在我快放棄這個主題的時候，開始我的hexo建置了(汗顏)

#### 重建
hexo既然重建那麼多次了。
肯定知道，如果要移植的時候`source`這個資料夾**裡面的東西**記得**全部**複製起來。
他會是你重頭到尾嘔心瀝血所創作出來的所有文章，如果你忘記備份並且他沒事自動上傳到git做紀錄，那你也只能節哀順變了...