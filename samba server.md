# samba server

## 簡介
Samba，是種用來讓UNIX系列的作業系統與微軟Windows作業系統的SMB/CIFS（Server Message Block/Common Internet File System）網路協定做連結的自由軟體。(像是Win10的網路芳鄰)


## 安裝步驟

1. 更新所有
```shell=
 $sudo apt-get upgrade 
 $sudo apt-get update 
 $sudo apt-get dist-upgrade
```

2. 安裝samba server
```shell=
 $sudo apt-get install samba samba-common
```

3. 創造用於samba的目錄
```shell=
 $sudo mkdir -p /home/amy/samba/share
```

4. 建立samba目錄權限
```shell=
 $sudo chmod 777 /home/amy/samba/share
```

5. 配置samba的配置文件
```shell=
 $sudo nano /etc/samba/smb.conf
```
```shell=
[share]
comment = share folder
browseable = yes
path = /home/amy/samba/share
create mask = 0700
directory mask = 0700
valid users = amy
force user = amy
force group = amy
public = yes
available = yes
writable = yes
```
![](https://i.imgur.com/eb15A8L.png)

6. 添加用戶(amy為我的用戶名，之後設定密碼)
```shell=
 $sudo smbpasswd -a amy
```
![](https://i.imgur.com/O9DqWYl.png)

7. 重啟samba server
```shell=
 $sudo service smbd restart
```

## 測試

1. 打開檔案總管>網路>\\ip(要輸入用戶與密碼)
![](https://i.imgur.com/KhhO8tA.png)







## 來源
- https://zh.wikipedia.org/wiki/Samba
- https://www.linuxidc.com/Linux/2018-11/155466.htm