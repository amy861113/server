# docker compose介紹與架設

## 簡介

docker與docker compose是兩種不同的程式，docker compose利用撰寫docker-compose.yml去整合多個container架設，就不需要每次架設都要打很多docker run指令

## 安裝步驟

- 第一種方法使用apt套件管理工具
  (缺點 : 可能不是安裝最新的docker compose，docker-compose.yml格式版本會受到限制)
  
1. 移除舊版本(如有安裝過docker，若無可跳過)
```shell=
 $sudo apt-get remove docker docker-engine docker.io -y
 $sudo apt-get autoremove -y
 $sudo apt-get purge -y
```

2. 下載相關工具與模組
```shell=
 $sudo apt-get update
 $sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

3. 安裝docker-compose
```shell=
 $sudo apt install docker-compose
```

- 第二種利用pip安裝(較為推薦)

1. 安裝pip
```shell=
 $sudo apt-get install python-pip
```

2. 升級pip
```shell=
 $sudo pip install --upgrade pip
```

3. 安裝docker-compose
```shell=
 $sudo pip install docker-compose
```

4. 建立連接檔
```shell=
 $sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

5. 查詢是否安裝成功
```shell=
 $docker-compose version
```
![](https://i.imgur.com/5Rz9MMG.png)
