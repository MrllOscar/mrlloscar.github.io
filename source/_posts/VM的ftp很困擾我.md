---
title: VM的ftp很困擾我
date: 2021-11-05 15:24:40
categories: 
- [虛擬環境]
tags: [530, VirtualBox, CentOS]
---

[你在找我嗎?](https://iter01.com/367794.html)
公司因為專案是利用ftp去做檔案修改的，雖然有討論過git的問題，但似乎因為某些因素git只拿來做版本整合
詳細情況也不清楚，總之目前就是從git上面拉下來一個專案，但是因為無法模擬出實際測試過後的結果，所以必須藉由VM的虛擬主機來模擬環境
因此又必須要先前一開始進公司的環境測試環節，那部份我卡了很久最後也是靠長官拯救才處理完成，頂多開始了解什麼東西是虛擬主機他是來幹嘛的，但詳細設定還是不會

廢話到這，**重點**都用這些標示了

### FTP與SFTP設定
如開頭連結一開始有網址可以查詢設定
* 系統Linux的CentOS7環境
##### FTP

1. 安裝啟動FTP服務

1.1 centos**安裝vsftpd**服務

使用 yum 安裝 vsftpd
`yum install -y vsftpd`

1.2啟動vsftpd
安裝完成後, **啟動vsftpd**服務 :
`service vsftpd start`

啟動後, 可以看到系統已經監聽了 21 埠(Ubuntu下命令為: lsof -i:21)
`netstat -nltp | grep 21`

2. 配置FTP許可權

2.1 瞭解 vsftpd 配置
vsftpd 的配置目錄為 /etc/vsftpd, 包含下列的配置檔案 :
        a. vsftpd.conf 為主要配置檔案
        b. ftpusers 配置禁止訪問 FTP 伺服器的使用者列表
        c. user_list 配置使用者訪問控制

2.2 阻止匿名訪問和切換根目錄
匿名訪問和切換目錄都會給伺服器帶來安全風險, 我們把這兩個功能關閉.
編輯 /etc/vsftpd/vsftpd.conf, 找到下面兩處配置並修改:

* 禁用匿名使用者  YES 改為NO
anonymous_enable=NO

* 禁止切換根目錄 刪除或#
chroot_local_user=YES

編輯完成後儲存配置, **重新啟動 FTP** 服務
`service vsftpd restart`

2.3 建立 FTP 使用者

**建立**一個使用者 ftpuser
`useradd ftpuser`

為使用者 ftpuser **設定密碼**
`passwd ftpuser`  或  echo “new_password” | passwd ftpuser –stdin

2.4 限制該使用者僅能通過FTP訪問

限制使用者 ftpuser 只能通過 FTP 訪問伺服器, 而不能直接登入伺服器

usermod -s /sbin/nologin ftpuser

 

2.5 為使用者分配主目錄

為使用者 ftpuser建立主目錄並約定
/data/ftp 為主目錄, 該目錄不可上傳檔案
/data/ftp/pub 檔案只能上傳到該目錄下
在/data中建立相關的目錄
mkdir -p /data/ftp/pub

**設定訪問許可權**
`chmod a-w /data/ftp && chmod 777 -R /data/ftp/pub`

**設定為使用者的主目錄**
`usermod -d /data/ftp ftpuser`

3、OK

至此, FTP服務已經搭建完成, 可以使用各種第三方客戶端來測試訪問FTP伺服器
訪問前, 記得**關閉防火牆**
`service firewalld stop`

~~如果需要使用root登入連線FTP服務, 需要配置 /etc/vsftpd/user_list 和 /etc/vsftpd/ftpusers, 將檔案中的root註釋~~


之後的重點就是開機 `service firewalld stop` 跟 `service vsftpd start` 兩個記得使用就好

目前仍然出現的問題還在研究...
- [ ] 1.VM開機自動啟動vsftpd 的server
- [ ] 2.針對80 port或是特定的port做指定開啟或關閉
- [x] 3.vsftpd config產生的530登入錯誤問題

希望之後能趕快把這一塊更新完成再補上完整的東西( ´•̥̥̥ω•̥̥̥` ) 

---

2021.11.22 上網找到一個[方法一](https://iter01.com/557032.html)去解決`回應: 530 Login incorrect.`的問題
針對`/etc/pam.b/vsftpd`進行`vim`修改，將`auth required pam_shells.so`跟`auth include password-auth`進行註解


還有一個[方法二](https://www.twblogs.net/a/5d074c4dbd9eee1ede039a0a)是將`auth required pam_shells.so`改成`auth required pam_nologin.so`

這是目前解決的兩種方法，但利用網路磁碟機的方法的時候，他一樣會沒辦法連線到....所以肯定還有另外的解決方式才對

問了其他學長，也有提供sftp登入就不會有這個問題的用法，可能等以後再試試看...
雖然搞不懂原因，但至少成功可以將檔案呈現在FileZilla了

---

2021.12.16 這一天比較有空，所以又研究了一次這個問題

加上這次主管有空來提點了**samba**這個問題，終於讓我找到一點頭緒

FTP的問題，透過上面註解是可以解決

但是連線網路磁碟機是沒辦法連線成功的，找了很久於是找到了[這個](https://josephjsf2.github.io/linux/2019/11/01/share_centos_folder_with_windows.html)

這位Joseph大大講得非常詳細，我們的VM安裝的時候是*透過samba讓linux跟windows做溝通的*

然後這次用的環境是**CentOS**，這個關鍵字不能忘(怎麼突然想到XXXX烤香腸的旋律)

CentOS上安裝Samba

`yum install samba samba-client samba-common -y`

建立目錄

設定

`vim /etc/samba/smb.conf`

```
[global]
	workgroup = SAMBA
	netbios name = SAMBA_NETBIOS
	server string = SAMBA SERVER
 
	log file = /var/log/samba/log.%m   
	max log size = 500    
	 
	security = user
	passdb backend = tdbsam

 
[ShareFolder]
	comment =Shared
	path = /home/joseph/SharedFolder
	browseable = yes
	writable = yes
 
	create mode = 0660
	directory mode = 2770
 
	valid users = root   # 必須是Linux上存在的user
```

>底下ShareFolder是針對自己專案去取名稱的
>
>其中裡面的設定相關
>
>workgroup：工作群組名稱
>
>netbios name：SAMBA netBIOS name
>
>server string：Server 描述說明
>
>log file：SAMBA log 位置，可傳入參數，如%m表示Client端 NetBIOS名稱
>
>max log size：表示SAMBA log檔最大SIZE，單位為KB，Samba會定期檢查並將超過大小的檔案作rename的動作，如加上 .old extension
>
>security：user 表示需要在主機上有帳號才可登入使用
>
>passdb backend：決定使用何種方式儲存user與group資訊，設定為tdbsam表示以TDB based password方式儲存，可傳入第二個optional參數表示passdb.tdb檔案欲存放位置，預設會在private目錄(${prefix}/private)
>
>​ 如passdb backend = tdbsam:/etc/samba/private/passdb.tdb
>
>​ 預設路徑於 /var/lib/samba/private/passdb.tbd，private目錄為隱藏目錄，且必需要有足夠權限才可進入與檢 視
>
>[SharedFolder]：表示在網路芳鄰上顯示的名稱，smb.conf中可以存在多個 [ ]區塊，表示不同的分享目錄與設定
>
>comment：註解
>
>path：欲共享的目錄路徑
>
>browsable：
>
>writable：是否可寫入，如果設為no，Client端將沒辦法對資料夾或檔案做寫入的動作
>
>create mode：從Client端建立檔案後，建立之檔案權限會設定為create mode的值
>
>directory mode：從Client端建立目錄後，建立之目錄權限會設定為directory mode的值
>
>valid users：可登入SAMBA的帳號白名單列表，可直接指定帳號清單，或是以 @開頭表示可使用的群組，指定的user一定要是系統上存在的帳號，否則是無效的
>
>​ 如 root, @user 表示允許root帳號與 @user群組中的帳號使用

設定開機啟動

```
systemctl enable smb
systemctl enable nmb
```

啟動Samba服務

```
systemctl start smb
systemctl start nmb
```

新增防火牆規則，允許外部可透過SAMBA連線至主機內存取目錄

```
firewall-cmd --permanent --zone=public --add-service=samba
firewall-cmd --reload
```

關閉SELinux(目前尚未能清楚知道關閉之原因，或是其實需要做什麼設定，受陷於目前不清楚SELinux，故只能先關閉)，目前未關閉SELinux情況下，Windows會沒權限無法存取SharedFolder內容

```
setenforce 0  # 暫時關閉，會於下一次登入時失效

vi /etc/selinux/config

SELINUX=disable #永久關閉SELINUX
```

然後開啟的時候如果碰到權限問題，就是chmod沒開(上面有寫)

`chmod a-w /data/ftp && chmod 777 -R /data/ftp/pub`

其中路徑記得改為自己路徑

全部設定完成之後，windows網路磁碟機就能完美的連線成功啦！！

終於，花了好多時間，解決了我心頭的大問題(灑花)

太棒了！今天又比之前學到更多東西了呢(˘•ω•˘)
