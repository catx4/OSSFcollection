# 以 ModSecurity 實作 Webapp Firewall
作者：[老薯條](http://vulscan.wynetech.com.tw)，2011 年 3 月投稿。

## 前言

在網路時代，相信每個企業都有屬於自己的網站，但由於人力與經費不足，常成為駭客覬覦的對象。相關的網路安全威脅事件也有日漸增多的趨勢，例如常見的 SQL Injection，造成個資外洩。而網路的安全威脅型態與其它威脅間的最大差異，在於網路的安全威脅，通常是程式設計師的疏失或經驗不足所造成的安全漏 洞（如XSS）。這與一般系統只要安裝修正程式 (patch) 即可修補系統漏洞有很大的差別。當然要解決程式的漏洞，最有效的方式即是利用 Code review 會議，重新檢視每一行程式碼，從中找出有問題的程式。但此種方式所花費的人力與物力浩大，不切實際。當然目前市面上有所謂的原始碼檢視軟體，但此種軟體不 但昂貴，而且最後還是需要仰賴人工進行比對。因此， 網路應用程式防火牆 WAF (Web Application Firewall) 因應而生。WAF 就如同一般防火牆，功能雷同，但它的定位是保護網站伺服器。

在本文中，筆者將使用開放源碼社群中最知名的網站伺服器 Apache，再加上 ModSecurity 模組，來實作一個具有 WAF 防護的網站伺服器。

所需套件如下表所示：

 軟體名稱 | 官方網址 | 說明 
---------|----------
 libapr | [http://apr.apache.org/](http://apr.apache.org/) | 安裝 ModSecurity 所需的套件 
 libpcre | [http://www.pcre.org/](http://www.pcre.org/) | 安裝 ModSecurity 所需的套件 
 libxml2 | [http://xmlsoft.org/downloads.html](http://xmlsoft.org/downloads.html) | 安裝 ModSecurity 所需的套件 
 Apache 2.2.16 | [http://www.apache.org/](http://www.apache.org/) | 網站伺服器 
 ModSecurity 2.5.12 | [http://www.modsecurity.org](http://www.modsecurity.org) | WAF（網路應用程式防火牆） 
 PHP | [http://www.php.net](http://www.php.net) | 著名的網頁程式 

### OWASP 2010 TOP 10

Open Web Application Security Project (OWASP) 是一個專門研究網路軟體安全的社群。他們每年均會提出最具威脅性的網路安全漏洞，來提醒使用者。以下簡單的說明 OWASP 於 2010 年所提出的網路安全漏洞：

####1\. Injection （注入攻擊）

由 於程式設計師疏失或經驗不足，未對使用者輸入的參數值進行檢驗，以致於惡意使用者可利用惡意的輸入值（如惡意 SQL 指令串或惡意的 script 碼），讓系統自動執行惡意的指令，而對系統造成危害。此類攻擊以 SQL injection、command injection 為代表，其中 SQL injection 最具代表也最具危害性。以下就來說明 SQL injection 的攻擊方式。

假設有一個程式，用於驗證是否為會員登入，如圖1 所示（假設會員表格 (table) 名稱為 account 並以 login 表示帳號參數，passwd 表示密碼參數）：
 [![](http://www.openfoundry.org/images/110322/ModSecurity/modsecurity_1.png)](http://www.openfoundry.org/images/110322/ModSecurity/modsecurity_1.png)
▲ 圖1

假如程式設計師並未對輸入欄位進行驗證，他的程式碼可能如下：

```
Select * from account where login=’+LOGIN +”’ andpasswd=’”+PASSWD+”’”
```

其中 LOGIN 為使用者輸入的帳號參數，而 PASSWD 為使用者輸入的密碼參數。在正常的情況下，使用者輸入正常的帳號及密碼，即組成正常的 SQL 查詢字串，如下所示：

```
Select * FROM account where login=’使用者帳號’ AND passwd=’使用者密碼’
```

如果有查詢到資訊，即表示為合法使用者（因為該筆記錄在 account 中有存在），否則即不允許登入。但是如果有惡意的使用者輸入如圖1 的字串作為帳號，所組成的 sql 查詢字串即如下所示：

```
Select * FROM account where login=’john’  - -  passwd=’’
```

在 SQL 語法中， - - 表示註解，即後續的指令均不需執行。聰明的讀者應該發現上述的指令有什麼蹊蹺了。沒錯，上述的 SQL 指令代表只要帳號符合，無需輸入密碼，即可通過驗證而成功登入。

另一個甚至不需輸入帳號的例子，是輸入如 " ' ' or 1=1 - - " 的字串作為帳號參數，則其所組成的SQL查詢指令為：

```
sqlSelect * FROM account where login=’’ OR 1=1  - -  passwd=’’
```

由於 “OR 1=1” 的條件式一定成立，所以即使不需帳號密碼，也可成功的登入。上述的例子僅為 SQL Injection 的基本型，其危害程度取決於攻擊者對於 SQL 的了解程度。有些資料庫伺服器，甚至有提供系統指令，就可能危害到系統安全。

####2\. Cross Site Scripting（XSS，跨網站腳本攻擊）

跨 網站腳本攻擊的原因跟 SQL injection 一樣，同樣是因為程式沒有檢驗使用者輸入的參數內容所造成。不過，與 SQL injection 最大不同在於， SQL injection 會對資料庫所在的主機造成重大危害，而 XSS 攻擊以造成瀏覽者安全上的危害為主，往往不會對於主機造成危害。這也導致管理者易於乎略此類網站攻擊的可能性，而使得此種攻擊有越來越普遍的趨勢。 XSS 攻擊流程如下:

攻擊者將含有 XSS 漏洞的網頁，置於受害的網站伺服器上。常見的是在討論區上留下含有 XSS 惡意碼的留言。當不知情的使用者瀏覽此網頁時（如瀏覽某則留言），即啟動 XXS 攻擊碼，而將無辜的第三者相關資訊回傳到駭客的電腦上，最常見的是回傳使用者電腦上的授權 (cookies)。

所以，對網路使用者而言，當瀏覽某個討論區留言時，發現出現彈跳視窗，就要特別注意是否有 XSS 的可能。如果發現彈跳視窗自動幫你轉址到其它的網址，那麼別懷疑，趕快離開那個網站、避開 XSS 攻擊就對了。對於網站管理者而言，在 [http://xssed.com/](http://xssed.com/) 網站中有提供含有 XSS 漏洞的網站，有興趣的讀者可到此網站上，查詢自己所管理的網站是否有被提報含有 XSS 漏洞。

####3\. Broken Authentication and Session Management（鑑別與連線管理漏洞）

此漏洞是指網站自行開發的身份證驗證與連線 (session) 管理，具有安全性的缺失。以下用網站身份驗證流程說明：

*   當使用者登入成功後，會將一個含有帳號及密碼（甚至權限等相關資訊的 cookies），丟至使用者的電腦端上
*   網站再存取該使用者的授權 cookies 來判別使用者的身份

在上述流程中，如果 cookies 並未加密，惡意的使用者只要取得此 cookies，即可得知其它使用者相關的機密敏感資訊。或者加密的演算法不夠嚴謹，一但被破解，也會造成使用者的機敏資料外流，甚至惡意使用者可藉以冒充成其它的使用者（或提升權限至管理者）。

####4\. Insecure Direct Object References（不安全的物件參考）

如果一個經驗不足的程式設計師想要實做一支能動態顯示檔案內容的程式，他會直覺想到把希望顯示的檔案設為參數，再傳進去就好了，如下連結所示：

```
http://xxx.xxx.xxx.xxx/show.php?file=xxx.txt
```

而後在接到參數值後，再直接開檔顯示即可。但是如果一個有心人傳進去的參數為：

```
http://xxx.xxx.xxx.xxx/show.php?file=../../etc/passwd
```

其中 ".." 為回到上一層，此種參數即可能將系統中的 /etc/目錄下的 passwd 檔案顯示出來。

####5\. Cross Site Request Forgery（CSRF，跨網站冒名請求）

從某種角度來看，CSRF 可視為廣義的跨網站攻擊 (XSS)。但 CSRF 通常是在使用者已登入系統服務下發動攻擊。例如：

在討論區中的某段留言塞進一段可直接登出 (logout) 的惡意程式碼。當使用者登入後、瀏覽相關留言時，只要瀏覽到這段留言，即會觸發該段惡意程式碼，而直接將該使用者登出。此即為 CSRF 攻擊。

####6\. Security Misconfiguration（不安全的組態設定）

此漏洞較偏向管理面的問題，如未更改預設的帳號及密碼，或未定時的更新系統的安全修正程式等等。

####7\. Failure to Restrict URL Access（未適當限制的 URL 取）

一般網站通常會分成前端程式及後端管理程式。基於安全的考量，Internet 上的使用者不應該獲准直接查詢後端管理程式，而應該只限制某些管理者可查詢及存取後端管理程式。如果網站未限制，而使 Internet 上的其它的使用者也可正常的查詢，即可能造成潛在的安全漏洞。

####8\. Unvalidated Redirects and Forwards（未驗證的網頁重新導向）

有些網站提供網頁重新導向至其它的網址，惡意的攻擊者可利此種特性，將惡意網址插入到重新導向的參數中，讓使用者連接到惡意的網站上。

####9\. Insecure Cryptographic Storage（不安全的加密儲存）

網站並未對機敏的資料進行加密處理，或使用不嚴謹的加密演算法，而導致攻擊者在取得相關的機敏資料後，可以很輕易的取得相關資訊。

####10\. Insufficient Transport Layer Protection（不安全的傳輸防護）

由於 HTTP 連線均是採用明碼的方式連線，攻擊者可在任何一個節點均可能利用 sniffer（竊聽）方式來取得來往資料。如果網站採用 HTTP 連線方式，來往的封包均以明碼方式傳輸，攻擊者即可輕易的取得相關的機敏資訊。

### 如何安裝 ModSecurity

在說明完相關的網路安全威脅後，接下來我們繼續來說明如何安裝 ModSecurity 來防止相關的網路安全威脅。

ModSecurity 是 Apache 的一個模組，可以提供入侵偵測及防禦功能，它就如同是網路應用程式的防火牆，可以用來抵擋如 SQL injection attacks、cross-site scripting、path traversal attacks 等相關的網路攻擊，由於 ModSecurity 需要 Apache 的 mod_unique_id 模組，所以在這邊我們需自行編譯支援 mod_unique_id 模組的 Apache 軟體，這裏筆者目的在於編譯一個 Apache + MySQL + PHP 的環境，其中筆者不多談如何編譯 MySQL，請有興趣的讀者自行參閲其它文件。在筆者的環境中，MySQL 是安裝在 /usr/local/mysql5 目錄下。

### 編譯 Apache

重新編譯支援 mod_unique_id 模組的 Apache 伺服器，如下步驟：

1\. 請讀者至 [http://httpd.apache.org/](http://httpd.apache.org/) 取得 Apache 原始碼（筆者使用版本為 2.2.16）

2\. 解開後，./configure --enable-so --enable-unique-id --prefix=/usr/local/apache2

\#重新組態 Apache，在此將 Apache 安裝在 /usr/local/apache2 目錄下並支援 DSO 及 unique-id 模組

3\. make #編譯 Apache 執行檔

4\. make install #安裝 Apache

在編譯完成後，請讀者執行 /usr/local/apache2/bin/httpd –l ，列出 Apache 內建模組，並檢查是否有 mod_unique_id.c 等字樣，如有，即表示已經內建 mod_unique_id 模組

### 編譯 PHP 模組

1\. 請讀者至 [http://www.php.net](http://www.php.net) 取得 PHP 原始碼（筆者使用版本為 5.2.10）

2\. 解壓縮後，執行 ./configure --with-apxs2=/usr/local/apache2/bin/apxs --with-mysql=/usr/local/mysql5 　#編譯一個支援 MySQL 的 PHP 模組

3\. make

4\. make install

在安裝完後，重新調整 Apache 的組態檔（在此為 /usr/local/apache2/conf/httpd.conf）

新增下列資訊：

LoadModule php5_module modules/libphp5.so # 新增 PHP 模組，讓 Apache 能支援 PHP 語言

AddType application/x-httpd-php .php .phtml .php3 #新增 PHP 檔案類型

調整組態檔 (httpd.conf) 後，

讀者可利用 /usr/local/apache2/bin/apachectl start 啟動 Apache 或 /usr/local/apache2/bin/apachectl stop 停止 Apache 運作

### 編譯 ModSecurity

1\. 至 [http://www.modsecurity.org/](http://www.modsecurity.org/) 取得最新版本（至目前為止，筆者取得版本為 2.5.12）

2\. 解壓縮後至 apache2 目錄下，執行下列指令
 ./configure --with-apxs=/usr/local/apache2/bin/apxs #編譯 ModSecurity 為 Apache 的模組

3\. make
 在 編譯的過程中，筆者曾遇過 “This PCRE version does not support match limits! Upgrade to at least PCRE v6.5” 的錯誤，這是因為 ModSecurity 還是使用 Apache 內建的 PCRE 版本，解決方式為：

cp /usr/include/pcre.h /usr/local/apache2/include ，覆蓋掉 Apache 所使用的pcre.h，再重新編譯即可。

4\. make install

編譯完成後，會在 /usr/local/apache2/modules/ 產生 mod_security.so 模組檔，接下來需在 /usr/local/apache2/conf/httpd.conf 新增下列設定檔

```
LoadModule security2_module modules/mod_security2.so ＜IfModule mod_security＞   #設定mod_security SecFilterEngine On     #開啟過濾引擎開關。SecServerSignature "Microsoft-IIS/6.0" #偽裝服務器標識＜/ IfModule＞
```

至此，讀者可以寫一個簡單的 PHP 程式 (test.php) 來測試安裝是否正確，如下所示：

```
＜?php phpinfo();    //顯示 apache 的系統資訊?>
```

假如一切設定正確，讀者應可看到如圖2 的畫面。
 [![](http://www.openfoundry.org/images/110322/ModSecurity/modsecurity_2.png)](http://www.openfoundry.org/images/110322/ModSecurity/modsecurity_2.png)
▲ 圖2

接下來我們可以簡單設定 /usr/local/apache2/conf/httpd.conf 檔案來測試 ModSecurity 模組是否正常運作，如下述設定：

```
＜IfModule mod_security2.c＞SecServerSignature "Microsoft-IIS/5.0"  #將網站伺服器偽裝成Microsoft-IIS＜/ IfModule＞
```

設定完成後，讀者需重新啟動 Apache 伺服器。而後以 Telnet（或用 nmap 可以）來測試 Apache 伺服器回覆的資訊是否已改為 Microsoft-IIS，請執行如下指令：

```
telnet 127.0.0.1 80get http:1.1 /
```

而後伺服器回覆的訊息如圖3：讀者可看出 Apache 此時回覆的資訊為 Microsoft-IIS，己達到隱藏真實網站伺服器資訊的目的。
 [![](http://www.openfoundry.org/images/110322/ModSecurity/modsecurity_3.png)](http://www.openfoundry.org/images/110322/ModSecurity/modsecurity_3.png)
▲ 圖3

### ModSecurity 組態說明

ModSecurity 是以設定規則的方式來定義應用程式防火牆規則 (WAF)，在 ModSecurity 2.X 的版本中，允許規則可在 HTTP 個別的狀態 (Phase) 執行，ModSecurity 2 將 HTTP 處理狀態 (Phase) 分為下列階段：
 1\. Request headers (REQUEST_HEADERS，phase 1)
 當網站伺服器接收到客戶端的 http 要求，正在解析 HTTP 表頭 (header) 的階段
 2\. Request body (REQUEST_BODY，phase 2)
 當網站伺服器接收到客戶端的 http 要求，正在解析 HTTP 內容 (body) 的階段
 3\. Response headers (RESPONSE_HEADERS,phase 3)
 當網站伺服器回覆到客戶端的 http 要求，在回覆 HTTP 標頭 (header) 的階段
 4\. Response body (RESPONSE_BODY,phase 4)
 當網站伺服器回覆到客戶端的 http 要求，在回覆 HTTP 內容 (body) 的階段
 5\. Logging (LOGGING,phase 5)
 在網站伺服器要寫入 log（如access.log error.log）的階段

### ModSecurity 常用參數說明

1\. SecRuleEngine On | Off #是否開啟或關閉 secrule 的解析，這一定要開啟，不然 ModSecurity 就失去意義了。
 2\. SecRule VARIABLES OPERATOR [ACTION] #設定規則，是 ModSecurity 主要設定的選項。
 (1) VARIABLES: ModSecurity 定義相當多的變數，在這邊僅說明幾個常用的變數，其它的就請讀者自行參閱相關的說明：

*   ARGS_GET: 此變數內存以 get 方式傳遞的資訊
*   ARGS_POST: 此變數內存以 post 方式傳遞的資訊
*   REMOTE_ADDR: 此變數內存遠端客戶主機的 IP 資訊
*   REQUEST_URI: 此變數內存整個 URL 的資訊
*   REQUEST_METHOD: 儲存遠端客戶主機所使用的 HTTP 傳遞方式，如 post get trace

(2) OPERATOR：利用正規表示法的比對條件，可分為下列兩種型式：
 使用內建的運算子 (operator)，在內建運算子之前加上 ：

*   contains: 如果有包含即為真，如下例：
     SecRule REQUEST_LINE　"＠contains .php"
     （假如 REQUEST_LINE 變數包含 .php ，條件即成立）
*   within: 如果有包含內設的字串，即為真，如下例：
     SecRule REQUEST_METHOD "!@within get,post"
     （假如 REQUEST_METHOD 變數內包含 get 或 post，條件即成立）
*   gt: 當大於時，條件即成立
*   eq: 當等於時，條件即成立
*   ge: 當大於等於時，條件即成立
*   lt: 當小於時，條件即成立

使用正規表示式：
 可使用正規表示法的特殊符號來設定比對條件，如下例：
 SecRule REQUEST_ADDR "^192\.168\.2\.1$"
 （當 REQUEST_ADDR 變數內容為 192.168.2.1，條件即成立）
 (3) ACTION
 常用的 action 如下所述：

*   append: 當條件成立時，在網站伺服器回覆的 HTML 的訊息中插入相關的資訊。
*   deny: 當條件成立時，不允許使用者瀏覽，如最後兩段舉的例子。
*   drop: 這個 ACTION 特別提出用在預防暴力攻擊及拒絕服務的預防上，如下：
     常用的 actionSecAction phase: 1,initcol:ip=%{REMOTE_ADDR},nolog\
     SecRule ARGS: login "!^$"\ nolog,phase:1,setvar:ip.auth_attempt=+1,deprecatevar: ip.auth_attempt=20/120
     SecRule IP:AUTH_ATTEMPT "@gt 25" \
     "log,drop,phase:1,msg: 'Possible Brute Force Attack'"\
*   （在 2 分鐘內若有超過 20 次認證存取錯誤，即認定為暴力攻擊，並記錄相關的資訊）

*   exec: 當條件成立時，可設定執行某個程式：
     SecRule REQUEST_URI "^/cgi-bin/script\.pl" \
     "exec:/usr/local/apache/bin/test.sh"
     （當使用者存取 script.pl 即執行 /usr/local/apache/bin/test.sh 程式）
*   log: 記錄 ModSecurity 的相關 LOG。
*   nolog: 不記錄 ModSecurity 的相關 LOG。
*   phase: 設定規則適用於那個 phase 階段，ModSecurity 定義了 5 個 phase，規則可設定適用於那個 phase
*   redirect: 當條件成立時，轉址到另一個網址
     SecRule REMOTE_ADDR "^xxx.xxx.xxx.xxx$"
     "redirect:http://www.modsecurity.org"
     （如果來源為xxx.xxx.xxx.xxx連線，即轉址到 http://www.modsecurity.org）

3\. SecGuardianLog ＜FILE＞: ModSecurity 特別用來阻檔拒絕服務攻擊，可與 iptables 搭配來過濾惡意來源的 HTTP 存取，並將相關的資訊寫入所設定的檔案中
 4\. SecPdfProtect On|Off #設定是否要開啟 PDF XSS 的保護
 5\. SecUploadDir ＜DIR＞ #設定檔案上傳的目錄
 6\. SecUploadFileLimit ＜NUMBER＞ #設定上傳檔案的最大個數
 7\. SecServerSignature ＜STRING＞ #偽裝網站伺服器的資訊，可利用此參數來隱藏網站伺服器真正的版本資訊

如下例，以 ModSecurity 來保護網站伺服器的安全
 1\. 設定不允許某些來源 IP 的連線
 (1) 在 httpd.conf 加上如下的設定

```
＜IfModule mod_security2.c＞SecRuleEngine OnSecRule REMOTE_ADDR "^xxx\.xxx\.xxx\.xxx$" deny＜/ IfModule＞
```

(2) 利用瀏覽器瀏覽，如果符合所設定的來源 IP，即會得到 403 拒絕服務的訊息

2\. 設定當有人以 Web 弱點掃描器掃描（在此以 [Nikto](http://cirt.net/nikto2) 為例）時，即拒絕 (deny) 連線
 在 httpd.conf 加上如下的設定：

```
＜IfModule mod_security2.c＞SecRuleEngine OnSecRule REQUEST_HEADERS:User-Agent "nikto"  deny＜/ IfModule＞
```

讀者或許會覺得 ModSecurity 的規則集設定相當的複雜，使用者也可以參考在 ModSecurity 的官方網址上釋出的相關規則集。規則集分為免費版及付費版，其中免費版本的規則集，僅較付費版本晚更新，使用者會較晚點取得最新版本而已。讀者可至下列網 址取得規則集：wget [http://downloads.prometheus-group.com/delayed/rules/modsec-2.5-free-latest.tar.gz](http://downloads.prometheus-group.com/delayed/rules/modsec-2.5-free-latest.tar.gz)。解開後，將相關檔案置於 http 的 conf 目錄下後，根據需要在 httpd.conf 加上 Include conf/rule/*.conf，即可套用相關的 ModSecurity 規則集，來保護您的網站伺服器。