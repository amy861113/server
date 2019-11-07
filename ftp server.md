# ftp server

## 簡介

文件傳輸協議 （ 英文 ： File Transfer Protocol ，簡稱為 FTP ）是用於在 網路 上進行文件傳輸的一套標準協議。它屬於 網路協議組 的 應用層 。

FTP是一個8位的客戶端-伺服器協議，能操作任何類型的文件而不需要進一步處理，就像MIME或Unicode一樣。 FTP 是屬於 TCP 服務的一種， FTP 是所有通訊協定裡最特殊的，其他的通訊協定例如 HTTP 、 SMTP 、 POP3... 都只需要一個通訊埠，然而 FTP 卻需要兩個通訊埠:

1. 一個用來傳遞客戶端與伺服器之間的命令，一般設在 port 21 ，稱之為命令通訊埠 (Command Port) 。

2. 一個是真正用來傳遞資料的，一般都設在 port 20 ，稱之為資料通訊埠 (Data Port) 。
:::info
有兩種設定:
- Anonymous(匿名下載)
- Authenticated(實體帳號)
:::

## 安裝步驟

1. 安裝server
```shell=
 $sudo apt-get install  vsftpd 
```

:::success
內定 vsftpd 設定為只允許 anonymous下載。安裝後 ftp 使用者下載目錄為 /home/ftp，但若想改成別的檔案可作以下步驟:

若要改為其他目錄如 : /srv/ftp
```shell=
 $sudo mkdir /srv/ftp 
 $sudo usermod -d /srv/ftp 使用者名
 $sudo /etc/init.d/vsftpd restart
```
:::

2. 查看是否啟動成功
```shell=
 $sudo service vsftpd status
```
![](https://i.imgur.com/w4z1rKY.png)

:::danger
假設出現Active: failed (Result: exit-code) since Sat 2018-11-03 20:36:08 CST; 1min 53s ago錯誤
輸入以下指令:
```shell=
 $sudo openssl req -x509 -nodes -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem -days 365 -newkey rsa:2048
 $sudo service vsftpd restart
```
:::


3. 編輯/etc/vsftpd.conf
```shell=
 $sudo nano /etc/vsftpd.conf
```
```shell=
local_enable=YES 
write_enable=YES
local_umask=000
chroot_local_user=YES
chroot_list_enable=YES 
chroot_list_file=/etc/vsftpd.chroot_list #一列只能列一個使用者
```


4. 建立/etc/vsftpd.chroot_list檔案
```shell=
 $sudo nano /etc/vsftpd.chroot_list
```
:::danger
在裡面新增你要的使用者，一行一個使用者
:::

5. 設定檔案夾權限
```shell=
 $sudo chown 使用者名:ftp -R 想要ftp可以使用的目錄  
```


5. 重啟server
```shell=
 $sudo /etc/init.d/vsftpd restart
```

## 測試

1. 利用fileZilla
![](https://i.imgur.com/R0mXg6Z.png)


## 來源
- http://irw.ncut.edu.tw/peterju/course/network/971/doc/homework/02/FTP.htm
- https://www.itread01.com/ycfxi.html