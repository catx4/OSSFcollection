作者：[老薯條](http://vulscan.wynetech.com.tw)，2010 年 4 月投稿。

# 用開源方案打造網站保鑣

隨著 Internet 的興起，有越來越多的企業擁有自己的網站。但或許是管理人力不足或疏於管理，以至於常常成為駭客覬覦的對象，常被入侵篡改網頁或被放置惡意程式（在筆者的 服務經驗中，網站的危安事件以放置惡意程式和釣魚網頁占最大宗），但不論是何種形式的入侵，都會對企業的形象和商譽造成莫大的傷害。

有鑑於網路入侵問題嚴重，本文結合開源碼社群中的「完整性分析」工具和「遠端備份」工具實作一套網站保護系統。當網站內的資料被不正常更動時能即時地監控並立即回復。而在本系統中希望能夠具備下列幾項功能：

● 能即時地偵測網站內的資料更動情形，例如新增（檔案或目錄）、刪除（檔案或目錄）及修改檔案等。

● 當偵測到網頁資料被不正常地更動時，會啟動回復（Restore）系統，自動至遠端備份主機取得相關的備份資料，來回復被不正常更動的網頁資料。

這裡會使用到的開源碼程式如下表所示：


* ** 軟體名稱 / 說明 / 官方網站**
* Fedora 11 / 作業系統 / [http://fedoraproject.org](http://fedoraproject.org/)
* rsync / 遠端備份軟體 / [http://rsync.samba.org](http://rsync.samba.org/)
* integrit / 完整性分析軟體 / [http://sourceforge.net/projects/integrit/](http://sourceforge.net/projects/integrit/)



### 安裝 integrit 完整性分析軟體

說到完整性分析工具，很多人第一時間想到的可能是[Tripwire](http://www.tripwire.org/)，Tripwire 是一個知名且功能強大的分整性分析工具，但相對地，其系統資源消耗較多且設定複雜，況且 Tripwire 目前已成為商業化軟體，因此這裡將介紹另一套具有相同功能但較為精緻的完整性分析工具integrit，integrit主要功能在於比對檔案系統是否有 不正常的更動情形。

integrit 的工作原理是，在系統剛建立完成時（在乾淨的情況下）先建立一個基準系統狀態資料庫（在此稱為 known database）。比對時，先建立目前系統狀態資料庫（在此稱為 current database），並與 known database 進行比對。

如果基準系統狀態資料庫與目前系統狀態資料庫比對不一致，即表示目前的系統狀態有被更動過，integrit將會顯示出被更動的檔案或目錄。

由於 integrit 並未提供如 rpm 等包裝檔，所以要利用原始檔直接編譯，請至「[http://sourceforge.net/projects/integrit/](http://sourceforge.net/projects/integrit/)」取得最新版本的 integrit。截至目前為止，筆者取得的版本為4.1。

下載完成後，利用指令「configure && make && make install」安裝 integrit。完裝完成後，將產生 integrit 主程式。

另外，integrit 在「utils」目錄下提供了工具程式 i-viewdb 來查看資料庫的內容。可至「utils」目錄下直接執行指令「make」，即可編譯出 i-viewdb 執行檔。編譯成功後，繼續設定integrit的設定檔（/etc/integrit.conf），設定檔內容如下表所示。

#### 參數名稱意義 
* known：基準系統狀態資料庫，比對即以此資料庫為基準點（Baseline） 
* current：目前系統狀態資料庫，記錄目前系統的狀態 
* root：比對啟始目錄，integrit 會從此目錄往下遞迴開始比對。如設定為「/usr」，即每次比對均只會從「/usr」目錄以下開始遞迴比對
* [RULE]格式如下--[prefix] [檔名或目錄][switch] ：可針對個別檔案或目錄設定特別的比對規則。

    **[prefix] 規則意義是：**
    * !：不比對該目錄或檔案，例如「!/usr/local」即表示不比對「/usr/local」目錄。  
    * =：僅比對該目錄或檔案，如果設定為目錄，代表僅比對該目錄而不遞迴的往下比對子目錄，如設定為「=/usr/local」，則僅比對「/usr/local」目錄，但不繼續往下比對「/usr/local」下的子目錄。

  ** [switch]參數說明如下：**
 * s：檢查checksum是否有改變
 * i：檢查inode是否有改變
 * p：檢查權限permissions是否有改變
 * l：檢查link的數量（number of links）是否有改變
 * u：檢查uid是否有改變
 * g：檢查gid是否有改變
 * z：檢查檔案大小是否有改變
 * a：檢查存取時間是否有改變
 * m：檢查更動時間是否有改變
 * c：檢查新建時間是否有改變

    如果[switch]為大寫即表示關閉該檢查，小寫則表示使用該檢查。 

以下將以 integrit 來檢查檔案系統變動的情況，假設在「/usr/local/apache2/htdocs/test/」目錄下有一個名叫index.html 的檔案，其內容如下：

```
<html>
 <b>hello world!!!</b>
 </html>
```

然後，在「/etc/integrit.conf」加入下列的設定：

```
known=/root/known.cdb #基準系統狀態資料庫
 current=/root/current.cdb #目前系統狀態資料庫
 root=/usr/local/apache2/htdocs/test #比對啟始目錄
```

首先，使用 integrit 來建立基準資料庫（請自行查閱 integrit 用法），以下列指令建立基準系統狀態資料庫：

```integrit -u -C /etc/integrit.conf -N /root/known.cdb```

其中-u 為 update，-C為使用「/etc/integrit.conf」設定檔，-N則為產生目前系統狀態資料庫，如果讀者參閱 integrit 的用法，會發現 -N 參數原意是用來產生目前系統狀態資料庫，而另一個參數 -O 才是用來產生基準系統資料庫，但筆者測試結果發現，使用 -O 參數並無任何作用，所以還是利用 -N 參數來建立基準系統狀態資料庫。

建立完成後，可利用 i-viewdb 來查看 integrit 的資料庫內容，並利用指令「i-viewdb /root/known.cdb」查看known.cdb 的內容。

建立好基準系統狀態資料庫後，將測試下列四種狀況：

####檔案系統新增情況

先利用指令「touch/usr/local/apache2/htdocs/test/index2.html」新增一個檔案，接著執行指令 「integrit -u -c-C /etc/integrit.conf」進行檢查（-c 的意思為 check，即為檢查）：

[![](http://www.openfoundry.org/images/100511/p01.jpg)](http://www.openfoundry.org/images/100511/p01.jpg)

integrit 一旦發現有新增檔案，就會以new來表示該新增檔案。

####刪除情況

先利用指令「rm -f/usr/local/apache2/htdocs/test/index.html」刪除檔案，然後使用指令「integrit -u -c -C/etc/integrit.conf」進行檢查。integrit若發現有被刪除檔案，即以 missing 來表示該被刪除檔案。

[![](http://www.openfoundry.org/images/100511/p02.jpg)](http://www.openfoundry.org/images/100511/p02.jpg)

####修改情況

先執行指令「touch /usr/local/apache2/htdocs/test/index.html」，重新更動 index.html 的存取時間，接著使用指令「integrit -u -c -C /etc/integrit.conf」來檢查：

[![](http://www.openfoundry.org/images/100511/p03.jpg)](http://www.openfoundry.org/images/100511/p03.jpg)

如果 integrit 發現檔案已經被修改過（在本例中為檔案存取時間被更動），就會以 changed 來表示該檔案已被修改。

####特別選項（switch）情況

integrit 也提供可針對個別的檔案或目錄特別的檢查選項（switch）。若不在意檔案修改時間被改變，可利用設定switch 選項來達到此要求。切換到「/etc/integrit.conf」，然後加入以下的設定：

```/usr/local/apache2/htdocs/test/index.html AMC```

該行設定表示不須對index.html的存取時間（A）、更動時間（M）和新建時間（C）做檢查（使用大寫字母即表示不對該選項做檢查）。

接著重新執行指令「integrit -u -c -C /etc/integrit.conf」進行檢查，會發現integrit不會認定index.html有變動的情形。其他更細緻的switch設定，請自行測試。

###設定rsync遠端備份程式

rsync 是一種異地備份軟體，目前由 Samba 組織維護。一般備份軟體備份方式可分為「鏡像備份」（MirrorBackup）和「增量備份」（IncrementalBackup）兩種。「鏡像備 份」就像鏡子一樣，將資料一對一的備份過去，就如同copy指令一樣。而「增量備份」每次備份前會先比較兩邊資料的不同，僅備份異動的資料。

rsync 軟體備份方式即屬於增量備份方式，每次備份之前會先比較來源端和目的端的檔案差異，僅將有差異的資料備份至目的端。在開始介紹 rsync 之前，先行定義下列名詞：

來源端：欲備份的主機
 目的端：備份主機（存放被備份主機的資料）

而 rsync 的運作方式分成伺服器（Server）模式與客戶端（Client）模式。伺服器模式是以 daemon 方式運作，即rsync 伺服器；客戶端模式則是以一般程式方式運作，可視為 Client 端程式。此外，rsync 備份時可區分 Push 和 Pull兩種行為，Push 是從來源端將備份資料庫備份到目的端，而 Pull 是將目的端的備份資料備份至來源端。

安裝 rsync 軟體的方法很簡單，使用指令「yum install rsync*」即可安裝好相關相關的軟體。安裝完成後，首先必須設定目的端的 rsync 伺服器的設定檔（/etc/rsyncd.conf），內容設定如下：

```
[restore] #備份代號，名稱可自訂.
 path = /restore #用來設定備份檔案要存放的目錄
 auth users = restore_user #可允許使用的帳號
 uid = root #設定 rsync 執行時，所使用的個人權限
 gid = root #設定 rsync 執行時，所使用的群組權限
 secrets file = /etc/rsyncd.secrets #認證用密碼檔，在此檔案設定 client 端的帳號及密碼
 read only = no
```

接著，設定「/etc/rsyncd.secrets」檔案，此檔案格式為「帳號:密碼」，設定如下：

```restore_user:passwd （請自行設定帳號及密碼）```

這裡要特別要注意的是，須將「/etc/rsyncd.secrets」擁有者設為 root，並且將權限設定為 600，不然備份指令可能不會成功：
```
chown root:root /etc/rsyncd.secrets #更動檔案擁有者為root
 chmod 600 /etc/rsyncd.secrets #重新設定檔案權限
```
然後，須設定來源端的密碼檔（/etc/rsyncd.secrets），此檔案僅須設定密碼 passwd，以本例而言即為 passwd。同樣地，此檔案也須設定擁有者為root以及將權限設為600：
```
chown root:root /etc/rsyncd.secrets
 chmod 600 /etc/rsyncd.secrets
```
完成來源和目的端設定之後，在目的端上要先行啟動 rsync 伺服器，預設 rsync 會由 xinetd 所啟動。請開啟「/etc/xinetd.d/rsync」檔案並設定如下的組態資料：
```
service rsync
 {
 disable = no #將預設disable =yes 改為 disable=no
 flags = IPv6
 socket_type = stream
 wait = no
 user = root
 server = /usr/bin/rsync
 server_args = --daemon
 log_on_failure += USERID
 }
```
設定完成後，利用指令「xinetd start」來啟動目的端的rsync伺服器。可利用指令「netstat -ant | grep LISTEM」來判別目的端rsync服務是否啟動（檢查服務埠873是否有開啟），如下圖所示。

[![](http://www.openfoundry.org/images/100511/p04.jpg)](http://www.openfoundry.org/images/100511/p04.jpg)

在rsync伺服器啟動完成後，可先行測試下列狀況：

####Push－從來源端將資料備份至目的端

 先在來源端上下達下列的指令：
```
rsync -rvlHpogDtS --password-file=/etc/rsyncd.secrets
 /usr/local/apache2/htdocs/test/* [restore_user@xxx.xxx.xxx.xxx](mailto:restore_user@xxx.xxx.xxx.xxx) ::restore
```
執行上述指令是為了將來源端上的「/usr/local/apache2/htdocs/test」目錄資料備份至xxx.xxx.xxx.xxx的伺服主 機，restore 為上例設定的備份代號，而 restore_user 是使用者名稱。其中 rsync 的參數用法說明如下：
```
-r：recursive，以遞迴的模式備份
 -v：verbose，詳細模式輸出
 -l：軟連結（Soft Link）的檔案也要備份過去
 -H：保留硬連結（Hard Llink）
 -p：保留文件權限
 -o：保留文件擁有者資訊
 -g：保留文件群組資訊
 -D：devices，設備檔案也要備份過去
 -t：times，保留文件時間
 -S：進行特殊處理，以節省儲存空間
```
如果一切順利，來源端的「/usr/local/apache2/htdocs」目錄的資料，將會被備份至目的端的「/restore」目錄。

####Pull－將資料從目的端下載至來源端

 接著，在來源端上執行如下的指令：
```
rsync -rvlHpogDtS --password-file=/etc/rsyncd.secrets [restore_user@xxx.xxx.xxx.xxx](mailto:restore_user@xxx.xxx.xxx.xxx) ::restore /usr/local/apache2/htdocs/test
```
該指令的作用是，從xxx.xxx.xxx.xxx下載備份資料至本機，其中 restore 為設定的備份代號，restore_user 為使用者名稱，如果一切順利，來源端將從目的端上的「/restore」目錄內取回相關的備份資料並置於「/usr/local/apache2 /htdocs/test」目錄下。其餘相關指令用法，請自行參閱rsync的說明。

###實作網站保護系統

在這個系統中將包含觸發元件、還原及備份元件這兩個子元件。觸發元件利用 integrit 軟體來監控檔案系統，一旦發現有異動，即觸發還原元件來回復被異動的檔案。

至於還原及備份元件，則利用 rsync 軟體來進行還原或備份功能。還原功能是至遠端 rsync 伺服器取回（Pull）備份資料來回復被異動的檔案。而備份功能則是在製作系統基準資料庫的同時，須將相關的檔案備份（Push）到遠端的rsync 伺服器。

這裡將實作一個網站系統的保護系統，當網站內的檔案遭受到不正常的異動時，能自動至遠端 rsync 伺服器下載備份資料來回復。先假設已建立好網站伺服器，網站根目錄為「/usr/local/apache2/htdocs/test」，IP為 「192.168.2.1」（在此稱為來源端），並參閱上述章節說明架設好 rsync 伺服器，IP為「192.168.2.2」（在此稱為目的端）。相關系統架構圖如下所示：

[![](http://www.openfoundry.org/images/100511/p05.jpg)](http://www.openfoundry.org/images/100511/p05.jpg)

首先，在來源端先製作一份基準資料庫以供後續比對之用，並且將相關的網站資料備份至遠端備份伺服器（目的端）上（backup 機制），在來源端上鍵入下列指令：
```
integrit -u -C /etc/integrit.conf -N /root/known.cdb &&
 rsync -rvlHpogDtS --password-file=/etc/rsyncd.secrets /usr/local/apache2/htdocs/test [restore_user@192.168](mailto:restore_user@192.168) .2.2::restore
```
&& 意指前一個指令成功，才可執行下一個指令。本指令為新建一個基準資料庫，名稱為 known.cdb，當成功建立基準資料庫後，再將「/usr/local/apache2/htdocs/test」目錄的檔案備份至目的端的 「/restore」目錄下。

在成功建立基準資料庫並將資料備份到遠端 rsync 伺服器（目的端）之後，還需要一個能「定時」監測網站根目錄且能自動至目的端下載回復資料的機制。

由上述 intergrit 的說明得知，異動情況可分為刪除（Missing）、修改（Changed）、新增（New），其中刪除情況直接至目的端取回備份資料（Pull）回復即 可，而新增情況因直接下載備份資料並不會移除新增的檔案，所以須移除新增的檔案。至於修改情況，因 rsync 伺服器上備份的檔案時間，可能會比實際線上的檔案時間還要舊，所以不會覆蓋掉實際線上的檔案，因此當發生檔案被修改時，須要進行下列兩個步驟：

1\. 移除被更動的檔案
 2\. 從遠端的 rsyn c主機下載（Pull）資料回復

筆者利用 perl 寫了一個小程式（webprotect.pl）來處理這些情況，請自行將 192.168.2.2 更改為自己主機的 IP，內容如下：
```
#!/usr/bin/perl
 $result=`integrit -u -c -C /etc/integrit.conf`; #檢查指令
 @token = split(/\n/, $result); #解析檢查指令的回傳字串
 $dirtyflag=0; #若有異動即須重新製作系統基準資料庫，0為無異動，1為異動
 foreach (@token)
 {
 if($_=~/(missing\:)/) #檔案系統有刪除情況，直接從遠端rsync伺服器取得資料回復
 {
 `rsync -rvlHpogDtS --password-file=/etc/rsyncd.secrets restore_user\@192.168.2.2::restore /usr/
 local/apache2/htdocs/test`;
 #回復，從遠端rsync伺服器取得資料回復
 $dirtyflag=1; #系統有異動的情況
 }
 elsif($_=~/(new\:)/) #檔案系統有新增情況
 {
 $line=$_;
 $line=~s/ / /g;
 @parse = split(/ /, $line);
 `rm -f $parse[3]`;#刪除該檔
 $dirtyflag=1;
 }
 elsif($_=~/(changed\:)/) #檔案系統有修改情況
 {
 $line=$_;
 $line=~s/ / /g;
 @parse = split(/ /, $_);
 `rm -f $parse[1]`; #刪除該檔
 `rsync -rvlHpogDtS p;nbsp; --password-file=/etc/rsyncd. secretsrestore_user\@192.168.2.2::restore /usr/local/apache2/htdocs/test`;
 $dirtyflag=1;
 }

} #foreach
 if($dirtyflag==1) #系統有異動即重新製作系統基準資料庫
 {
 `integrit -u -C /etc/integrit.conf -N /root/known.cdb `;
 }
```
接著，可以將此程式加入到 cron 排程之中，設定內容如下：
```
*/1 * * * * /usr/bin/perl /usr/bin/webprotect.pl
```
如此一來，程式就會每分鐘定時地監控網站根目錄，一旦發現不正常的異動，即會自動至目的端下載備份資料回復，為網站系統提供完整性的保護。

####作者：吳惠麟
 多年資安相關工作經驗，專長整合開源碼資源及資訊安全；合著有「資訊安全實驗與原理」一書，現任職於中山大學「台灣電腦網路危機處理中心」。

**本文經《網管人》同意，轉載自第 51 期文章內容。**