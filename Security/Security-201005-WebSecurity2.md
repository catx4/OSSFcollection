# Web Security 網站安全基礎篇（二）
作者：Allen Own，2010 年 5 月投稿。

###前言

我們在前篇已經有介紹一些駭客的思維以及攻擊手法，接下來我們要介紹身為開發者的我們，要如 何去確保自己的網站應用程式是安全無虞的。透過網路上 OWASP 組織提供的防護方法以及弱點介紹，我們可以很快的檢視自己的網站是否有安全弱點。以下我們會介紹 OWASP Top 10 十大網站安全弱點，以十項最常見的弱點來介紹網站的安全問題。

###Open Web Application Security Project 開放網站應用程式安全計畫

OWASP 是一個開放社群的非營利組織，致力於改善網站應用程式的安全性。OWASP Top 10 揭露常見的網站應用程式弱點，以供軟體開發安全參考。

[OWASP Top 10](http://www.owasp.org/index.php/Top_10%20%28http://www.owasp.org/index.php/Top_10)

通常我們會針對 OWASP Top 10 來進行基本的網站安全風險說明。目前 OWASP Top 10 釋出 2010 年版。十大風險如下：

* A1 – Injection（注入攻擊）
* A2 – Cross Site Scripting (XSS)（跨站腳本攻擊）
* A3 – Broken Authentication and Session Management（身分驗證功能缺失）
* A4 – Insecure Direct Object References（不安全的物件參考）
* A5 – Cross Site Request Forgery (CSRF)（跨站冒名請求）
* A6 – Security Misconfiguration（安全性設定疏失）
* A7 – Failure to Restrict URL Access（限制 URL 存取失敗）
* A8 – Unvalidated Redirects and Forwards（未驗證的導向）
* A9 – Insecure Cryptographic Storage（未加密的儲存設備）
* A10 – Insufficient Transport Layer Protection（傳輸層保護不足）

以下針對這十大風險做一個簡單的介紹。

####A1 – Injection（注入攻擊）

網站應用程式執行來自外部包括資料庫在內的惡意指令，SQL Injection 與 Command Injection 等攻擊包括在內。 因為駭客必須猜測管理者所撰寫的方式，因此又稱「駭客的填空遊戲」。

舉例來說，原本管理者設計的登入頁面資料庫語法如下：
```
$str = "SELECT * FROM Users WHERE Username='“.$user."' and Password=‘”.$pass."'“;

如果說 $user 以及 $pass 變數沒有做保護，駭客只要輸入「’ or ‘‘=’」字串，就會變成以下：

$str = “SELECT * FROM Users WHERE Username='' or ''='' and Password= '' or ‘’=‘’”;
```
如此一來，這個 SQL 語法就會規避驗證手續，直接顯示資料。

簡述駭客攻擊流程：
1. 找出未保護變數，作為注入點
2. 猜測完整 Command 並嘗試插入
3. 推測欄位數、Table 名稱、SQL 版本等資訊
4. 完整插入完成攻擊程序

防護建議：
* 使用 Prepared Statements，例如 Java PreparedStatement()，.NET SqlCommand(), OleDbCommand()，PHP PDO bindParam()
* 使用 Stored Procedures
* 嚴密的檢查所有輸入值
* 使用過濾字串函數過濾非法的字元，例如 mysql_real_escape_string、addslashes
* 控管錯誤訊息只有管理者可以閱讀
* 控管資料庫及網站使用者帳號權限為何

####A2 – Cross Site Scripting ( XSS )（跨站腳本攻擊）
網站應用程式直接將來自使用者的執行請求送回瀏覽器執行，使得攻擊者可擷取使用者的 Cookie 或 Session 資料而能假冒直接登入為合法使用者。

此為目前受災最廣的攻擊。簡稱 XSS 攻擊。攻擊流程如下圖：

1. 受害者登入一個網站
2. 從 Server 端取得 Cookie
3. 但是 Server 端上有著 XSS 攻擊，使受害者將 Cookie 回傳至 Bad Server
4. 攻擊者從自己架設的 Bad Server 上取得受害者 Cookie
5. 攻擊者取得控制使用受害者的身分

[![](http://www.openfoundry.org/images/100608/web%20security.png)](http://www.openfoundry.org/images/100608/web%20security.png)

防護建議：
* 檢查頁面輸入數值
* 輸出頁面做 Encoding 檢查
* 使用白名單機制過濾，而不單只是黑名單
* HP 使用 htmlentities 過濾字串
* .NET 使用 Microsoft Anti-XSS Library
* [OWASP Cross Site Scripting Prevention Cheat Sheet](http://www.owasp.org/index.php/XSS_%28Cross_Site_Scripting%29_Prevention_Cheat_Sheet)
* [各種 XSS 攻擊的 Pattern 參考](http://ha.ckers.org/xss.html)

####A3 – Broken Authentication and Session Management（身分驗證功能缺失）

網站應用程式中自行撰寫的身分驗證相關功能有缺陷。例如，登入時無加密、SESSION 無控管、Cookie 未保護、密碼強度過弱等等。

例如：

應用程式 SESSION Timeout 沒有設定。使用者在使用公用電腦登入後卻沒有登出，只是關閉視窗。攻擊者在經過一段時間之後使用同一台電腦，卻可以直接登入。

網站並沒有使用 SSL / TLS 加密，使用者在使用一般網路或者無線網路時，被攻擊者使用 Sniffer 竊聽取得 User ID、密碼、SESSION ID 等，進一步登入該帳號。

這些都是身分驗證功能缺失的例子。

管理者必須做以下的檢查：
* 所有的密碼、 Session ID 、以及其他資訊都有透過加密傳輸嗎？
* 憑證都有經過加密或 hash 保護嗎？
* 驗證資訊能被猜測到或被其他弱點修改嗎
* Session ID 是否在 URL 中暴露出來？
* Session ID 是否有 Timeout 機制？

防護建議：
* 使用完善的 COOKIE / SESSION 保護機制
* 不允許外部 SESSION
* 登入及修改資訊頁面使用 SSL 加密
* 設定完善的 Timeout 機制
* 驗證密碼強度及密碼更換機制

####A4 – Insecure Direct Object References（不安全的物件參考）

攻擊者利用網站應用程式本身的檔案讀取功能任意存取檔案或重要資料。進一步利用這個弱點分析網站原始碼、系統帳號密碼檔等資訊，進而控制整台主機。

例如：http://example/read.php?file=../../../../../../../c:\boot.ini。

防護建議：
* 避免將私密物件直接暴露給使用者
* 驗證所有物件是否為正確物件
* 使用 Index / Hash 等方法，而非直接讀取檔案

####A5 – Cross Site Request Forgery (CSRF)（跨站冒名請求）

已登入網站應用程式的合法使用者執行到惡意的 HTTP 指令，但網站卻當成合法需求處理，使得惡意指令被正常執行。

舉例來說，攻擊者在網站內放置了```<img src="http:/server.com/send.asp?to=xxx" border="0" /> ```，受害者讀取到此頁面之後，就會去 server.com 主機執行 send.asp 惡意行為。

例如 Web 2.0 時代的社交網站等等，都是 CSRF 攻擊的天堂。

防護建議：
* 確保網站內沒有任何可供 XSS 攻擊的弱點
* 在 Input 欄位加上亂數產生的驗證編碼
* 在能使用高權限的頁面，重新驗證使用者
* 禁止使用 GET 參數傳遞防止快速散佈
* 使用 Captcha 等技術驗證是否為人為操作

或者參考 OWASP 所提供的 CSRF Solution
* [OWASP CSRFTester Project](http://www.owasp.org/index.php/CSRFTester)
* [OWASP CSRFGuard Project](http://www.owasp.org/index.php/Category:OWASP_CSRFGuard_Project)
* [OWASP CSRF Prevention Cheat Sheet](http://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29_Prevention_Cheat_Sheet)

####A6 – Security Misconfiguration（安全性設定疏失）

系統的安全性取決於應用程式、伺服器、平台的設定。因此所有設定值必須確保安全，避免預設帳號、密碼、路徑等。甚至被 Google Hacking 直接取得攻擊弱點。

防護建議：
* 軟體、作業系統是否都有更新到最新版本？是否都有上最新 Patch?
* 不需要的帳號、頁面、服務、連接埠是否都有關閉？
* 預設密碼是否都有更改？
* 安全設定是否都完備？
* 伺服器是否都有經過防火牆等設備保護？

各種設備、系統的預設密碼，都可以在網路上找到一些整理資料。
 [http://www.phenoelit-us.org/dpl/dpl.html](http://www.phenoelit-us.org/dpl/dpl.html)
 [http://www.routerpasswords.com/](http://www.routerpasswords.com/)
 [http://www.defaultpassword.com/](http://www.defaultpassword.com/)

####A7 – Failure to Restrict URL Access（限制 URL 存取失敗）

網頁因為沒有權限控制，使得攻擊者可透過網址直接存取能夠擁有權限或進階資訊的頁面。例如管理介面、修改資料頁面、個人機敏資訊頁面洩漏等等。

舉例來說，
```
 /admin
 /backup
 /logs
 /phpmyadmin
 /phpinfo.php
 /manage
```
 這些都是常見的路徑及檔案。攻擊者只要猜測到，就可以操弄主機。

防護建議：
* HTTP Service 直接限制來源 IP
* 使用防火牆阻擋
* 密碼授權加密頁面
* 網站架構最佳化

####A8 – Unvalidated Redirects and Forwards（未驗證的導向）

網頁應用程式經常將使用者 Forward 或 Redirect 至其他頁面或網站，沒有驗證的機制。攻擊者可將受害者導向至釣魚網站或惡意網站，甚至存取受限制的資源。

例如：
 [http://example.cc/redir.jsp?url=evil.com](http://example.cc/redir.jsp?url=evil.com)
 [http://example.cc/func.jsp?fwd=admin.jsp](http://example.cc/func.jsp?fwd=admin.jsp)
 [http://g.msn.com/9SE/1?http://xxx.com](http://g.msn.com/9SE/1?http://xxx.com)

防護建議：
* 非必要時避免使用 Redirect 及 Forward
* 驗證導向位置及存取資源是合法的

####A9 – Insecure Cryptographic Storage（未加密的儲存設備）

網 站應用程式沒有對敏感性資料使用加密、使用較弱的加密演算法或將金鑰儲存於容易被取得之處。加密演算法是安全防護的最後一道防線，當駭客取得了帳號密碼， 可以簡單的使用一些破解軟體甚至線上服務進行破解。例如 Cain & Abel，MD5 Reverse Lookup 等。

防護建議：
* 使用現有公認安全的加密演算法
* 減少使用已有弱點的演算法，例如 MD5 / SHA-1，甚至更簡單的加密法
* 安全的保存私鑰

####A10 – Insufficient Transport Layer Protection（傳輸層保護不足）

網頁應用程式未在傳輸機敏資訊時提供加密功能，或者是使用過期、無效的憑證，使加密不可信賴。

例如：攻擊者竊聽無線網路，偷取使用者cookie；網站憑證無效，使用者誤入釣魚網站。

防護建議：
* 盡可能的使用加密連線
* Cookie 使用 Secure Flag
* 確認加密憑證是有效並符合 domain 的
* 後端連線也使用加密通道傳輸
* [http://www.owasp.org/index.php/Transport_Layer_Protection_Cheat_Sheet](http://www.owasp.org/index.php/Transport_Layer_Protection_Cheat_Sheet)
```
Cookie Secure Flag 設定：
 ● PHP setcookie ("TestCookie", "", time() - 3600, “/", ".example.com", 1);
 ● JSP cookie.setSecure(true);
 ● ASP.NET cookie.Secure = True;
```
###小結

以上 OWASP Top 10 包含了大部分常見的網站安全弱點，但並不是只有這些。管理者必須時時注意最新的資安消息，並且對自己的主機定時進行更新，檢查系統記錄檔，絕不可有僥倖之心。如此一來，才能確保主機處於安全的狀態。

###附錄

以下附上常見的弱點資料庫以及網站淪陷資料庫，管理者可以定期瀏覽最新的資訊安全消息，並且檢查自己的網站有沒有在淪陷資料庫榜上有名。

弱點資料庫
 [CVE - Common Vulnerabilities and Exposures (CVE)](http://cve.mitre.org/)
 [SecurityFocus](http://www.securityfocus.com/bid)
 [National Vulnerability Database](http://nvd.nist.gov/)
 [VUPEN Security](http://www.vupen.com/)

網站淪陷資料庫
 [TW 網站淪陷資料庫](http://www.itis.tw/compromised)
 [Zone-H](http://www.zone-h.org/archive)
 [](http://www.xssed.com/archive)

####作者簡介

翁浩正 ( Allen Own )
 網駭科技技術顧問，資安團隊 NISRA 創辦人，多年網路管理、資訊安全、駭客攻防技術經驗，自覺什麼都沒有，只有滿腔熱血。

* [Mail](http://%20%3Cscript%20language=%27JavaScript%27%20type=%27text/javascript%27%3E%20%3C%21--%20var%20prefix%20=%20%27mailto:%27;%20var%20suffix%20=%20%27%27;%20var%20attribs%20=%20%27%27;%20var%20path%20=%20%27hr%27%20+%20%27ef%27%20+%20%27=%27;%20var%20addy60201%20=%20%27allenown%27%20+%20%27@%27;%20addy60201%20=%20addy60201%20+%20%27gmail%27%20+%20%27.%27%20+%20%27com%27;%20document.write%28%20%27%3Ca%20%27%20+%20path%20+%20%27%5C%27%27%20+%20prefix%20+%20addy60201%20+%20suffix%20+%20%27%5C%27%27%20+%20attribs%20+%20%27%3E%27%20%29;%20document.write%28%20addy60201%20%29;%20document.write%28%20%27%3C%5C/a%3E%27%20%29;%20//--%3E%20%3C/script%3E%20%3Cscript%20language=%27JavaScript%27%20type=%27text/javascript%27%3E%20%3C%21--%20document.write%28%20%27%3Cspan%20style=%5C%27display:%20none;%5C%27%3E%27%20%29;%20//--%3E%20%3C/script%3E%E9%80%99%E5%80%8B%20E-mail%20%E5%9C%B0%E5%9D%80%E5%B7%B2%E7%B6%93%E8%A2%AB%E9%98%B2%E6%AD%A2%E7%81%8C%E6%B0%B4%E6%83%A1%E6%84%8F%E7%A8%8B%E5%BC%8F%E4%BF%9D%E8%AD%B7%EF%BC%8C%E6%82%A8%E9%9C%80%E8%A6%81%E5%95%9F%E7%94%A8%20Java%20Script%20%E6%89%8D%E8%83%BD%E8%A7%80%E7%9C%8B%20%3Cscript%20language=%27JavaScript%27%20type=%27text/javascript%27%3E%20%3C%21--%20document.write%28%20%27%3C/%27%20%29;%20document.write%28%20%27span%3E%27%20%29;%20//--%3E%20%3C/script%3E)
* [Blog](http://allenown.blogspot.com)
* [Twitter](http://twitter.com/allenown)
* [Plurk](http://www.plurk.com/allenown)