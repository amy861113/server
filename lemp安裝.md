# lemp安裝

## 版本

- linux: centos 7
- php: 7.3.19
- mysql: 5.5.65-mariadb
- nginx: 1.16.1

## 安裝步驟

:::danger
以下步驟需登入root帳號
:::
1. 安裝epel套件
```shell=
$ yum install epel-release -y
```
2. 安裝nginx
```shell=
$ yum install nginx -y
```

3. 重啟nginx服務
```shell=
$ systemctl start nginx
$ systemctl enable nginx
```

4. 本機網頁輸入[虛擬機ip]檢查是否安裝成功
![](https://i.imgur.com/PeaPeKc.png)

5. 安裝mysql
```shell=
$ yum install mariadb-server mariadb -y
```

6. 重啟mysql服務
```shell=
$ systemctl start mariadb
$ the systemctl enable mariadb
```

7. 重設mysql root預設
```shell=
$ mysql_secure_installation
```
:::info
mysql會問root密碼，因為剛剛安裝mysql，所以直接按[enter]就好了．
之後輸入[y]設定root密碼，因為安全性問題，接下來建議都按[ｙ]
:::

8. 安裝php7.3
```shell=
$ wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm 
rpm -Uvh remi-release-7.rpm
```

9. 啟動php73信息庫，默認為禁用
```shell=
$ yum install yum-utils -y 
$ yum-config-manager-啟用remi-php73
```

10. 安裝php 套件包
```shell=
$ yum --enablerepo=remi,remi-php73 install php-fpm php-common
```

11. 安裝php modules以確保服務正常運行
```shell=
$ yum --enablerepo=remi,remi-php73 install php-opcache php-pecl-apcu php-cli php-pear php-pdo php-mysqlnd php-pgsql php-pecl-mongodb php-pecl-redis php-pecl-memcache php-pecl-memcached php-gd php-mbstring php-mcrypt php-xml
```

12. 更改nginx設定
```shell=
$ nano /etc/nginx/conf.d/default.conf
```
:::info
新增以下文字
```cofig=
server {
    listen   80;
    server_name  your_server_ip;

    # note that these lines are originally from the "location /" block
    root   /usr/share/nginx/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ .php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```
:::

13. 重啟nginx服務
```shell=
$ systemctl restart nginx
```

14. 設定php-fpm
```shell=
$ nano /etc/php-fpm.d/www.conf
```
:::info
更改以下區塊變數：
- user = apache to user = nginx
- group = apache to group = nginx
- listen.owner = nobody to listen.owner = nginx
- listen.group = nobody to listen.group = nginx
;listen = 127.0.0.1:9000
=> listen = /var/run/php-fpm/php-fpm.sock
:::

15. 重啟php-fpm服務
```shell=
$ systemctl start php-fpm.service
$ systemctl enable php-fpm.service
```

16. 防火牆開啟相關服務不(假設centos7有開啟防火牆)
```shell=
$ sudo firewall-cmd --permanent --zone=public --add-service=http
$ sudo firewall-cmd --permanent --zone=public --add-service=https
$ sudo firewall-cmd --reload
```

17. 關閉selinux
 - 修改/etc/sysconfig/selinux
 ```shell=
 $ sudo nano /etc/sysconfig/selinux
 ```
 - 將 SELINUX=enforcing 改成 SELINUX=disabled
 ```config=
 # SELINUX=enforcing
 SELINUX=disabled
 ```
 - 重新開機
 ```shell=
 $ reboot
 ```

18. 設定 nginx，將 .php 的請求轉到 php-fpm 處理
```shell=
$ sudo nano /etc/nginx/nginx.conf
```
:::info
在 server {} 區塊裡面新增以下程式碼:
```config=
location ~ \.php$ {
         fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
         fastcgi_index  index.php;
         fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
         include        fastcgi_params;
}
error_page 404 /404.html;
      location = /40x.html {
}

error_page 500 502 503 504 /50x.html;
      location = /50x.html {
}
```
:::
:::warning
- 只能有一個server區塊，不然將無法運行
- 注意 fastcgi_pass 的部分要和 php-fpm 的設定一樣
:::

19. 檢查nginx設定是否正確
```shell=
$ sudo nginx -t
```

20. 重新載入nginx設定
```shell=
$ sudo nginx -s reload
```

21. 在 /usr/share/nginx/html 裡，新增一隻 php 檔案
```shell=
$ sudo nano /usr/share/nginx/html/info.php
```
:::info
php內容：
```php=
<?php phpinfo(); ?>
```
:::

22. 在瀏覽器網址打上 [ip]/info.php
![](https://i.imgur.com/RdvH8Ab.png)

## 來源
- [how-to-install-lemp-centos7](https://www.hostinger.com/tutorials/how-to-install-lemp-centos7)
- [利用 LNMP (Linux + Nginx + MySQL + PHP) 架設伺服器](https://kim85326.github.io/2019/05/11/%E5%88%A9%E7%94%A8-LNMP-(Linux-+-Nginx-+-MySQL-+-PHP)-%E6%9E%B6%E8%A8%AD%E4%BC%BA%E6%9C%8D%E5%99%A8/)
- [yum安装时出现：Cannot retrieve metalink for repository: epel. Please verify its path and try again](https://www.jianshu.com/p/d485a4b88eb6)
- [mysql之修改sql_mode](https://blog.csdn.net/l631768226/article/details/77105047)