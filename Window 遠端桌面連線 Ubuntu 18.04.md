# Window 遠端桌面連線 Ubuntu 18.04
>[name=陳美芳]
>[time=Mon, Aug 27, 2018 5:52 PM]

## 安裝流程

1. 安裝遠端桌面管理軟體
```shell=
    $sudo apt-get install xrdp
    $sudo apt-get install xfce4
```

2. 配置遠端桌面管理軟體
```shell=
    $echo xfce4-session > ~/.xsession
```

```shell=
    $sudo nano /etc/xrdp/startwm.sh
```
![](https://i.imgur.com/t0FwBQx.png)


3. 重新啟動
```shell=
    $sudo service xrdp restart
```

4. 測試
![](https://i.imgur.com/XXtIWkI.png)

5. 輸入帳密
![](https://i.imgur.com/lvuD3JF.png)


6. 成功
![](https://i.imgur.com/PRpEvxr.png)



## 來源
- [Windows 遠端桌面連線到 Ubuntu 16.04---1](http://honglung.pixnet.net/blog/post/167257893-windows-%E9%81%A0%E7%AB%AF%E6%A1%8C%E9%9D%A2%E9%80%A3%E7%B7%9A%E5%88%B0-ubuntu-16.04)
- [Windows 遠端桌面連線到 Ubuntu 16.04---2](https://tw.saowen.com/a/0e17f8c55367b01872e668e9b36544c3eb47bf719868ef1c75af234f8952611f)
- [Ubuntu 18.04: Install LXDE for desktop environment](https://www.hiroom2.com/2018/05/06/ubuntu-1804-lxde-en/)
- [Ubuntu 16.04: Connect to LXDE desktop environment via XRDP](https://www.hiroom2.com/2017/09/30/ubuntu-1604-xrdp-lxde-en/)
- [Lubuntu Setup Remote Desktop With Xrdp](https://code.luasoftware.com/tutorials/linux/lubuntu-setup-remote-desktop-with-xrdp/)