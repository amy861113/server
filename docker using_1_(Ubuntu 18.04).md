# docker using<1>(Ubuntu 18.04)

## 指令

1. 搜尋需要的映像檔
```shell=
 $docker search [軟體名稱]
```
此範例用nginx與apache http
```shell=
 $docker search nginx
```
![](https://i.imgur.com/07kPLQm.png)

```shell=
 $docker search "apache http"
```
:::danger
假設出現以下錯誤--Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/images/search?limit=25&term=nginx: dial unix /var/run/docker.sock: connect: permission denied
可輸入以下指令:
```shell=
 $sudo usermod -a -G docker $USER
 #then
 $sudo reboot
```
:::

3. 搜尋完後，利用docker pull去下載docker image
```shell=
 $docker pull <docker image name>
```
範例指令:
```shell=
 $docker pull nginx
 $docker pull httpd
```

4. 查看映像檔
```shell=
 $docker images
```
![](https://i.imgur.com/UsurVpf.png)

5. 執行container
```shell=
 $docker run -p [對外port]:[預設port] -d <image 名稱>
```
範例指令:
```shell=
 $docker run -p 8080:80 -d nginx
```

## 測試
![](https://i.imgur.com/qKGAscl.png)

:::success
假設將-d去掉，就不會出現container id，當用瀏覽器開啟網址時，在執行docker run指令畫面上，會出現nginx server上的訊息
```shell=
 $docker run -p 8081:80 nginx
```
![](https://i.imgur.com/Smms1MI.png)

:::



## 來源
- https://techoverflow.net/2017/03/01/solving-docker-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket/