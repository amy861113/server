# Ubuntu 18.04 安装MySQL數據庫和可視化工具MySQL Workbench

> [name=陳美芳]
> [time=Tue, Aug 28, 2018 5:16 PM]


## 安裝流程

1. 安裝軟體
```shell=
    $sudo apt-get install mysql-server 
    $sudo apt-get install mysql-client
    $sudo apt-get install libmysqlclient-dev
```

2. 檢查是否安裝成功
```shell=
    $ps -aux | grep mysql
```

3. MySQL Workbench下载封包
```shell=
    $wget https://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-workbench-community_8.0.12-1ubuntu18.04_amd64.deb
```
==需要到下列連結去更新下載網址(需註冊)==
- https://dev.mysql.com/downloads/workbench/

4. 安裝 MySQL Workbench
```shell=
    $sudo dpkg -i mysql-workbench-community_8.0.12-1ubuntu18.04_amd64.deb
```

==出現錯誤，因此需要安裝依賴包==
![](https://i.imgur.com/WcvocNB.png)


5. 安裝依賴包，然後重新執行一次安裝
```shell=
    $sudo apt install -f
    $sudo dpkg -i mysql-workbench-community_8.0.12-1ubuntu18.04_amd64.deb
```
:::info
測試前必須先將mysql遠程連接
相關 : [安装MySQL數據庫和可視化工具MySQL Workbench前置作業](https://hackmd.io/VnFZDFLfTJ6uYEn4V9DbHQ)
:::
6. 在Window下，安裝MySQL Workbench 8.0.12
![](https://i.imgur.com/0i3zrOD.png)

7. 安裝後，點開會看到以下圖片
![](https://i.imgur.com/GMU1KPv.png)

8. 新增連線
![](https://i.imgur.com/Fcic4BW.png)

9. 點選新增的連線，輸入登入root mysql的密碼
![](https://i.imgur.com/0uGmuGC.png)

10. 成功連線
![](https://i.imgur.com/PySyzrV.png)



## 來源
- [Ubuntu16.04安装MySQL數據庫和可視化工具MySQL Workbench](https://blog.csdn.net/DreamHome_S/article/details/78137151)
