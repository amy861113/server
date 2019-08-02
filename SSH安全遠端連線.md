# SSH安全遠端連線
> [name=陳美芳]
> [time=Thu, Aug 2, 2018 10:29 AM]

## 簡介

### ssh基本功能

- 分享Unix Like主機的運算能力
- 利用sftp傳送檔案或資料夾到其他伺服器



## 基礎安裝
### 安裝流程

1. 更新
```shell=
    $sudo apt-get update
```

2. 安裝openssh-server
```shell=
    $sudo apt-get install openssh-server
```

3. 查看是否執行ssh-server
```shell=
    $ps -aux | grep ssh
```
![](https://i.imgur.com/f7KrTyp.png)

4. 設定sshd_config內容
```shell=
    $sudo nano /etc/ssh/sshd_config
```

==ubuntu本身就有限制root的登入，但以防萬一，還是更改下面這行==
#PermitRootLogin Yes --> PermitRootLogin no


==可以更改port，可以查詢沒有使用過的port==
#What ports, IPs and protocols we listen for (約第4、5行)
Port 22 

- 查詢port
```shell=
    $sudo less /etc/services 
```

5. 限制ip登入
```shell=
    $sudo nano /etc/hosts.allow
```
==只適合固定ip==
sshd:xxx.xxx.xxx.xxx :allow

6. 拒絕ip登入
```shell=
    $sudo nano /etc/hosts.deny
```
==設定完就會讓只有你允許的ip登入==
sshd:all:deny

7. 重新啟動ssh
```shell=
    $sudo /etc/init.d/ssh stop
    $sudo /etc/init.d/ssh start
```

:::info
不同於apache2可以restart，所以只能stop跟start，而且無論程式是否支援restart，stop跟start一定會有的。
:::

## 進階安裝
### 設定sftp檔案傳輸

1. 創立sftp訪問用戶群組
```shell=
    $sudo addgroup sftp-users
```

==方便管理權限==


2. 創立ssh用戶群組
```shell=
    $sudo addgroup ssh-users
```

3. 用戶加入群組
```shell=
    $sudo usermod -G sftp-users -s /bin/false 用戶名 //將用戶從其他群組刪除，並加入sftp-users
    $sudo usermod -a -G ssh-users 管理者名 //將管理者加入群組，-a參數代表不從其他群組中刪除
```

4. 創建可讓sftp用戶傳輸檔案的目錄
```shell=
    $sudo mkdir /home/sftp_root
    $sudo mkdir /home/sftp_root/shared
    $sudo chown admin:sftp-users /home/sftp_root/shared
    $sudo chmod 770 /home/sftp_root/shared
```

5. 更改設定檔，鎖定我們的用戶
```shell=
    $sudo nano /etc/ssh/sshd_config
```
Subsystem sftp /var/lib/openssh/sftp-server -> #Subsystem sftp /var/lib/openssh/sftp-server
增加 Subsystem sftp internal-sftp

補充:
AllowGroups ssh-users sftp-users
Match Group sftp-users
    ChrootDirectory /home/sftp_root
    AllowTcpForwarding no
    X11Forwarding no
    ForceCommand internal-sftp
==以上內容:只允許ssh-uers及sftp-users通過SSH訪問系統；針對sftp-users用戶，額外增加一些設置：將“/home/sftp_root”設置為該組用戶的系統根目錄（因此它們將不能訪問該目錄之外的其他系統文件）；禁止TCP Forwarding和X11 Forwarding；強制該組用戶僅僅使用SFTP==


6. 重新啟動
```shell=
    $/etc/init.d/ssh restart
```

## 來源
- [ubuntu SSH遠端安全連線安裝及設定](http://blog.udn.com/nigerchen/2262865)
- [sftp-1](https://www.linuxidc.com/Linux/2017-06/144993.htm)
- [sftp-2](https://45squared.com/setting-sftp-ubuntu-16-04/)
- [ubuntu 使用sftp限制新增使用者，只能登入所屬的home目錄](http://gn01816565.pixnet.net/blog/post/101823148)
