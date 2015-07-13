# OpenMeetings - 自由軟體線上會議系統
作者：Louie Chen，2012 年 10 月投稿。

由於需要用到多人視訊功能，但是又要能夠分享桌面，目前似乎沒有自由軟體能做到如此。所以就試著找看看，找到 apache foundation 的 openmeetings，這才知道有這麼一套 "自由軟體線上會議" 程式存在。看了些文件後，真是覺得功能符合我的需求，於是 花了幾天時間來研究安裝建置及使用，做成筆記。

### openmeetings 特點：

*   自由軟體
*   視訊及語音會議，也可打文字聊天 (chat)。
*   白板可展示圖片、影片、簡報等等（可同時多個白板）。
*   分享及控制桌面（類似 vnc），並可將桌面錄影起來。
*   詳細的權限控管（防止講到一半被干擾）。
*   內建民調系統，方便決策用。
*   預約會議，可以 email 通知。
*   支援外掛程式，擴充功能。

### 安裝前準備：

小弟的環境是以 debian testing (wheezy) 安裝，若是其他 distribution，請自行修改。

命令是以一般使用者即可安裝，只有在安裝 package 時 apt-get 要 root 權限，在此是用 sudo。

在安裝 openmeetings 前，有些東西是必要的，要先準備好：

1.  JRE (java runtime environment)，安裝 openjdk。```$ sudo apt-get install openjdk-7-jre```

2.  要能夠分享桌面，必須要能執行 .jnlp 檔，裝上 icedtea-netx
    ```$ sudo apt-get install icedtea-netx```

3.  將桌面錄製成影片或滙入 .avi, .flv, .mov, .mp4 到白板要安裝工具。
    ```$ sudo apt-get install ffmpeg sox swftools```

    debian wheezy 的 swftools 裡並沒有 pdf2swf 這個指令，所以無法上傳 pdf 檔來做簡報，若一定要有，請自行從原始碼編譯。

4.  在白板中匯入圖片，要圖片轉檔工具。
    ```$ sudo apt-get install imagemagick```

5.  要在白板中匯入 office 檔，要裝上 libreoffice 或 openoffice 及轉檔工具 jodconverter。
    ```$ sudo apt-get install libreoffice```

    openmeetings 滙入 .doc, .docx, .ppt, .pptx，都是用 libreoffice 或 openoffice 去做轉換動作，所以在 openmeetings 這台伺服器上一定要裝 office。而呼叫 office 來做轉換的動作，則由 jodconverter 來完成。

    因為 openmeetings 在檔案上傳後，都是轉成 pdf，所以要安裝 ghostscript。
    ```$ sudo apt-get install ghostscript```

6.  上傳的 office 要支援中文，系統上要裝 utf8 中文字型。
    ```$ sudo apt-get install ttf-arphic-uming ttf-arphic-ukai```

    若之前舊文件有用 big5 字型，也要安裝 big5 字型。
    ```$ sudo apt-get install ttf-arphic-bsmi00lp ttf-arphic-bkai00mp```

    若文件用到 windows 的細明體的話，預設 linux 並沒有，所以轉換後會變亂碼，直接將字型從 windows 拷貝到 linux 下即可。  
    ```$ sudo mkdir /usr/share/fonts/truetype/windows/```   
    ```$ sudo cp mingliu.ttc /usr/share/fonts/truetype/windows/```   

### 開始安裝：

到 http://incubator.apache.org/openmeetings/ 抓 openmeetings 回來。

解開到指定目錄：

```$ mkdir /media/share/apps/openmeetings/ ```   
 ```$ tar xvf apache-openmeetings-incubating-2.0.0.r1361497-14-07-2012_1108.tar.gz -C /media/share/apps/openmeetings```

由於 debian wheezy 裡的 jodconverter 是 2.2.2 版，必須將 office 啟動成一個 service 在背景跑，listen 一個 tcp port，然後 jodconverter 2.x 再透過 tcp port 去要求 office 做轉換動作。

但是 jodconverter 3.0 則 office 不用在背景跑，jodconverter 3.x 會去呼叫 office 來轉檔，轉完後會關閉 office，所以用 3.x 版比較省資源 。

jodconverter 2.x:   
```$ sudo apt-get install jodconverter```
 ```$ soffice --headless --nofirststartwizard --accept="socket,host=localhost,port=8100;urp"```
 會 listen 在 localhost:8100, 而 jodconverter 2.x 在轉換時會自行去跟 office 溝通。

