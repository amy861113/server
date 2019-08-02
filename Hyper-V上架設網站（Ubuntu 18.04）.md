# Hyper-V 上架設網站(Ubuntu 18.04)


## 簡單介紹
- NAT Port Mapping:當內部測試用的虛擬機器如果有提供服務如http、smtp、ftp等，此時就必須要將外部IP使用的80 port 對應至內部虛擬IP。

## 安裝步驟

1. 為了讓內部虛擬機可以提供HFS服務(http)，要先設定IP對應內部虛擬port。網路>變更介面卡設定>V-WAN(外部網路)點右鍵內容>共用>設定
![](https://i.imgur.com/Iahrt7H.png)

2. 會看到 1700~1708 ，稍微點開看看，找到 80 port(網頁伺服器常用)，然後點開輸入內部虛擬機的IP。
![](https://i.imgur.com/AKY4iky.png)

## 測試

1. 在網頁上輸入內網的網址或在本機以外的機器輸入本機網址，出現以下圖片。
![](https://i.imgur.com/to9xgZE.png)



## 來源
- [Hyper-V 建立 NAT 及 Port Mapping](http://longfamily.pixnet.net/blog/post/119258120-hyper-v-%E5%BB%BA%E7%AB%8B-nat-%E5%8F%8A-port-mapping)