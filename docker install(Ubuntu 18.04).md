# docker install(Ubuntu 18.04)

## 安裝步驟

1. 更新
```shell=
 $sudo apt-get update
```

2. 下載相關工具與模組
```shell=
 $sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

3. 下載並新增APT儲存庫憑證
```shell=
 $curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

:::info
9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88通過搜索指紋的後8個字符，驗證您現在是否擁有帶有指紋的密鑰 。
:::

4. 檢查APT儲存庫憑證
```shell=
 $sudo apt-key fingerprint 0EBFCD88
```
![](https://i.imgur.com/rPbdnbH.png)

5. 新增docker儲存庫到儲存庫清單
```shell=
 $sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

6. 更新APT儲存庫
```shell=
 $sudo apt-get update
```

7. 安裝docker CE版
```shell=
 $sudo apt-get install docker-ce docker-ce-cli containerd.io
```

8. 檢查版本
```shell=
 $docker -v
```
![](https://i.imgur.com/0py3Wg5.png)




## 來源
- https://docs.docker.com/install/linux/docker-ce/ubuntu/


