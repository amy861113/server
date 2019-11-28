# docker using<2>(Ubuntu 18.04)

## 安裝步驟

1. 查看執行中的container
```shell=
 $docker ps
```
![](https://i.imgur.com/qbnMSHD.png)

2. 回到執行8081的那個前景執行的nginx，按ctrl+C，再執行docker ps，第二個nginx消失了

![](https://i.imgur.com/wffBowS.png)

3. 加上-a指令可以將已經啟動過的container都列出來，不管它是不是在運行中，方便用來重新啟動或刪除container
```shell=
 $docker ps -a
```
![](https://i.imgur.com/ZRatr0K.png)

4. 快速查詢最近一次啟動的container狀態
```shell=
 $docker ps -l
```
![](https://i.imgur.com/b4VnYw4.png)

5. 啟動被停止的container
```shell=
 $docker start <container id>
```
範例指令:
```shell=
 $docker start 7941a82b5141
```

6. 停止container
```shell=
 $docker stop <container id>
```
範例指令:
```shell=
 $docker stop 7941a82b5141
```

:::success
想要重新啟動container
```shell=
 $docker restart <container id>
```
:::

7. 移除container
```shell=
 $docker rm <container id>
```

8. 查詢被container佔領的硬體空間
```shell=
 $docker ps -s
```
![](https://i.imgur.com/tmFYTJY.png)


9. 刪掉不用的docker image
```shell=
 $docker images
 $docker rmi <image name or id>
```

10. 查看container詳細資訊(它會列出json格式的資料，可以用 --format(-f)去擷取你要的資料)
```shell=
 $docker inspect <container id or name>
 $docker inspect -f '{{json .Metadata}}' nginx
```

11. 查看詳細image資訊
```shell=
 $docker inspect <image id or name>
```

12. 將檔案複製進container(以index.html複製進nginx為例)
```shell=
 $docker cp <檔案名稱與完整路徑> <目的container id or name>:<完整目的資料夾路徑>
 $docker cp index.html 7941a82b5141:/usr/share/nginx/html
```
:::danger
警告 : cp指令會覆蓋掉相同名稱的檔案且不會有任何通知
:::

13. 查看哪一個container用掉最多資源
```shell=
 $docker stats
 $docker stats <contaier id or name>
 $docker stats -a #顯示機器上所擁有的container
```
![](https://i.imgur.com/Y4WfBOm.png)
:::info
出現全畫面的<即時>container資源使用狀況資訊，要跳出請按Ctrl+C
:::

14. 查看container即時log
```shell=
 $docker logs <container id or name>
 $docker logs --tail 2 <container id or name> #取最後兩條的log
 $docker logs --since 2m <container id or name> #取最後兩分鐘的log
 $docker logs -f <container id or name> #即時更新的log，跳出按下ctrl+c
```


