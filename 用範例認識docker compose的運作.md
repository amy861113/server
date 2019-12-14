# 用範例認識docker compose的運作

## 簡介

docker compose由以下兩種元件組成:
- docker-compose指令
- docker-compose.yml檔案

docker-compose指令可以執行大部分docker指令的選項及功能，差別在於docker-compose指令大多是直接使用docker-compose.yml檔案裡的設定值，以下利用安裝Laravel，Nginx和MySQL作為範例


## 操作步驟
1. 下載docker-compose.yml範例檔案
```shell=
 $cd ~
 $git clone https://github.com/laravel/laravel.git laravel-app
 $cd ~/laravel-app
 $docker run --rm -v $(pwd):/app composer install #掛載在此目錄，避免compose全域掛載
 $sudo chown -R $USER:$USER ~/laravel-app #使其歸非root用戶所有
```

2. 創建Docker Compose文件
```shell=
 $nano ~/laravel-app/docker-compose.yml
```
```yaml=
version: '3' #定義yml版本
services: #定義需要啟動的container的設定資訊

  #PHP Service
  app: #自訂名
    build: #類似docker build
      context: . #指定Dockerfile位置
      dockerfile: Dockerfile #指定Dockerfile名稱
    image: digitalocean.com/php #本地找不到，預設會到docker hub下載
    container_name: app #可有可無，但每次restart docker-compose時，要確定此名字是否存在，否則出現錯誤
    restart: unless-stopped
    tty: true
    environment:
      SERVICE_NAME: app
      SERVICE_TAGS: dev
    working_dir: /var/www
    networks:
      - app-network

  #Nginx Service
  webserver:
    image: nginx:alpine
    container_name: webserver
    restart: unless-stopped
    tty: true
    ports:
      - "80:80"
      - "443:443"
    networks:
      - app-network

  #MySQL Service 
  db:
    image: mysql:5.7.22
    container_name: db
    restart: unless-stopped
    tty: true
    ports:
      - "3306:3306"
    environment:
      MYSQL_DATABASE: laravel
      MYSQL_ROOT_PASSWORD: your_mysql_root_password
      SERVICE_TAGS: dev
      SERVICE_NAME: mysql
    networks:
      - app-network

#Docker Networks
networks:
  app-network:
    driver: bridge
```

3. 檢查docker-compose.yml內容是否正確(無執行的情況下)
```shell=
 $docker-compose config -q #or
 $docker-compose -f ~/laravel-app/docker-compose.yml config -q
```

4. 執行docker-compose.yml
```shell=
 $docker-compose up -d
 $docker-compose -p amy up -d #這可以使創建出來的資料有前綴字amy_
```

:::danger
假設出現錯誤說版本相容衝突，可以重新安裝根據以下網址:
[docker compose介紹與架設](/Pk5fJOOFRHueC6zZSB13Sw)
:::


:::info
以上只是說明文件內容無法完全建立docker-compose還須配置nginx、mysql與php，還有創建Dockerfile，如需想完整安裝可觀看來源網址
:::


## 來源

- https://www.digitalocean.com/community/tutorials/how-to-set-up-laravel-nginx-and-mysql-with-docker-compose#step-3-%E2%80%94-persisting-data