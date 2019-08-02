# Hyper-V 安裝 Ubuntu 18.04


## 安裝步驟

1. 新增虛擬機
![](https://i.imgur.com/6tj15qP.png)

2. 點"下一步"。
![](https://i.imgur.com/rZugmwC.png)

3. 輸入名稱，並選擇是否要更改儲存位置。
![](https://i.imgur.com/n5icFLq.png)

4. 選擇世代，因為第一代較為廣泛，因此選擇"第一代"。
![](https://i.imgur.com/mc9aIVE.png)

5. 依照你的電腦實體記憶體的大小，調整虛擬機器記憶體的大小（建議值：大約是實體記憶體的三分之一到二分之一），點選"下一步"。
![](https://i.imgur.com/bfDf3mD.png)

6. 點選想要的網路，並點選"下一步"。
![](https://i.imgur.com/WRV7bLX.png)

7. 使用預設名稱跟位置，輸入硬碟大小，點選"下一步"。
![](https://i.imgur.com/OWGZF2X.png)

8. 選擇"從可開機 CD/DVD-ROM安裝作業系統"，選擇"映像檔"，點選"瀏覽"，找到所需uso檔，點"下一步"。
![](https://i.imgur.com/EEKvF8Y.png)

9. 新增的虛擬機器如下圖，確認資料後，點"完成"。
![](https://i.imgur.com/FlGhU2v.png)

10. 點選"設定"
![](https://i.imgur.com/2Vc0jT1.png)

11. 點選"處理器"，以你所需來設定CPU數目，然後按"確定"。
![](https://i.imgur.com/jRqzXUJ.png)

12. 點選"連線"。
![](https://i.imgur.com/XbPLruf.png)

13. 點選"啟動"。
![](https://i.imgur.com/4J4CSbA.png)

14. 開始進入安裝Ubuntu 18.04
![](https://i.imgur.com/DbIATRg.png)

:::danger
如果設定完的 Ubuntu 18.04 無法支援剪貼簿(虛擬機與主機的複製貼上)可以利用 Hyper-V 的快速建立來創建 Ubuntu 18.04。 
![](https://i.imgur.com/WWZbd29.png)
![](https://i.imgur.com/IiFodie.png)
![](https://i.imgur.com/jXxZdPI.png)


若還是無法支援剪貼簿工作
1. 設定 Hyper-v 設定加強的工作階段
![](https://i.imgur.com/ogexvJp.png)
![](https://i.imgur.com/RWh4oFm.png)

2. 關掉虛擬機(點選關閉)，並重新連線，會跳出這個畫面(圖一)，點選"連線"。
![](https://i.imgur.com/wFtgsix.png)
 
3. 若無，點"檢視"，然後勾選"加強工作的階段"，再重新第二步驟。

:::

:::success

關於連網部分，請看　[Windows 架設Hyper-V 虛擬機器](https://hackmd.io/bXBm23XeQVOYOOdl2HiVXw)。

:::

## 來源
- [Hyper-V安裝Ubuntu 16.04](https://blog.xuite.net/yh96301/blog/463474218)
- [使用Hyper-V在Windows上安裝Ubuntu 18.04 LTS](https://linuxhint.com/install_ubuntu_1804_lts_hyperv/)