或使用 jodconverter 3.x:
 下載 jodconverter 3.0:
 [http://code.google.com/p/jodconverter/downloads/list](http://code.google.com/p/jodconverter/downloads/list)
 解開至 openmeetings 目錄裡  
``` $ unzip jodconverter-core-3.0-beta-4-dist.zip -d /media/share/apps/openmeetings/ jodconverter``` 是 3.0-beta-4, 若下次 beta 5 或正式版釋出，則路徑會不同，要再修改，所以預先做個連結，以後不用再到 openmeetings 裡修改路徑，將新版做個連結即可。   
 ```$ cd /media/share/apps/openmeetings/```   
``` $ ln -sf jodconverter-core-3.0-beta-4 jodconverter-core```   

預設 openmeetings 資料庫是用 apache derby database，若要改成 mysql 則需在此先準備好。

1.  mysql 伺服器部份設定：首先 mysql 伺服器的 charset 一定要設成 utf8，否則無法安裝成功。
     $ sudo apt-get install mysql-server
     $ sudo vi /etc/mysql/my.cnf
     [mysqld]
     character_set_server=utf8
     重新啟動 mysql server
     $ sudo /etc/init.d/mysql restart

2.  安裝 Jconnector下載 [http://www.mysql.com/downloads/connector/j/](http://www.mysql.com/downloads/connector/j/)
     解開後，
     將 mysql-connector-java-5.1.22-bin.jar 放到指定目錄下
     $ cp mysql-connector-java-5.1.22-bin.jar /media/share/apps/openmeetings/webapps/openmeetings/WEB-INF/lib/

3.  openmeetings 呼叫 mysql 設定：將 mysql 專用設定檔覆蓋掉原本的：
     $ cd /media/share/apps/openmeetings/webapps/openmeetings/WEB-INF/classes/META-INF/
     $ cp mysql_persistence.xml persistence.xml
     修改 persistence.xml 裡面 mysql server 位置、使用者、密碼。

    由於 openmeetings 要使用 mysql 時，是以 mysql root 權限去使用，這樣風險太大。沒必要的權限不要開。所以先手動建立 openmeetings 資料庫，然後允許 your_openmeetings_user 這個使用者只能使用這個資料庫，其他的不能動。

    以 mysql root 連到 mysql server 下指令：
     mysql > create database openmeetings;
     mysql > grant all on openmeetings.* to your_openmeetings_user@localhost identified by 'your_openmeetings_password';

    密碼設為 your_openmeetings_password, 這樣 client 只能以 your_openmeetings_user 使用者由本機連進來，並且僅能使用 openmeetings 這個資料庫，若 openmeetings 伺服器和 mysql 不在同一台，請改 localhost。

### 啟動 openmeetings server：

```$ cd /media/share/apps/openmeetings/```   
 ```$ ./red5.sh```   
 假如你的 openmeetings 伺服器需要更好效能、能承受更高負載，則啟動時請改用：
 ```$ ./red5-highperf.sh```

等個約 30 秒，若沒問題，伺服務啟動完成，會 listen tcp port 5080, 1935, 8088 和其他幾個 port。

openmeetings 本身就有內建 http server，不用額外配合 apache web server。

Port 5080: HTTP (瀏覽器登入及檔案上傳下載)  
 Port 1935: RTMP (Flash Stream and Remoting/RPC)  
 Port 8088: RTMP over HTTP-Tunneling (rtmpT)   

因此在 openmeetings server 上的防火牆要打開 tcp 5080, 1935, 8088， 其他的是 openmeetings 內部自己使用就不需要。

打開瀏覽器，開啟 openmeetings web-installer, 開始安裝：
 [http://localhost:5080/openmeetings/install](http://localhost:5080/openmeetings/install)

開啟頁面有一堆說明，預設 openmeetings 資料庫是用 apache derby dat abase, 在真實 production run 環境下，可改用 mysql、postgres、 IBM DB2、oracle 等等。不管那些說明，直接按 "Continue with STEP 1" 進入安裝畫面。

開始要先填上使用者及密碼，電子郵件及時區，這個使用者即為 "超級使用者" 身份。電子郵件則是忘記密碼時重置密碼用及傳送會議連結。

在 Configuration 部份，預設是系統會寄一封信給新註冊的使用者，使用者收信後，點選 email 上的連結啟動帳號，若是不想這麼麻煩，可修改

Send Email to new registered Users (sendEmailAtRegister) 		No  
New Users need to verify their EMail (sendEmailWithVerficationCode) 		No 

預設電子郵件伺服器是指向 localhost, 所以本機 mail server 要 listen tcp port 25；另外也可使用外部 smtp server, 如 google 的 smtp.gmail.com 的方式。   
SMTP-Server (smtp_server) 		smtp.gmail.com   
SMTP-Server Port (default Smtp-Server Port is 25) (smtp_port) 		587   
SMTP-Username (email_userpass)  
		gmail 使用者名稱(自行設定)   
SMTP-Userpass (email_userpass) 		gmail 密碼(自行設定)   
Enable TLS in Mail Server Auth 		Yes   

在 Converters 部份，若是有在 PATH 裡的執行檔，則不用在此設定，可直接找到。因此只要調整 jodconverter 3.x 版的 JOD Path：  
 ```JOD Path: ./jodconverter-core/lib```

其他的都保持原狀，拉到最底下按 INSTALL 按鈕開始安裝動作，我的電腦持續卡住約 10 分鐘才安裝完成(使用 derby database)，若用 mysql 大約 2 分鐘，要有點耐心，完成會跳到另一個視窗，千萬不要一直 refresh 或重開瀏覽器，否則會安裝失敗。

### 使用：

安裝完成後，直接按 Enter the Application 到登入畫面。登入畫面是用 flash 的，所以瀏覽器要裝 adobe flash plugin, 小弟試用過 gnash, 無法使用。 也可直接開啟網址使用，網址為 [http://localhost:5080/openmeetings](http://localhost:5080/openmeetings)

以剛才建立的超級使用者登入 (Sign in) 或新註冊一個，按 (Not a member?) ，註冊時可選擇語言，有正體中文可選，可是翻譯並不完整而且很多是大陸用語，為求一致性，所以選英文介面來介紹。

#### Dashboard：

剛登入時是位於 "Dashboard" ，如圖1。

[![alt](http://www.openfoundry.org/images/121009/OpenMeetings/OpenMeetings_01.jpg)](http://www.openfoundry.org/images/121009/OpenMeetings/OpenMeetings_01.jpg)   

▲圖1 Dashboard

上方的功能表項目：
 【Home】回到 "Dashboard" 及開啟月曆觀看及預約會議。
 【Recordings】觀看及下載之前已錄製好的影片。
 【Rooms】各會議室目前狀況（公共，私人，個人會議室）。每間會議室有不同的屬性，如 Interview Room 就是一對一。
 【Administration】若是以超級使用者登入，則會出現此選單，內容有：

1.  Users : 管理及修改使用者資料，可於此設定使用者等級 (user, moderator, admin)。
2.  Connections : 觀看目前伺服器上線的使用者，必要時可將他踢出伺服器。
3.  Usergroups : 將使用者分群組，如系統組、推廣組。
4.  Conference rooms : 檢視及微調各個會議室的詳細屬性，若於某會議室裡的 「Moderated」核取方塊內打抅，而且某位進入的使用者 等級為 moderator，則此使用者會自動擁有會議主持人權限。
5.  Configuration : 修改 OpenMeetings 伺服器的細項設定。
6.  Language editor : 從這裡做訊息翻譯動作。
7.  LDAP : 結合 LDAP 使用的設定。
8.  Backup : 使用者資料匯出及匯入，用於伺服器升級時。

左上角是個人資料及大頭照。

右上角 "Profile" 可修改個人詳細資料。

下方 "My rooms" 活頁標籤為個人會議室及你必須參加的會議列表，按每個會議室的整條長方型條可看目前會議室內的人員，要加 入會議室則點 [Enter]。

"My rooms" 的右邊是 "Chat"，可看目前線上人員並與他溝通。也可邀請某位目前在線上的人進某個會議室。

右邊是簡易使用說明及捷徑。按下 [Start] 可進入會議室列表（和上方 "Rooms" 一樣）。接著請按下 [Start]，即可看到會議室列表 。

在公開會議室選 "conference room with micro option set" 這間會議室並點 [Enter] 。

#### 會議室：

首先會彈出一個 "Choose device" 視窗，選擇 webcam、麥克風及解析度，按 [Start recording test] 確認 webcam 有看到影像，沒問題按 [Start conference] 進入會議室，如圖2。

[![alt](http://www.openfoundry.org/images/121009/OpenMeetings/OpenMeetings_02.jpg)](http://www.openfoundry.org/images/121009/OpenMeetings/OpenMeetings_02.jpg)  

▲圖2 會議室

第一個進入會議室的人，會自動成為會議主持人，擁有所有權限。（使用白板、桌面分享及錄影、控制遠端桌面、是否有權限獨 佔麥克風）。

左上角為功能表：
 【EXIT】離開會議室
 【Files】上傳檔案
 【Actions】多項功能

1.  Send invitation: 將目前會議室直接以連結網址寄出電子郵件來邀請人加入會議。
2.  Share/record screen: 分享目前桌面及桌面錄影。
3.  Apply to be moderator: 要求擁有會議主持人的所有權限
4.  Apply for whiteboard access: 要求使用白板權限。
5.  Apply for audio/video access: 要求分享聲音及影像權限。
6.  Create a poll: 建立問卷調查。
7.  Poll results: 看問卷結果。
8.  Vote: 問卷調查投票。
9.  Edit default settings: 更改預設白板設定。

最右上角可看到目前會議室名稱、要求取得會議主持人權限及分享自己的桌面及錄影。

點選分享及桌面錄影圖示，會出現視窗詢問要以什麼應用程式來開啟 .jnlp 檔，若在 client 機器上有裝 icedtea-netx，則此時可選 IcedTea Java Web Start 來使用。可將桌面分享給會議中的人，並可同時錄影起來，如圖3。

按左上角 [Start sharing]，會立即將自己的桌面分享給會議室內的所有人員。

右上 [Close] 關閉此視窗。

左邊中間可微調要分享桌面的大小，預設是全螢幕，可以拖拉只分享某一部份。

右邊中間可調整錄影的解析度。

下方 [Start Recording] 開始錄影， [Stop recording] 停止錄影，可以不分享桌面，單純只錄影。

在按下停止錄影後，伺服器還在背景做轉換動作，所以要過個幾分鐘，影片才會出現在 Dashboard 裡的 Recordings，不要認為為什麼沒看到影片。

[![alt](http://www.openfoundry.org/images/121009/OpenMeetings/OpenMeetings_03.jpg)](http://www.openfoundry.org/images/121009/OpenMeetings/OpenMeetings_03.jpg)  
▲圖3 桌面分享及錄影視窗

左邊：

"Users" 活頁標籤為參加會議室人員列表及權限，於此要求權限或開放權限給某人。

"Files" 活頁標籤可上傳檔案放到白板展示。在上傳完成後，檔案位於 "My files (home drive)"，只能自己能存取，用滑鼠將檔案拖到 "Room files (public drive)" 則參加會議的人都可以存取，不同會議室有不同的 public drive。將要展示的文件或圖片，以滑鼠左鍵拖到右 方白板上，若是多頁檔案，則下方 "Document properties" 綠色箭頭可切換上、下頁；或是於左方檔案列表檔案上按滑鼠右鍵，選 "Open document"，則可看到每一頁內容，直接在頁面上按滑鼠左鍵即可展示到白板。

左下角視窗為動作視窗，若有要求權限時，在會議主持人的左下角出現要求訊息，會議主持人可於此同意或拒絕權限。

右邊一大塊即為白板，預設只有一個白板，可按上方綠色內有白色加號圓圈來新增另一個白板。

白板下方 "Properties" 可調整畫筆顏色及方塊或橢圓填滿的顏色及投影片上、下頁功能。

最下方 "Chat" 可打字發言。

會議流程大致如下：

1.  進入會議室，先將 webcam 及麥克風設定好。
2.  至少要求使用白板及獨佔麥克風權限，或要求全部權限才能做簡報，展示投影片。
3.  點選 "Files" 活頁標籤並上傳檔案。
4.  將檔案以滑鼠左鍵拖到右方白板。
5.  點選自己的麥克風視窗來獨佔麥克風，會同時將別人的麥克風靜音。
6.  開始簡報。

經過一陣子的研究，大致弄懂 OpenMeetings 的操作，整個功能非常完整，若再加上外掛來運作，真是殺手級軟體，不愧是 "自由軟體線上會議方案"。

### 參考文件：

[http://jainan.blogspot.tw/2011/10/202- windows-2003-install-openmeetings.html](http://jainan.blogspot.tw/2011/10/202-windows-2003-install-openmeetings.html)

[http://incubator.apache.org/openmeetings/](http://incubator.apache.org/openmeetings/)