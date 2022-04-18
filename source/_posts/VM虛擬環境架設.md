---
title: VM虛擬環境架設
date: 2021-11-15 17:00:15
excerpt: 看到這標題就知道你來到虛擬主機的環境了，歡迎來到這個世界(๑•́ ₃ •̀๑)
categories: 
- [虛擬環境]
tags: [ VirtualBox, CentOS]
---

# VM虛擬環境
###### tags: `網頁`

### 安裝
看到這標題就知道你來到虛擬主機的環境了，歡迎來到這個世界(๑•́ ₃ •̀๑)，
本次使用的是VM BOX(Oracle VM VirtualBox)的程式進行安裝，[參考網址](https://yiling0308.pixnet.net/blog/post/358163087)
因為這次要求的是環境CentOS 7.5.1804、PHP5.6、mysql Ver15.1、Distrib 10.3.18-、MariaDB。本篇介紹就會以上述的條件進行安裝，若版本上有所不同可以google相對應的東西去做更改

* ###### 第一步：下載[Virtual Box](https://www.virtualbox.org/)

* 第二步：下載centOS的映像檔，官方網站有版本但可能會針對環境做許多不同版本的更改
   版本資訊可以上網google，這邊推薦幾個用過的[網站](http://ftp.jaist.ac.jp/pub/Linux/CentOS-vault/7.5.1804/isos/x86_64/)、[交大網站](https://centos.cs.nctu.edu.tw/)、[官方網站](https://wiki.centos.org/Download)

* 第三步：開始安裝centOS
  `新增`
  →為你的**虛擬主機取名**和**選取存放資料夾**，系統要選`Oracle`跟小紅帽`redhat`
  →記憶體大小可以根據需求調整，後續仍然可以增加
  →`立即建立虛擬硬碟`
  →選擇`VDI(VirtualBox磁碟映像)`
  →`動態分配`
  →`啟動`
  →選擇下載的CentOS映像檔啟動
  →畫面按 **↑**選擇`Install CentOS 7`
  →移出視窗的方法：**右下方**那個閃閃發亮的**Ctrl**
  →選擇安裝語言
  →選擇`安裝目的地`，`自動配置磁碟分割`，左上角完成
  →`開始安裝`
  →`設定Root密碼`(Root是管理員，擁有最高權限，密碼越安全越好)
  →建一個用戶`給予管理員權限`(管理員權限建議使用Sudo，不要用root)
  →VM`重新開機`後就可以使用了
  →重新開機回到安裝畫面的話，映像檔沒有退出的關係(上方`裝置→光碟機→虛擬磁碟機中移除磁碟`→重新開機)

  進到一片黑色畫面顯示localhost login:代表安裝完成啦!
  輸入使用者帳號後再輸入密碼進行登入

### PHP5.6
  * 第四步：先安裝PHP的環境，這次要安裝的php是5.6版本，我們會需要yum進行輔助，安裝之前我們需要用EPEL跟Remi資料庫來協助我們
  ```cmd
    yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
  ```
  →接下來，安裝`yum-utils`，英文說他是 which is an assortment of utilities that integrate，也就是一個聚合體
  ```cmd
    yum-utils
  ```
  →`yum-utils`最重要的程式之一是`yum-config-manager`，使用他來啟用Remi作為安裝PHP版本的資料庫(repository)選擇，版本會在最後面進行輸入
  使用 YUM-UTILS 的 yum-config-manager 啟用 PHP 7.3 的 Remi repository
  ```cmd
    yum-config-manager --enable remi-php55   [Install PHP 5.5]
    yum-config-manager --enable remi-php56   [Install PHP 5.6]
    yum-config-manager --enable remi-php72   [Install PHP 7.2]
    yum-config-manager --enable remi-php80   [Install PHP 8.0]
  ```
  安裝 PHP
  ```cmd
      yum install php php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo
  ```
  進行 PHP 版本檢查
  ```cmd
  $ php -v
  ```

### mysql ver15.1 (mariaDB安裝)
  * 第五步：進行mariaDB的安裝，mariaDB跟MySQL是通用的，但mariaDB是免費開源的程式，所以通常做為預設安裝的樣式。
  兩種安裝mariadb與mariadb-server套件
  ```cmd
    sudo yum groupinsatll mariadb mariasb-client -y
  ```
  上面是安裝Red Hat Enterprise Linux 7 RH254 Red Hat System Administration III Edition 1 Student Workbook的作法，但有另外一種作法為
  ```cmd
    sudo yum insatll mariadb mariasb-server -y
  ```

  接下來也有兩種啟用MariaDB服務的方式
  ```cmd
    sudo systemctl start mariafb
  ```
  或是讓MariaDB服務會在每次開機自動啟動
  ```cmd
    sudo systemctl enable mariafb
  ```
  
  檢查MariaDB版本
  ```cmd
   mysql -V
   ```
   記得後面V要是大寫
    升級MariaDB
      先停用MariaDB的服務：
      
```cmd
systemctl stop mariadb
```

移除目前安裝的MariaDB舊版本：
```cmd
yum remove mariadb mariadb-server
```

安裝新版本的MariaDB：
```cmd
  yum install mariadb mariadb-server
```

啟動自動啟用MariaDB服務：
```cmd
  systemctl enable mariadb
```

升級原有的Database：
```cmd
  mysql_upgrade -u root -p
```

重啟MariaDB服務：
```cmd
  systemctl restart mariadb
```

### [composer安裝](https://getcomposer.org/download/)
在VM虛擬機中輸入
```cmd
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

分別是(翻譯)
將安裝程序下載到當前目錄
驗證安裝程序 SHA-384，您也可以在此處交叉檢查
運行安裝程序
刪除安裝程序

所以刪除安裝程序前進行安裝
```cmd
sudo mv composer.phar /usr/local/bin/composer
```

-----延伸閱讀
[Windows 7 VirtualBox 安裝](https://shian420.pixnet.net/blog/post/329440572-%E5%BB%BA%E7%AB%8B%E8%99%9B%E6%93%AC%E7%92%B0%E5%A2%83)

