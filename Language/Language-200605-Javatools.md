# Java Opensources for Web Development Part I：嘗試使用來自 Opensource 的小工具(1)
作者：李日貴，2006 年 5 月投稿。

###Lession 1 : 我該如何存取一個設定檔？

在我們撰寫一些系統的時候，往往需要設定一些基本的屬性，在使用 Java 進行 Web 開發之中，可以將相關設定放在 JNDI Server 再透 過 context lookup 重量級的方式來取得相關的屬性。不過，有時候 簡單的環境，不必耗時耗力去搞清楚如何去使用 Java naming 的技 術，往往不過是要讀取一些設定檔罷了，所以我們這時候可以利用 Jakarta commons-configuration 的小工具， 來讓我們簡化這方面 的工作。

什麼叫做一個設定檔？簡單來說，一個設定檔最重要的就是有屬性名 稱與屬性值，通常都是利用 String 的方式來存取，必要時候再進行 轉換物件型態為實質的屬性型態。話說回來，現在設定檔的撰寫方式 琳瑯滿目，Jakarta commons-configuration 是否可以滿足我們的需 求？我本身大多採用 Properties 或 XML 的方式在撰寫設定檔，也 許會有人將設定在資料庫之中，很令人高興的是 commons-configuration 的確可以涵蓋大多數人的需求。

由 http://jakarta.apache.org/commons/configuration/ 下載最新 的版本，將它解壓縮到一個目錄之後，我們可以看到該目錄之中，有 一個 commons-configuration-##.##.jar 並且將該 jar 檔案放到你 的 /WEB-INF/lib/ 之下，當 Application Server 重新載入該 webapp 之後， 就代表可以使用了。

因為 Servlet 可以設定在 webapp 啟動的同時，馬上呼叫。只需要 在 web.xml 設定 load-on-startup 即可，所以我們利用該技巧，將 相關的設定屬性值在剛開始的時候取出放到某個 instance 之中，這 樣就可以提供給其他程式直接使用。

####範例 1-1：透過 SystemInitServlet 載入 system-config.xml

system-config.xml 放在 /WEB-INF/conf 之下

    <config>

    <ip>10.10.1.1</ip>

    <account>jini</account>

    <password>jakarta99</password>

    <roles>

    <role>admin</role>

    <role>manager</role>

    <role>user</role>

    </roles>

    </config>

接著再利用 SystemInitServlet 中利用 XMLConfiguratin 取得 system-config.xml Configuration config = new XMLConfiguration (configPath);

    String ip = config.getString("ip");

    String account = config.getString("account");

    String password = config.getString("password");

    List roles = config.getList("roles.role");

這樣，我就可以自由利用這幾個設定在 system-config.xml 的屬性， 進行系統中的設定。

####範例 1-2：透過 SystemInitServlet 載入 system-config.properties

其實我們僅僅需要修改 Configuration 讀取的檔案位置，並且修改 為 properties 檔案名稱。最後利用 PropertiesConfiguration 就 可以讀取相關的資訊，也可以讀取 Collection 型態的集合屬性。

    Configuration config = new PropertiesConfiguration(configPath);

另外，我們可以 config.setProperties(“addproperty”, “this is added by configuration” ); 接著 config.save() 將相關設定 值存放到該 properties 的檔案之中。

總之，透過 commons-configuration 可以簡化您在存取設定檔案的 工作，就不用花時間去了解 FILE IO 的程式運作方式，更可以簡化 Java 存取 XML 的工作。 當然，隨著版本的演進，功能越來越多， 例如兩個設定檔的合併、plist 的支援等等，所以可預期的是，我們 將可以用到更多支援的設定檔方式。（下期待續）

####關於作者：

李日貴目前擔任松凌科技 (Softleader) 技術總監，同時是 Java 公 開原始碼報作者。專長包括：大型 J2EE 系統整合開發、金融交易平 台的研發、企業入口網站的規劃與分析、Java Open Source 的研究。