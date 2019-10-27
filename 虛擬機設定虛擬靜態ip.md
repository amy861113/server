# 虛擬機設定虛擬靜態ip

## 安裝步驟


:::danger
必須設定雙網卡 :
![](https://i.imgur.com/qKADV9J.png)
:::
1. 網路卡命名為eth0...
```shell=
 $sudo nano /etc/default/grub
```
:::info
GRUB_CMDLINE_LINUX="" >> 「GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
:::

2. 刷新且更新
```shell=
 $sudo grub-mkconfig -o /boot/grub/grub.cfg
 $sudo update-grub
```

3. 確認/etc/NetworkManager/NetworkManager.conf 中之 managed=false
```shell=
 $cat /etc/NetworkManager/NetworkManager.conf
```


4. 修改"/etc/sysctl.conf"，將# net.ipv4.ip_forward=1此行的# (註解)拿掉
```shell=
 $sudo nano /etc/sysctl.conf
```

5. 將"/proc/sys/net/ipv4/ip_forward"的值修改為1
```shell=
 $sudo nano /proc/sys/net/ipv4/ip_forward
```

6. 修改/etc/network/interfaces檔案,設定靜態IP
```shell=
$sudo nano /etc/network/interfaces
```
```shell=
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
address 192.168.137.11
netmask 255.255.255.0
network 192.168.137.0
broadcast 192.168.137.255


```
![](https://i.imgur.com/jfTJsiD.png)




7. 修改host檔案
```shell=
 $sudo nano /etc/hosts
```

8. 重啟
```shell=
 $reboot
```

## 測試結果
![](https://i.imgur.com/8UQrlGx.png)


## 來源
- [VMware Fusion 設定ubuntu16.04虛擬機器靜態ip和DNS](https://www.itread01.com/p/149338.html)
- [Ubuntu系统下的网络配置](http://www.lryb.net/?p=1330)
- [Ubuntu配置DNS令其永久生效的方法](https://blog.csdn.net/solaraceboy/article/details/79037540)



