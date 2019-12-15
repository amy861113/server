# docker compose常用指令

## 指令

1. 驗證撰寫的docker-compose.yml檔案內容是否有錯誤
```shell=
 $docker-compose config
```

2. docker-compose.yml若有使用到Dockerfile來產生Docker image(變動也可以重新產生)
```shell=
 $docker-compose build
```

3. 下載docker-compose.yml中指定取得的image
```shell=
 $docker-compose pull node-red db #服務名稱
 $docker-compose pull
```

4. 對docker-compose.yml的container進行start| stop| restart| pause| unpause| kill
```shell=
 $docker-compose start #可替換以上單字來做動作
```

5. 移除docker-compose.yml內所有停用comtainer
```shell=
 $docker-compose rm -v
 $docker-compose rm -v [服務名稱]
 $docker-compose rm -s [服務名稱] #此係數會讓正運行中的container被停用移除
```

6. 啟動docker-compose.yml內撰寫的服務
```shell=
 $docker-compose up #-d -->背景執行
 $docker-compose run node-red ash #此為服務並非image。因安全考量， 它像docker run一樣需搭配參數去做service設定
```

7. 查看docker-compose.yml指令支援的版本
```shell=
 $docker-compose version 
```
![](https://i.imgur.com/3QVfd8o.png)


:::info
docker-compose logs和docker logs幾乎一模一樣，最大差別在於docker-compose logs不支援以指定的起始時間查詢Log及額外的詳細log顯示方式
:::

8. 查詢contaimer內部port所對應的外部port
```shell=
 $docker-compose port [服務名稱] [內部port]
```

9. 顯示在docker-compose.yml檔在service定義下的container狀態(包含啟動中與非啟動中)
```shell=
 $docker-compose ps
 $docker-compose ps -q #只顯示container id資訊
```

10. 查詢docker-compose參數用法
```shell=
 $docker-compose [指令名] --help
 $docker-compose [指令名] -? #docker指令不支援
```
![](https://i.imgur.com/3s7xuZG.png)

:::danger
以上的服務名稱皆為docker-compose.yml內service名稱
:::