---
title: nginx 新增port連線到不同頁面
date: 2022-01-22 16:13:41
index_img: /img/2022-01-22 161712.png
excerpt: 最後只是一個小小的東西沒有打開，害我花了好多時間(๑•́ ₃ •̀๑)
categories: 
- [疑難雜症]
tags: [ VirtualBox, CentOS, nginx, php-fpm]
---

# Nginx

其實最後的<font color="FF3333">故障點</font>如果不是<font color="FFFF33">虛擬機(nginx)沒有重新啟動</font>，大概就是<font color="FFFF33">防火牆沒有成功開通了</font>

nginx的介紹在網路上相信很多人都有看過，簡單來說他跟apache的類型相似

>Nginx：
>>注重高效能、高開發性
>Apache：
>>目前民間最為廣泛使用的Web Server
>IIS：
>>微軟所製的Web Server，支援完整，但僅能在Windows系統下使用。

相關的比較應該[google](https://www.google.com/search?q=nginx+apache+%E6%AF%94%E8%BC%83&rlz=1C1CHBD_zh-TWTW975TW975&sxsrf=AOaemvKOS04ZcHSpaNZuOzZAUjYg7K7Hig%3A1642830820076&ei=5JvrYZuPBI3L-Qbi47SYDw&oq=nginx+apa&gs_lcp=Cgdnd3Mtd2l6EAMYATIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQyBQgAEIAEMgUIABDLATIFCAAQywEyBQgAEMsBMgUIABDLATIFCAAQywE6BAgjECc6BAgAEENKBAhBGABKBAhGGABQAFjrDGDgGWgAcAJ4AIABPYgB3AGSAQE0mAEAoAEBwAEB&sclient=gws-wiz)會給你很多建議

nginx的設定在網路上千萬種，因為centOS 7 所以有嘗試將範圍縮小，但其實差不多

關於[nginx的基礎設定](https://blog.hellojcc.tw/nginx-beginner-tutorial/)這一篇就很值得看

雖然ubuntu不是我們的環境，但它裡面除了安裝改使用yum之外其他都差不多

關於nginx：
```
http {
# server 一定在 http 裡面
server {
# listen 是個 simple directive
# 對 server 這個 block directive 作設定
# 指定它監聽 port 80
listen 80;
# location 一定在 server 裡面
location {}
}
}
```
這就是`/etc/nginx/nginx.conf`內設定的規則

proxy server這塊沒有特別做設定，所以我就不再關公面前耍大刀了

而[nginx上面要執行php就要靠php-fpm](https://tec.xenby.com/20-nginx-%E8%88%87-php-fpm-%E9%81%8B%E4%BD%9C%E4%BB%8B%E7%B4%B9%E8%88%87%E8%A8%AD%E5%AE%9A%E8%AC%9B%E8%A7%A3)了*當然也許有其他東西我只是舉例*

<font color="996633">nginx</font> 是一個 web server、一個靜態檔案伺服器

<font color="996633">php-fpm</font> 是 FastCGI Process Manager 的縮寫、接收特定 request 並且運行 php 腳本產生結果

沒有一定要用於 php，可以依據情況將 request 丟給其他程式語言處理，或是將 request 丟給不同的 php 版本處理

```
server {
    gzip on;
  
    location / {
        try_files $uri $uri/ =404;
    }

    location ~ .*\.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.2-fpm.sock;
    }
}
```

這個就是php-fpm的用法，其中<font color="FFFF33">gzip是壓縮</font>，這就是另外一[part](https://www.ltsplus.com/linux/nginx-enable-gzip)了

![](/img/2022-01-22 161712.png)

這就是我自己寫的部分，一樣location \根目錄做try_files，底下在寫一個location去對php做編譯

這樣才能成功顯示出自己的網頁

![](2022-01-22 170817.png)

最後的最後，幾個需要注意的點

檢查狀態
`sudo nginx -t`

重新執行*starting new worker processes with a new configuration, graceful shutdown of old worker processes*
`sudo nginx -s reload`

檢查port
`netstat -lnt`

#### [防火牆配置](https://iter01.com/566910.html)
```
yum install -y firewalld 
systemctl enable firewalld        #開機啟動
systemctl start firewalld        #啟動
```

#開放埠
```
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
```

#流量轉發
```
firewall-cmd --add-forward-port=port=80:proto=tcp:toport=8800 --permanent
firewall-cmd --add-forward-port=port=443:proto=tcp:toport=4433--permanent
```

#重啟防火牆讓配置生效
`firewall-cmd --reload`

確認nginx狀態
`sudo systemctl status nginx`

重新啟動nginx及php-fpm
```
sudo systemctl restart nginx
sudo systemctl restart php-fpm
```

最後可以看看這篇認真的[教學](https://fuglemanpeter.blogspot.com/2017/09/centos-7-nginx-mariadb-php.html)