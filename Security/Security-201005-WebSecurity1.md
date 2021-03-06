# Web Security 網站安全基礎篇（一）
作者：Allen Own，2010 年 5 月投稿。

###前言

網路安全一直都是相當重要的議題，網站就是所有使用者的入口，提供各種服務的網站也有著各式各樣的風險，因此網站一直以來都是駭客攻擊的首要目標。 一個網站的安全與否取決於很多因素，作業系統的版本、網站應用程式的撰寫、架構的設計、使用者的設定等等。只要任何一個環節出問題，都會導致整個網站暴露 於風險之中。技術層面來看，管理者必須要時時跟隨系統應用程式的腳步，注意最新的消息、技術和攻擊手法，因應進行修補及防護，才能確保系統處於安全的狀態。

###「沒有不安全的系統，只有不安全的人」

人們在探討資訊安全的時候，往往會有一個想法，就是什麼系統是不安全的，什麼軟體是不安全的，甚至什麼公司出產的軟體都是有問題的。但是其實大家必 須要有一個觀念，多數的情況，不安全的其實是人，而不是系統。一個再安全的系統，只要管理者做出了錯誤的設定，攻擊者就可以因此直接進入到我們的系統。在 我們遇到的案例中，多數的弱點都是由管理者的疏忽所發生的。

為什麼資訊安全的重要性越來越高？主要是因為網路為主的應用大量增加，開發技術也越來越簡易，使用攻擊程式進行攻擊的門檻也日漸降低。因此只要有心人士找到相對應版本的攻擊程式，就可以輕易的對我們的主機進行攻擊。

###Google Hacking

攻擊的門檻到底有多低呢？利用 Google Hacking 這樣的手法，就可以對一個網站進行攻擊。Google Hacking 是一個無技術門檻的攻擊手段，藉由網站管理者錯誤的設定及疏失，以及 Google 搜尋 Index 的能力，搜索可攻擊的網址。攻擊者可以因此取得網站資訊、攻擊標的和暴露的弱點等等。

Google Hacking 到底有多危險呢？以下圖為例，我們只要在網路上搜尋一些字串，就可以取得一些令人驚訝的資訊。例如說我們搜尋「密碼表 ext:xls」，就會找到以下的資訊。有心人士就可以因此利用以下的資訊進行進一步的攻擊。還有其他攻擊手法，可以找到其他系統資訊，我們在這邊就不多 做介紹。如果想要知道更多 Google Hacking 的語法，可參考以前的 「[Google Hacking Database](http://johnny.ihackstuff.com/ghdb/)」，可以找到一些相關的範例。

[![](http://www.openfoundry.org/images/100525/websecurity01.png)](http://www.openfoundry.org/images/100525/websecurity01.png)

這些問題有一大部分都是因為人為的因素。因此，在探究系統的安全之前，管理者必須先從自身做好資訊安全的規劃。從技術、知識到思維，都要做好建設。

###駭客想的跟你不一樣！

在了解系統該怎麼防護之前，一定要先了解駭客的思維，因為駭客想的跟你不一樣！一般來說，駭客攻擊的流程可以分為五個步驟：偵查、掃描、取得權限、維持權限、清除足跡。

####偵查 (Reconnaissance)
攻擊者準備攻擊之前進行的偵查，找尋目標的相關資訊，以利之後的攻擊。

####掃描 (Scanning)
掃描目標主機的弱點，取得主機作業系統、服務和運作狀況等資訊。

####取得權限 (Gaining Access)
利用系統弱點取得主機權限。

####維持權限 (Maintaining Access)
維持目前取得的權限，以便日後再次存取而不需繁雜的步驟。

####清除足跡 (Clearing Tracks)

清除入侵的痕跡。

###真實攻擊事例

讓我們來看看一個真實的駭客案例。

駭客鎖定了一個受害者網站，從 Google 上搜尋此網站所有的相關資訊。並利用 Google Hacking 技術找到所有網站暴露的目錄及檔案。

接著，經由網站找到的弱點，嘗試進行攻擊。並且植入 Web Shell 後門，透過網頁連入操控主機。例如：http://victim.org/webshell.php?cmd=oxox

連入主機後發現帳號權限不足，因此從主機內尋找可用的資訊。例如從 /etc/passwd 找尋可用帳號，利用 /var/log 查詢系統可用資訊，或者搜尋有無 setuid 檔案可供利用。

下個步驟，駭客發現主機 Kernel 版本過舊，有可提權的弱點。因此撰寫或搜尋 Exploit 進行攻擊，取得 root 管理者權限。

得到權限後，為了方便連入主機，放置後門供日後使用。常用的手法例如建立主機帳號、/etc/rc.d 放置後門，代換 sshd 系統服務等。

最後，清除自己的足跡，例如指令記錄檔及系統紀錄。

```
 ~/.history
 ~/.bash_history
 /var/log/*
```

這是一個很簡單的駭客思維，但是只要我們了解這些，就能夠知道駭客是怎麼進行一次完整的入侵行為，進而知道該如何去防禦。

###該如何去確保網站安全？

就如同前面所講，網站是一個窗口，使用者經由網站取得資訊，攻擊者也經由網站進行攻擊。因此網站的防護就是第一道防線。如果第一道防線都沒有做好， 那攻擊者就可以長驅直入。那我們到底要怎麼去確保網站安全呢？目前網路上 OWASP 組織有提供各種防護方法以及弱點介紹，讓網站管理者依循維護及修改。

我們會在下篇繼續有關 OWASP 的詳細介紹。

####作者簡介

翁浩正 ( Allen Own )

網駭科技技術顧問，資安團隊 NISRA 創辦人，多年網路管理、資訊安全、駭客攻防技術經驗，自覺什麼都沒有，只有滿腔熱血。

*   Mail： [allenown@gmail.com](mailto:allenown@gmail.com)
*   Blog：[http://allenown.blogspot.com/](http://allenown.blogspot.com/)
*   Twitter：[http://twitter.com/allenown](http://twitter.com/allenown)
*   Plurk：[http://www.plurk.com/allenown](http://www.plurk.com/allenown)