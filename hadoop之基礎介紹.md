# hadoop之基礎介紹

## 簡介

Hadoop 是一個集儲存、運算、資源管理於一身的分散式 Big Data 處理平臺，分別為三大模組提供服務：

- HDFS
- Yarn
- MapReduce

## HDFS

HDFS為 Hadoop Distributed File System 的縮寫，分散式檔案系統。

HDFS是個可擴充(scalable)且可靠(reliable)的分散式檔案系統，由一台NameNode與至少一台的DataNode所組成。

儲存檔案到HDFS前，檔案會被拆開成數等分小區塊，稱之block，並且會將同一個block複製成數等分(replication, 預設值是3份)再將這些block分散儲存到各個DataNode，同時會產生一份清單，記載著這份檔案所屬的block與散落在哪幾台DataNode，這份清單會被記錄在NameNode上，而相同的block不會同時存在於同一個DataNode上。當某個Hadoop client需要讀取這個檔案時，會先跟NameNode發出請求，NameNode會根據這份清單回覆檔案的block位於哪幾台DataNode，Hadoop client再根據這份清單將各個block讀取出來，還原成一個完整個檔案。

由以上的流程可以了解NameNode與DataNode的用途：

- NameNode：儲存檔案的block清單，稱之為metadata。
- DataNode：負責儲存實體檔案的block。

## Yarn

Yarn為 Yet Another Resource Negotiator 的縮寫，是一個資源管理系統，用來管理各種分散式運算應用程式所使用的資源，在Hadoop平台上執行MapReduce的應用程式，必須藉由Yarn監控與分配資源來確保Job可正常運作完畢。

Yarn主要由兩大service組成：

- ResourceManager:主要是用來管理與裁決Hadoop叢集內資源的使用權。每個Hadoop叢集內具有一個ResourceManager。

- NodeManager:NodeManager是負責監控Hadoop叢集內每台機器的資源使用情況，例如memory, cpu, disk, network等，並且將資訊回報給ResourceManager。每個Hadoop叢集內一台或以上的NodeManager，數量預設會與DataNode相同。

當某個分散式運算的Job/Application 被submit至Yarn上面運行時，這個Job/Application會被拆成數個tasks並且產生一個ApplicationMaster (AM)，AM會負責與ResourceManager請求需要運算的資源，這時候ResourceManager會根據NodeManager回報的消息，告知AM哪幾台機器有空閑的資源可以使用，此時這些tasks會以一種抽象的資源概念:Container 被分配到這些機器上進行運算。

## MapReduce

MapReduce是用來在撰寫分散式計算大量資料的 framework，主要分為Map與Reduce兩個步驟。Map工作階段會把需要運算的資料拆分為多個獨立區塊(chunk)，平行運算完後第一階段的運算結果儲存於檔案系統上(通常會是在HDFS內)，進入Reduce階段會把Map運算的結果進行第二次的運算，運算出最後的結果。並非所有的MapReduce都會經歷過Map與Reduce這兩階段的步驟，有些Job只有Map，而有些只有Reduce，端看運算的邏輯為何。

由於MapReduce所有運算的過程都會讀寫檔案，運算效能相較之下就比較慢。運算的功能慢慢的被後起之秀Apache Spark所取代，但目前並非所有的運算情景都可使用Spark執行，故MapReduce還有其存在的價值！

## 來源

- https://ithelp.ithome.com.tw/articles/10190756
- https://bigdatafinance.tw/index.php/tech/methodology/409-hadoop





