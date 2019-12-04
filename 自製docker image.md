# 自製docker image

## 安裝步驟

1. 從運行中的container產生新的image(如:nginx)
```shell=
 $docker pull nginx
 $docker run --name nginx-hello -p  8080:80 -d nginx
 $docker cp index.html nginx-hello:/usr/share/nginx/html
 $docker commit nginx-hello nginx-commit #產生新的image
 $docker stop nginx-hello
 $docker images
```
![](https://i.imgur.com/xlayYEU.png)

:::info
對新產生的image下文字註解介紹(-m)or作者聯絡資訊(-a)
```shell=
 $docker commit -m "Nginx Hello World Image" -a "Arthur Yu" nginx-hello nginx-commit
```
:::

2. 利用build指令自動化產生新的image(nginx)
- 產生Dockerfile
```shell=
#
# Nginx Dockerfile
#
# https://github.com/dockerfile/nginx
#

# Pull base image.
FROM dockerfile/ubuntu #指定要下載image名稱

# Install Nginx.
RUN \ #指定執行命令
  add-apt-repository -y ppa:nginx/stable && \
  apt-get update && \
  apt-get install -y nginx && \
  rm -rf /var/lib/apt/lists/* && \
  echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
  chown -R www-data:www-data /var/lib/nginx

# Define mountable directories.
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]

# Define working directory.
WORKDIR /etc/nginx #指定執行指令資料夾位置

# Define default command.
CMD ["nginx"] #指定container啟動完成後要執行的指令(當有多個CMD時，只有最後一個CMD會有作用)

# Expose ports.
EXPOSE 80 #指定port
EXPOSE 443
```
- docker build產生自訂的image
```shell=
 $docker build -t <新產生image name>:<版本> <dockerfile所在完整路徑或網址>
 $docker build -t nginx-build:demo ~
```
:::danger
切記:Dockerfile只能叫Dockerfile
:::

3. 匯出匯入搬移image(不同主機間搬移)
```shell=
 $docker save -o <匯出檔案名.tar> <要匯出image name>:<版本>
 $docker save <要匯出image name> > <匯出檔案名.tar>
 $docker load  -i <匯入檔案名.tar>
 $docker load  -i < <匯入檔案名.tar>
```

4. 用container匯出成新的image(container內部使用的檔案系統匯出)
```shell=
 $docker export -o [匯出的檔案名稱.tar] [container id or name]
 $docker export [container id or name] > [匯出的檔案名稱.tar]
 $docker import [匯入檔案名稱與路徑] [image name]:[版本標籤]
```
:::info
範例:
```shell=
 $docker export -o nginx-export.tar nginx-hello
 $docker import nginx-export.tar ayubiz/nginx:hello
 $docker run -d --name nginx-export -p 8080:80 ayubiz/nginx:hello #會產生錯誤，因為在import時，啟動nginx指令遺失，需利用 -c參數將遺失的指令加入到新產生的image，如下:
 $docker import nginx-export.tar -c 'CMD ["nginx", "-g", "daemon off;"]' ayubiz/nginx:hi #-c需搭配Dockerfile的語法
 $docker run -d --name nginx-export -p 8080:80 ayubiz/nginx:hi
```
:::


## 來源
- https://github.com/dockerfile/nginx