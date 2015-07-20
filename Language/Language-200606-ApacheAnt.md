# 跨越 IDE 的 Apache Ant
作者：李日貴，2006 年 6 月投稿。

Java Opensources for Web Development Part I：嘗試使用來自 Opensource 的小工具(1) Lession 2 : 跨越 IDE 的 Apache Ant Java 最基礎的編譯工具就是 JDK 之中的 javac 這個編譯器，但是往往在開發一個大型的專案之時，我們通常會利用 IDE 來完成相關的程式開發、除錯、編譯及包裝的動作，進而與一些應用伺服器或是資料庫做相關的整合。但是，往往開發習慣的不同，Java 之中有許多不錯的 IDE 工具讓大家使用，但是，該如何將一個專案讓大家都可以順利的匯入，我們可以利用 apache ant 這個小工具，另外，更可以利用 ant 這隻小螞蟻配合其他小工具進行連續性的軟體工程整合開發 (Continuous Integration : MartinFlower)[註] 。 其實講述 ant 操作的書籍已經很多了，我個人建議僅需要從 http://ant.apache.org 下載 ant 的 Binary 檔案，解壓縮到一個目錄後( 例如 c:\apache-ant-1.6.5\ ) 並且設定環境變數 `ANT_HOME = c:\apache-ant-1.6.5\` 以及 PATH 加上 `%ANT_HOME%\bin`，加上參考著範例直接練習就足夠應付大多數的狀況了。

不同於如 make其他編譯工具，ant 採用了 XML 作為執行的環境設定參考。當你執行 ant 的同時，他會先去尋找該執行目錄之下是否存放著 build.xml ，或是你可以強制 ant –f setup.xml 來執行 setup.xml 這個 ant 參考檔案。另外，通常我們除了使用 build.xml 來設定工作的項目之外，我們還會設定一個 build.properties，這個檔案通常會因為環境的不同進行一些資料庫或某些伺服器的名稱與位置設定等。

我們下載上次講到 jakarta commons-configuration 的 source code，解壓縮之後，你就應該可以在根目錄之中，察看到 build.xml 這個檔案，我們就來這個來做範例吧。

一個 xml 檔案最上方，通常會有對這份文件的宣告，如果有中文建議以 UTF-8 並且存成 UTF-8 的檔案。

    <?xml version="1.0" encoding="UTF-8"?>

所有的 build.xml 之中，都匯定義 <project>，我們通常還會設定預設 (default)的工作目標 (target) 以及工作的目錄 (basedir)。

    <project default="jar" name="commons-configuration" basedir=".">

    </project>

很明顯的，我們可以看到預設工作目標是 “jar” 這個 target，所以我們可以檢查到

    <target name="jar" description="o Create the jar" depends="compile,test">

    </target>

這時候，便可以發現 jar 和 compile 與 test 是具有相依性 (depends) ，換句話說，當我們在執行 jar 之前，會先去執行 compile 與 test 這個 target，這種繼承關係，就是 ant 能夠大受歡迎的地方。

至於到了 target 之中，我們到底要執行什麼任務 (task)，這就必須了解，ant 能夠協助我們完成什麼。

    * 檔案壓縮的任務 : <jar>, <zip>, <war> 等等

    * 稽核檢驗的任務 : <jdepend>, <jprobe> 等等

    * 檔案編譯的任務 : <javac>, <jspc> 等等

    * 系統部署的任務 : <serverdeploy> // 目前大多應用伺服器都支援熱部署

    * 文件產生的任務 : <javadoc>, <stylebook> 等等

    * EJB 專屬的任務 : <ddcreator>, <ejbc> 等等

    * 程式執行的任務 : <ant>, <exec>, <java> 等等

    * 檔案目錄的任務 : <mkdir>, <copy>, <delete> 等等

    * 日誌記錄的任務 : <record>

    * 郵件寄發的任務 : <mail>

    * 其他工具的任務 : <echo>, <script>, <sql> 等等

    * Properties 的任務 : <property>, <propertyfile> 等等

    * 遠端作業的任務 : <ftp>,<telnet>,<setproxy> 等等

    * 共同作業的任務 : <cvs>, <clearcase> 等等

    * 單元測試的任務 : <junit>, <test> 等等

當我們擁有了這些任務，就可以很快地製作出我們的專案。最重要的，我們更可以利用 ant 來協助我們連續式開發的方式。另外，大多的 java opensource 隨著原始碼都會附加 build.xml 讓使用者去 compile 出相關的系統， 所以學習 java opensource 的人不能不認識 ant 的操作。

1.註：http://www.martinfowler.com/articles/continuousIntegration.html
2.相關網址：Lession 1 : 我該如何存取一個設定檔？