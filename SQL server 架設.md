# SQL server 架設

## 安裝步驟
1. 下載 mysql5.7
```shell=
 $sudo apt-get install mysql-server
```

2. 設定 root 密碼 (安裝時跳出設定視窗)
:::warning
若設定期間未跳出設定root密碼可使用以下指令

- 初始化資料庫( 第一個問密碼，其他的為了安全性，都按Y)
```shell=
$sudo mysql_secure_installation
```
:::


## 設定遠端SQL


1. 登入 myql
![](https://i.imgur.com/EcLv3He.png)


2. 建立想要遠端的使用者
```sql=
    GRANT ALL PRIVILEGES ON *.* TO '使用者名稱'@'%' IDENTIFIED BY '密碼' WITH GRANT OPTION; 
```
:::success
% : 代表所有電腦都可以訪問連線
:::

3. 檢視系統資料庫有沒有剛剛建立的使用者資訊
```sql=
use mysql;
SELECT DISTINCT CONCAT('User: [', user, '''@''', host, '];') AS USER_HOST FROM user; 
```
![](https://i.imgur.com/WiPHb4X.png)

4. 查看 port
```sql=
    show global variables like 'port';
```

5. 檢視 port
```shell=
    netstat -an | grep 3306
```
:::danger
如果以上步驟都成功，但還是連不上。
1. 開啟設定檔
```shell=
    $sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

2. bind-address = 127.0.0.1 改成 0.0.0.0
![](https://i.imgur.com/AFr0OJW.png)
0.0.0.0是INADDR_ANY地址

3. 重啟sql 服務
```shell=
/etc/init.d/mysql restart
```
:::





## 來源
- https://www.itread01.com/content/1545762662.html
- https://www.raspberrypi.org/forums/viewtopic.php?t=168400