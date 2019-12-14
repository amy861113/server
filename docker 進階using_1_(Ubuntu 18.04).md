# docker 進階using<1>(Ubuntu 18.04)

## 指令

1. 幫container取名字
```shell=
 $docker run --name <container 名稱> -p <對外port>:<預設port> -d <docker image 名稱>
```
:::info
如果不用 --name，它會自動產生一個名字
:::

2. 設定環境變數才能執行的container
```shell=
 $docker run -e <變數名稱>=<變數值> -p <對外port>:<預設port> -d <docker image 名稱>
```
:::info
以上指令最直觀的例子為mysql架設
```shell=
 $docker run --name mysql -e MYSQL_ROOT_PASSWORD=123 -d mysql
 $docker exec -it mysql mysql -p #登入
```
:::

3. 掛接外接硬碟
```shell=
 $docker run -v <外部資料夾或檔案的完整路徑>:<container 內部要被取代的完整資料夾路徑或檔案名稱> -p <對外port>:<預設port> -d <docker image 名稱>
``` 

4. 直接執行或命令container的程式or指令
```shell=
 $docker run <docker image 名稱> <指令完整檔案路徑及選項>
 $docker run nginx /bin/ls #將此container根目錄檔案清單列出來
```

5. container執行結束後(run)，自動移除
```shell=
 $docker run --rm nginx /bin/ls
```

6. container掛掉或主機重開機後，自動重新啟動
```shell=
 $docker --restart=always <container id or name> #重開機或掛掉時，自動重啟
 $docker --restart=unless-stopped <container id or name> #掛掉時，自動重啟
 $docker --restart=on-failure:5 <container id or name> #非正常退出狀態下，嘗試重啟五次 
```
:::info
此參數 --restart預設為no
:::

7. 進入container操作指令列(如:nginx)
```shell=
 $docker run -it --rm --name nginx-cmd nginx /bin/bash
```
![](https://i.imgur.com/Tl1tuFU.png)

8. 對於運行中container進行指令操作(如:mysql)
```shell=
 $docker exec -it <container id or name> <指令完整檔案路徑與名稱及選項>
 $docker exec -it mysql mysql -p #登入
```

9. container綁定ip
```shell=
 $docker run -p <主機ip>:<主機連接port>:<container預設port> -d <docker image 名稱>
```

10. 建立container專屬網路
```shell=
 $docker network create <docker 網路名稱>
 $docker run --name <container 名稱> --net=<docker 網路名稱>
```

11. 移除container專屬網路
```shell=
 $docker network rm <docker 網路名稱>
 $docker network inspect <docker 網路名稱> #查詢加入網路的container id
```
:::danger
警告:一旦移除網路，加入網路的container就無法啟動，除非在新建一個同樣名稱的docker網路
:::


12. 關閉container
```shell=
 $docker stop <container id or name> #正常關閉container
 $docker kill <container id or name> #強制關閉container
```

13. 建立專用的Data Volume儲存資料(如:nginx)
- 第一種直接利用docker run -v去建立data volume(docker volume name自動產生)
```shell=
 $docker run -v /usr/share/nginx/html --name nginx-vol -p 8080:80 -d nginx
 $docker inspect -f '{{json .Moumts}}' nginx-vol #查詢volume name
 $sudo ls /var/lib/docker/volumes/<docker volume name>/_data #查看volume內的檔案
 $docker volume rm <docker volume name> #volume不會隨著container刪除消失，因此得手動刪除
 $docker volume prune #小心使用，會刪掉所有的volumes
```
- 第二種docker volume create + docker run -v
```shell=
 $docker volume create ngix-html #建立volume
 $docker volume ls #查詢建立的volumes
 $docker run -v nginx-html:/usr/share/nginx/html --name nginx-vol -p 8080:80 -d nginx
```
:::info
可以利用docker run的-v去建立data volume or docker volume create + docker run -v
多個container可以共用一個data volume唷!
:::
