# Web Server基本架設
> [name=趙祐祥][name=呂佳陵][name=陳美芳]
> [time=Mon, Jul 24, 2017]

## 網頁伺服器
### 簡介
一個最簡單的 Web Server 之功能包含下列三個步驟:

1. 接收瀏覽器所傳來的網址。
2. 取出相對應的檔案。
3. 將檔案內容傳回給瀏覽器。

然而、在這個接收與傳回的過程中，所有的資訊都必須遵照固定的格式，規範這個接收/傳送格式的協定，稱為超文字傳送協定 **(Hyper Text Transfer Protocol)**，簡稱為 HTTP 協定。

from:http://ccckmit.wikidot.com/code:webserver
### 安裝流程
1. 更新apt-get
```shell
    $sudo apt-get update
```
2. 安裝apache2
```shell
    $sudo apt-get install apache2
```
3. 重啟apache2
```shell
    $sudo service apache2 restart
```
4. 確定是否啟動apache2
```shell
    $sudo ps -aux | grep apache2
```

附註:
開啟apache2
```shell
    $sudo service apache2 start
```
關閉apache2
```shell
    $sudo service apache2 stop
```
### 相關設定
1. 安全設定 
    * 避免網頁被讀取
        設定/etc/apache2/apache2.conf 
        ```shell=
         $sudo nano /etc/apache2/apache2.conf 
        ```
        
        移除<Directory / > 和 </Directory> 中間的indexes字眼!
        ![](https://i.imgur.com/3e05UHu.png)
    * 移除網頁伺服器資訊
        修改/etc/apache2/conf-available/localized-error-pages.conf
        ```shell
            $sudo nano /etc/apache2/conf-available/localized-error-pages.conf
        ```
        設定ErrorDocument 403 內容
        ![](https://i.imgur.com/qzypRn7.png)

2. 啟動個人網頁
```shell
    $sudo a2enmod userdir
```
3. 無法讀取個人家目錄的php檔，修改php7.0.conf檔
```shell
    $sudo vi /etc/apache2/mods-available/php7.0.conf
```
修改下方設定，將Off改為ON
![](https://i.imgur.com/QfauBvh.png)


### 顯示結果
- 查看是否安裝完成:在瀏覽器輸入    IP:webport
![](https://i.imgur.com/aUQQK9X.png)

附註:
    未移除網頁伺服器資訊
    
- 個人家目錄的php檔
![](https://i.imgur.com/IUwkScx.png)


## php
### 簡介
PHP的全名為**Hypertext Preprocessor**，它是個被廣泛運用在網頁程式撰寫的語言，尤其是它能適用於網頁程式的開發及能夠嵌入HTML文件之中，它的語法和C、Java及Perl等語法相似，且學習起來更容易上手。PHP的目地是為了能使網站開發者可以快速地撰寫動態網頁。

from:https://sls.weco.net/blog/ie965119/04-jan-2009/12246

### 安裝流程
1. 安裝PHP 7.0
```shell
    $sudo apt-get install php7.0 lilibapache2-mod-php7.0 php7.0-gd
```


### 顯示結果
- 測試php檔
![](https://i.imgur.com/ecyDSv6.png)




## 資料庫
### 簡介
資料庫，即一個可以存放大量資料集合的地方，而資料庫管理系統(DBMS)則為提
供使用者在不需要了解資料庫內部實際運作下能有效率且方便的對資料庫進行
管理的介面

from:http://yes.nctu.edu.tw/SQL/Articles/SQL%E5%92%8C%E8%B3%87%E6%96%99%E5%BA%AB%E7%B0%A1%E4%BB%8B(%E5%85%A8%E6%96%87%E7%89%88)%20.pdf
### 安裝流程
1. 安裝mysql 5.7
```shell
    $sudo apt-get install mysql-server
```
2. 設定 root 密碼 (安裝時跳出設定視窗)

:::warning
若設定期間未跳出設定root密碼可使用以下指令

- 初始化資料庫( 第一個問密碼，其他的為了安全性，都按Y)
```shell=
$sudo mysql_secure_installation
```
:::

### 顯示結果
- 登入mysql
![](https://i.imgur.com/MoZpfij.png)


# 網頁伺服器SSL憑證
## 簡介
- 全名**Secure Sockets Layer**
- 網頁伺服器與瀏覽器之間加解密方式

## 運作方式
- Client如何開始建立SSL對話，以發送port來判斷
http port:80
https port:443
- Web Server須有公鑰與私鑰
公鑰 => 數位簽章憑證(有效期限)

## 設定步驟
### 產生私鑰及憑證
1. 建立SSL資料夾存放私鑰及憑證(位置可依喜好)
```shell
    $sudo mkdir /etc/apache2/ssl
```
2. 切換目錄
```shell
    $cd /etc/apache2/ssl
```
3. 產生RSA Private Key
```shell
    $sudo openssl genrsa -out apache.key 2048
```
- genrsa
Generation of RSA Private Key.
- 2048
the size of the private key to generate in bits. This must be the last option specified. The default is 512.
4. 產生憑證申請單
```shell
    $sudo openssl req -new -key apache.key -out apache.csr
```
- req
PKCS#10 X.509 Certificate Signing Request (CSR) Management.
- new
this option generates a new certificate request. It will prompt the user for the relevant field values. The actual fields prompted for and their maximum and minimum sizes are specified in the configuration file and any requested extensions.
If the -key option is not used it will generate a new RSA private key using information specified in the configuration file.
- key
This specifies the file to read the private key from. It also accepts PKCS#8 format private keys for PEM format files.
- out
This specifies the output filename to write to or standard output by default.
5. 憑證申請單內容
- Country Name (2 letter code) [AU]:TW
- State or Province Name (full name) [Some-State]:Taiwan
- Locality Name (eg, city) []:taichung
- Organization Name (eg, company) [Internet Widgits Pty Ltd]:PU
- Organizational Unit Name (eg, section) []:CCI
- Common Name (e.g. server FQDN or YOUR name) []:(Domain name or 自己取(webhttps.cmrdb.cs.pu.edu.tw))
- Email Address []:(管理員email)(隨便填)

6. 產生數位簽章
```shell
    $sudo openssl x509 -req -days 365 -in apache.csr -signkey apache.key -out apache.crt
```
- x509 : 由上而下金字塔式的憑證制度。
- req
by default a certificate is expected on input. With this option a certificate request is expected instead.
- days
specifies the number of days to make a certificate valid for. The default is 30 days.
- in filename
This specifies the input filename to read a certificate from or standard input if this option is not specified.
- signkey filename
this option causes the input file to be self signed using the supplied private key.
- out filename
This specifies the output filename to write to or standard output by default.

7. 刪除申請單
```shell
    $sudo rm apache.csr
```
8. 更改apache.key權限
```shell=
    $sudo chmod go-rwx apache.key
    or
    $sudo chmod 0600 apache.key
```

### 設定apache伺服器
1. 打開SSL模組
```shell
    $sudo a2enmod ssl
```
2. 設定SSL位置
```shell
    $sudo cd /etc/apache2/sites-available/default-ssl.conf
    複製一份修改
    $sudo cp default-ssl.conf ssl.conf
```
修改底下兩行位置
```shell
SSLCertificateFile /etc/apache2/ssl/SSL/CertificateFile(apache.crt) (放簽章)
SSLCertificateKeyFile /etc/apache2/ssl/SSL/CertificateKeyFile(apache.key) (放私鑰)
```

3. 打開SSL虛擬站台
```shell
    $sudo a2ensite ssl
```
- 強制導向https瀏覽 (80轉443port)
```shell
    $sudo vi /etc/apache2/sites-available/000-default.conf
    新增
    <VirtualHost *:80>
    ...
    Redirect "/" "https://120.110.112.106:62443"
    ...
    </VirtualHost>
```
4. 測試https

- 網頁連結https://120.110.112.106:62443
or
```shell 
    $curl -k https://10.10.10.62 
```

# Let's Encrypt
## 簡介
是由許多大公司以及各大非營利團體為了推廣 HTTPS 而贊助的一家免費發佈 SSL certificate 的 Certiciate Authority (CA)。
## 限制
- Names/Certificate：單一 certificate 限制 100 個 hostname。
- Certificates/Domain：每個 domain 每個禮拜最多 20 個 certificate，但更新不計算在內 (需要憑證內的 hostname 與之前完全一樣)。
- Certificates/FQDNset：相同 hostname 的憑證每個禮拜最多發出五個。
## 安裝流程
**1. 下載最新 release 的 dehydrated 並且解開**
- dehydrated 是一個使用ACME服務器，簽署證書的客戶端。
```shell=
$ # refer:https://github.com/lukas2511/dehydrated/releases
$curl -LO https://github.com/lukas2511/dehydrated/archive/v0.4.0.tar.gz
$tar -zxv -f v0.4.0.tar.gz
$cd dehydrated-0.4.0/
```
或是透過Git下載最新版本
```shell=
$cd ~; git clone https://github.com/lukas2511/dehydrated.git
$cd dehydrated/
```
也可以直接只抓執行檔
```shell=
$curl -LO https://raw.githubusercontent.com/lukas2511/dehydrated/master/dehydrated
```
**2. 把程式安裝到 /etc/dehydrated/ 下**
```shell=
$mkdir /etc/dehydrated/
$cp ~/dehydrated/dehydrated /etc/dehydrated/
$chmod a+x /etc/dehydrated/dehydrated
```
**3. 建立 SSL certificate 證驗證過程時所需要的目錄**
```shell=
$mkdir -p /var/www/dehydrated/
```
**4. 設定 Apache，在要認證的 virtual host 裡加上**
```shell=
Alias /.well-known/acme-challenge/ /var/www/dehydrated/
```
- 第一次需要先同意 Let's Encrypt 的條款
```shell=
$sudo /etc/dehydrated/dehydrated --register --accept-terms
```
- 第一次產生 SSL certificate
```shell=
$sudo /etc/dehydrated/dehydrated -c -d 網域名
```
成功的話會有類似的輸出：
```shell=
# INFO: Using main config file /etc/dehydrated/config
Processing 網域名
 + Signing domains...
 + Generating private key...
 + Generating signing request...
 + Requesting challenge for 網域名...
 + Responding to challenge for 網域名...
 + Challenge is valid!
 + Requesting certificate...
 + Checking certificate...
 + Done!
 + Creating fullchain.pem...
 + Done!
```
成功後產生的檔案都在 /etc/dehydrated/certs/網域名/ 裡：
```shell=
drwx------ 2 root root 4096 Feb 24 02:25 .
drwx------ 3 root root 4096 Feb 24 02:23 ..
-rw------- 1 root root 1651 Feb 24 02:25 cert-1456280700.csr
-rw------- 1 root root 2143 Feb 24 02:25 cert-1456280700.pem
lrwxrwxrwx 1 root root   19 Feb 24 02:25 cert.csr -> cert-1456280700.csr
lrwxrwxrwx 1 root root   19 Feb 24 02:25 cert.pem -> cert-1456280700.pem
-rw------- 1 root root 1675 Feb 24 02:25 chain-1456280700.pem
lrwxrwxrwx 1 root root   20 Feb 24 02:25 chain.pem -> chain-1456280700.pem
-rw------- 1 root root 3818 Feb 24 02:25 fullchain-1456280700.pem
lrwxrwxrwx 1 root root   24 Feb 24 02:25 fullchain.pem -> fullchain-1456280700.pem
-rw------- 1 root root 3243 Feb 24 02:25 privkey-1456280700.pem
lrwxrwxrwx 1 root root   22 Feb 24 02:25 privkey.pem -> privkey-1456280700.pem
```
**5. 修改 Apache 的 SSL 設定**
```shell=
SSLCertificateFile /etc/dehydrated/certs/網域名/cert.pem
SSLCertificateChainFile /etc/dehydrated/certs/網域名/chain.pem
SSLCertificateKeyFile /etc/dehydrated/certs/網域名/privkey.pem
```
**6. 重新載入 Apache 的設定檔 (或是直接重新啟動)**
```shell=
$sudo service apache2 reload
$sudo service apache2 restart
```
**7. 設定 /etc/cron.d/dehydrated-網域名 ，讓 cron 每天自動檢查並更新**
```shell=
0 0 * * * root sleep $(expr $(printf "\%d" "0x$(hostname | md5sum | cut -c 1-8)") \% 86400); ( /etc/dehydrated/dehydrated -c -d 網域名; /usr/sbin/service apache2 reload ) > /tmp/dehydrated-網域名.log 2>&1
```
:::info
- 選用 dehydrated 而非官方的 certbot 是因為 dehydrated 的需求相當低，只需要有 curl 與 openssl 就可以執行，相較於官方版本需要 Python 會比較簡單。
- 放到 /etc/dehydrated/ 下的目的是避免之後各作業系統有提供 dehydrated 的套件而衝突到 (套件通常都會把可執行檔放到 /usr/bin 或是 /usr/sbin 下)，另外一方面 dehydrated 會吃同一個目錄下的 config，這對於設定上可以少一些功夫。
- 在 cron job 裡面每天執行是因為 dehydrated 會自己檢查憑證有效期限，如果還有一個月以上的時間有效就不會 renew，所以不需要擔心每天執行會造成 Let's Encrypt 的伺服器產生負擔。
- 在 cron job 中的 sleep $(expr $(printf "\%d" "0x$(hostname | md5sum | cut -c 1-8)") \% 86400)這樣的設計是可以避免同時間有太多機器到 Let's Encrypt 的伺服器，造成類似 DDoS 的攻擊。
:::
參考資料: https://letsencrypt.tw/


# SUID, SGID, SBIT
## SUID
### 說明
- 當 s 這個標誌出現在檔案擁有者的 x 權限稱為SUID，也稱為 Set UID。
- SUID的限制和功能:
    SUID 權限僅對二進位程式(binary program)有效；
    執行者對於該程式需要具有 x 的可執行權限；
    本權限僅在執行該程式的過程中有效 (run-time)；
    執行者將具有該程式擁有者 (owner) 的權限。
備註:
    SUID 僅可用在binary program 上， 不能夠用在 shell script 上面，對於目錄也是無效的
### 範例
![](https://i.imgur.com/5XtJYpT.png)


## SGID
### 說明
- 當 s 這個標誌出現在群組的 x 權限稱為SGID，也稱為 Set GID。

*與 SUID 不同的是，SGID 可以針對檔案或目錄來設定
- SGID針對檔案的限制和功能:
    SGID 對二進位程式有用；
    程式執行者對於該程式來說，需具備 x 的權限；
    執行者在執行的過程中將會獲得該程式群組的支援！
- SGID針對目錄的限制和功能:
    使用者若對於此目錄具有 r 與 x 的權限時，該使用者能夠進入此目錄；
    使用者在此目錄下的有效群組(effective group)將會變成該目錄的群組；
    用途：若使用者在此目錄下具有 w 的權限(可以新建檔案)，則使用者所建立的新檔案，該新檔案的群組與此目錄的群組相同。
     
### 範例
新增/bin/ls SGID權限
其他人無法看到/root裡面的內容
![](https://i.imgur.com/BGxXzad.png)
使用帳號群組為sam
![](https://i.imgur.com/eBFdvcJ.png)
透過SGID，sam帳號取得root群組 (r-x)，使sam能夠讀到/root裡面的內容
![](https://i.imgur.com/FQc3L3x.png)
補充:使用ls需有rx的權限
## SBIT
### 說明
- SBIT也稱為Sticky Bit

*SBIT 目前只針對目錄有效，對於檔案已經沒有效果了
- SBIT針對目錄的限制和功能:
    當使用者對於此目錄具有 w, x 權限，亦即具有寫入的權限時；
    當使用者在該目錄下建立檔案或目錄時，僅有自己與 root 才有權力刪除該檔案;

### 範例
新增一個目錄，/home/sam/sbit/，且給予SBIT權限
![](https://i.imgur.com/pdZ1vP8.png)
切換到test帳號，新增一個檔案為123
![](https://i.imgur.com/pKlZH7T.png)
並將檔案123權限全開
![](https://i.imgur.com/JnKPMVs.png)
切換到test2帳號，此帳號無法移除
![](https://i.imgur.com/uwnzjPm.png)
切換為自己或root，才有權限刪除檔案
![](https://i.imgur.com/R5CaXBM.png)


## SUID/SGID/SBIT 權限設定
- 數字型態更改權限的方式為『三個數字』的組合，那麼如果在這三個數字之前再加上一個數字的話，最前面的那個數字就代表這幾個權限了。
    *4 為 SUID
    *2 為 SGID
    *1 為 SBIT
    
- 假如下達 SUID/SGID/SBIT 的指令，卻沒有 x 權限時，則會出現S/T。
備註:
    S/T代表『空的』
    
參考資料: 
1. https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-16-04
2. http://linux.vbird.org/linux_basic/0220filemanager.php#suid_sgid_sbit
