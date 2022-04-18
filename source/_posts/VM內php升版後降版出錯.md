---
title: VM內php升版後降版出錯
date: 2022-01-11 16:48:22
index_img: /img/VMerror3.png
categories: 
- [虛擬環境]
tags: [ 疑難雜症 , VirtualBox, CentOS]
---

# PHP 升版後降版(7.2.5 => 7.1.33)

#### 起因
一開始<font color=#008000>為了測試php artisan migrate是否能夠成功</font>，於是在VirtualBox上面執行測試

因為VM環境的關係很多資料並沒有與正式機對上(也可能大家欄位都沒照規範)

先跳出了<font color=#FF0000>錯誤</font>
```sql
SQLSTATE[42S21]: Column already exists: 1060 Duplicate column name 'xxxxx' (SQL: alter
table `XX_xxxxx` add `xxxxx` tinyint not null default '0' comment '(⁎˃ᆺ˂)(╬ Ò ‸ Ó)(ꐦ ಠ皿ಠ )( >д<)(-`д´-)')

[PDOException]
SQLSTATE[42S21]: Column already exists: 1060 Duplicate column name 'xxxxx'
```
等等的錯誤，提醒你你的表格已有但是你在migrate的資料表內沒有這個檔案紀錄

於是就<font color=#0000FF>手動加加減減修正了他</font>。

再多番<font color=#FF6600>校正</font>之後，成功執行了`php artisan migrate`卻無法執行`php artisan migrate:rollback`

然後多次測試後他吐出一個<font color=#0000FF>訊息</font>

```cmd
composer detected issues in your platform:
Your Composer dependencies require a PHP version">= 7.2.5" . You are running 7.1.33.

PHP Fatal error:...
```

![](/img/VMerror1.png)

之後連`php artisan migrate`也變成

![](/img/VMerror2.png)

其實google會發現有很多[方法1](https://www.codegrepper.com/code-examples/php/Composer+detected+issues+in+your+platform%3A+Your+Composer+dependencies+require+a+PHP+version+%22%3E%3D+7.2.5%22.)、[方法2](https://stackoverflow.com/questions/65339404/composer-detected-issues-in-your-platform-your-composer-dependencies-require-a)可以幫忙解決問題

但偏偏我也不知道我那時候是腦充血還是英文能力退化成幼稚園，我就直接對著數字*7.2.5*、*7.1.33* 感到興奮

一陣<font color=#FF0000>腦充血</font>就進行了	<font color=#FFD700>PHP版本升級</font>。

不升級還好，<font color=#FF0000>一升級發現版本跟自己專案不符</font>，還沒搞清楚升降版的問題的時候google到[方法](https://stackoverflow.com/questions/54117054/complete-uninstall-and-re-install-of-php-on-centos-7/54118089)<font color=#FF0000>直接降版</font>

但其中的`sudo yum install mod_php70u php70u-cli php70u-mysqlnd`等等的安裝指令一直失敗

所以我當中跳去搜尋<font color=#800080>其他安裝方法</font>

## 接下來悲劇就發生了

因為	<font color=#FF00FF>連自己移除了什麼東西都不知道</font> `sudo yum remove --setopt=clean_requirements_on_remove=1 php php-pear php-mysql php-cli php-common mod-php`

安裝完之後一股腦兒的衝阿衝(Ò 皿 Ó ╬)，直接將php7.1.33版本安裝之後就啥都不管的瘋狂測試

想當然，最後因為`php7-fpm-nginx php7-cli php7-mysqlnd`等等的套件都沒裝，問題百出

先是<font color=#00FF00>入口頁面顯示</font>

```cmd
502 Bad Gateway nginx/1.20.1
```

然後google找歪找到<font color=#00FF00>問題以為是</font>

```cmd
httpd[9079] (98)Address already in use+centos
```

開始對他<font color=#FF0000>瘋狂的輸出，在連續使用</font>

```cmd
sudo service httpd stop/start
sudo service nginx stop/start
```

的連擊拳想迫使他恢復正常，但VM就像上路大魔王一樣無動於衷

<font size=1px>哭阿</font>

但在這之中`Samba`還是正常運作所以還是可以對檔案進行CRUD

在經過多方詢問後得到答案

![](/img/VMsuccess1.png)
![](/img/VMsuccess2.png)
![](/img/VMsuccess3.png)

當然也有這種答案

![](/img/VMerror3.png)

~~而這位還是當初教學我php的某位長輩...我真不知道我怎麼進化成現在這樣的~~

最後終於在兩天上班時間的閒暇時間中檢查找到問題，並且記得開啟他們

```
sudo systemctl restart php-fpm
sudo systemctl restart nginx
sudo systemctl restart supervisord
```

* 這邊順便補充如果要常駐開啟就是`systemctl enable ?????`

在成功開啟`php-fpm`之後得到了`Composer detected issues in your platform: Your Composer dependencies require a PHP version ">= 7.2.5".`

這也是<font color=#00FF00>版本升降後衍生的問題</font>

參考了網路的[文章](https://stackoverflow.com/questions/65339404/composer-detected-issues-in-your-platform-your-composer-dependencies-require-a)終於找到方法

只是<font color=#FF6600>明明人家說</font>

```
Follow this trick

add this line in config object of composer.json file

"platform-check": false

run php artisan config:cache

then run composer dump-autoload in terminal
```

我卻看到`"platform-check": false`後，又不知道哪來的衝動

<font color=#FF6600>僅執行</font>

```
composer config -l -g
composer config -g platform-check false
```

就要他給我結果，就像主管跟你說：「這個專案很急趕快弄，等等給我？」

你問主管說：「多久要？」

然後主管回你說：「兩秒後」

一樣的感覺ಠ_ಠ

在這邊跟被我凌虐的linux說聲抱歉இдஇ

執行完上面的忽略檢查之後，要執行`php artisan config:cache`跟`composer dump-autoload`之後

<font color=#0000FF>網頁才會恢復正常，真是經歷了一波大災難</font>

#### 結尾

只能說碰到問題的時候要更謹慎的處理，一股腦兒的衝可能越來越恐怖

最好去看過的網頁都要留下紀錄，以免之後想要翻自己做了什麼都找不到

這次也是很幸運我覺得刪除東西會有問題，把網頁加入最愛，來找到我remove的問題

才能順利把這個問題有方向性的解掉

當然如果自己多碰幾次說不定就能知道自己錯在哪了 ಥ⌣ಥ

為了專案執行，先前也有備份一台備用的VM，避免自己工作上的進度delay

才能順利完成分配工作的狀況下順便處理問題

<font color=#FF0000>備份的好習慣可是很重要的</font>

最後希望以後碰到這個問題的時候還想的到以前碰到這些，或是正在看這篇文章的你能得到一點幫助

一起進步(˘•ω•˘)