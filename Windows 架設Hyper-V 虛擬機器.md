# Windows 架設Hyper-V 虛擬機器


## 簡單介紹

Hyper-v 虛擬交換器類型，分為

1. 外部網路: 讓虛擬機器可以與外部網路連線，簡單說就是與host主機共用實體網路卡
2. 內部網路: 可讓Host主機與虛擬機器溝通
3. 私人網路: 讓虛擬機器彼此可以溝通，與外部網路隔離


## 安裝步驟

1. 進入設定
![](https://i.imgur.com/0HaPmEO.png)

2. 點選程式與功能
![](https://i.imgur.com/9hFN3jY.png)

3. 點選開啟或關閉Windows功能
![](https://i.imgur.com/wplWhDO.png)

4. 點選hyper-V，並按確定。
![](https://i.imgur.com/WfTsV3x.png)
![](https://i.imgur.com/2g932Xj.png)

5. 立即重新啟動

6. 進入Hyper-V管理員介面
![](https://i.imgur.com/7lzFr19.png)

7. 選擇電腦後，點選"虛擬交換管理員"，建立虛擬交換器，安裝虛擬機才能連網。
![](https://i.imgur.com/VXWcioC.png)

8. 選擇「外部」，點選「建立虛擬交換器」。這個方式會使用實體網路的IP連結到網際網路，你的連線設定是DHCP，虛擬機器會自動取得IP；如果是使用固定IP，必須自己設定連線的IP，虛擬機器才能上網。
![](https://i.imgur.com/SinXvco.png)
![](https://i.imgur.com/MmviKCK.png)

:::danger

若出現外部網路無法連線的情況，請確定網路共和中心>乙太網路>右鍵內容>確定 Hyper-V Extensible Virtual Switch 沒有勾選。
![](https://i.imgur.com/Kk6hILb.png)

:::


9. 點"套用"時，網路可能不通，點"是"。
![](https://i.imgur.com/qMuNHup.png)

10. 創造一個內部的虛擬交換器，並輸入名稱，點"套用"。
![](https://i.imgur.com/uvww8se.png)

11. 造一個私人的虛擬交換器，並輸入名稱，點"套用"。
![](https://i.imgur.com/0BILCyz.png)


12. 虛擬交換器建立完成以後，點選「新增」，選擇「虛擬機器」，開始建立虛擬機器。
![](https://i.imgur.com/KvFMw5k.png)

13. 打開網路共和中心
![](https://i.imgur.com/GmKbFYs.png)

14. 開啟控制台 > 網路和網際網路 > 網路和共用中心 > 點擊"變更介面卡設定"，會看到剛剛所新增的虛擬網卡。
![](https://i.imgur.com/m3sqVJc.png)

15. 要設定外部(V-WAN)與內部網路(V-LAN)連通，對V-WAN點右鍵，點擊"內容"。
![](https://i.imgur.com/7dM3e5m.png)

16. 選擇共用，勾選[允許其他網路使用者透過這台電腦的網際網路連線來連線]後，選擇要共享訊號的網路卡(這裡就選擇之前所建立的 V-LAN 虛擬網卡)，按[確定]後離開設定視窗。
![](https://i.imgur.com/PYZAXci.png)

17. 接下來，確認是否設定正確，可以檢查之前所建立的 V-LAN 虛擬網卡，是否有取得有效的內部 IP 位置。
![](https://i.imgur.com/hg4uNYh.png)

18. 將 Hyper-V 內的虛擬機器，設定成該 V-LAN 網卡後，按"確定"，此虛擬機就可以連網了。
![](https://i.imgur.com/3BKO3Jh.png)







## 來源
- [Windows 10虛擬機器Hyper-V](https://blog.xuite.net/yh96301/blog/459512721-Windows+10%E8%99%9B%E6%93%AC%E6%A9%9F%E5%99%A8Hyper-V)
- [如何讓 Hyper-V 所建立的虛擬機器，可以使用 NAT 來共享網路？](https://key.chtouch.com/ContentView.aspx?P=2249)