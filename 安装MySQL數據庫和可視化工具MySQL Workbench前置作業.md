# 安装MySQL數據庫和可視化工具MySQL Workbench前置作業

## 設定流程

1. 登入mysql
```shell=
    $sudo mysql -u root -p
```

2. 成功登入
![](https://i.imgur.com/ZfqdXfW.png)

3. 選擇mysql數據庫
```sql=
    use mysql;
```
![](https://i.imgur.com/AvM0yMM.png)

4. 授權給其他用戶
```sql=
    grant all privileges on *.* to root@"%" identified by "password" with grant option;
```

==\*.* -> 第一個星星=數據庫名，第二個星星=數據庫表名==
==password替換成你的root mysql密碼==

5. 刷新權限訊息
```sql=
    flush privileges;
```

6. 查看當前允許連接用戶
```sql=
    select user,authentication_string,host from user where user = 'root';
```

7. 退出mysql
```sql=
    exit;
```

8. 更改配置信息
```shell=
    $sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf /
```
![](https://i.imgur.com/B46plli.png)

bind-address = 127.0.0.1 -> #bind-address = 127.0.0.1 

9. 重啟服務
```shell=
    $sudo /etc/init.d/mysql restart
```



## 來源
- [Ubuntu 開啟 MySQL 遠端連線的設定方法](https://blog.csdn.net/qq_29738785/article/details/54667094)