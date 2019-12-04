# swapon常用指令

## 指令

1. 顯示目前的swap
```shell=
 $sudo swapon --show
```

2. 是否曾經有建立swap
```shell=
 $free -h
```
![](https://i.imgur.com/1zrg7oZ.png)



3. 檢查硬盤分區上的可用空間
```shell=
 $df -h #/dev/sda1
```


4. 創建1G到swap
```shell=
 $sudo fallocate -l 1G /swapfile
 $ls -lh /swapfile
```
![](https://i.imgur.com/JSLqZz1.png)


5. 啟動swap資料
```shell=
 $sudo chmod 600 /swapfile #root才能讀取
 $ls -lh /swapfile
 $sudo mkswap /swapfile
 $sudo swapon /swapfile
```
![](https://i.imgur.com/skZfDuG.png)

![](https://i.imgur.com/FPDaEzv.png)

6. 查看
```shell=
 $sudo swapon --show
 $free -h
```
![](https://i.imgur.com/t6djPgm.png)

![](https://i.imgur.com/7dwxlzX.png)

:::warning
以上設定當重新開機時會消失，若想要使swap永久啟動，請更改/etc/fstab
:::





## 來源
- https://man.linuxde.net/swapon
- https://www.opencli.com/linux/linux-free-command
- https://ephrain.net/linux-%E4%BD%BF%E7%94%A8-fallocate-%E6%8C%87%E4%BB%A4%E5%BF%AB%E9%80%9F%E5%BB%BA%E7%AB%8B%E6%8C%87%E5%AE%9A%E5%A4%A7%E5%B0%8F%E7%9A%84%E6%AA%94%E6%A1%88/
- https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04