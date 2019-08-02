# 虛擬機設定虛擬靜態ip

## 安裝步驟

1. 修改/etc/NetworkManager/NetworkManager.conf 的managed 參數。
```shell=
$sudo nano /etc/NetworkManager/NetworkManager.conf
```
![](https://i.imgur.com/Jy8yHZX.png)

2. 重新啟動
```shell=
$sudo reboot now
```

3. 修改/etc/network/interfaces檔案,設定靜態IP
```shell=
$sudo nano /etc/network/interfaces
```
```shell=
auto eth0
iface eth0 inet static 
address 192.168.137.10
netmask 255.255.255.0
gateway 192.168.137.1
dns-nameservers 8.8.8.8
```
![](https://i.imgur.com/wtGxBvU.png)



4. 重新啟動服務
```shell=
$sudo service network-manager restart
```

5. 測試結果
![](https://i.imgur.com/8UQrlGx.png)


## 來源
- [VMware Fusion 設定ubuntu16.04虛擬機器靜態ip和DNS](https://www.itread01.com/p/149338.html)
- [Ubuntu系统下的网络配置](http://www.lryb.net/?p=1330)
- [Ubuntu配置DNS令其永久生效的方法](https://blog.csdn.net/solaraceboy/article/details/79037540)



