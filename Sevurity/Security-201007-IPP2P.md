#以 IPP2P 控管 P2P 軟體 
作者：[老薯條](http://vulscan.wynetech.com.tw) ，2010 年 11 月投稿。 


###前言 
隨著 internet 的風行，相關的資安問題也層出不窮，除了傳統的病毒及網路釣魚等威脅外，不當使用 P2P 軟體所衍生的資安問題也日漸受到重視，在本文中，筆者將應用開源碼社群中的 IPP2P 搭配 netfilter（linux 系統上最有名的防火牆軟體，即一般人所熟知的 iptables）機制，來建構一個可控管 P2P 軟體的防火牆系統。在本方案中所使用到的軟體如下表所示： 

* **軟體名稱 / 說明 / 官方網址** 
* Fedora 11 / 所使用的作業系統 /[http://fedoraproject.org/](http://fedoraproject.org/) 
* L7-filter / Netfilter 的外掛核心模組，將新增此模組才可讓 Netfilter 具有應用層的封包分析能力 / [http://l7-filter.sourceforge.net/](http://l7-filter.sourceforge.net/)  
* Kernel 2.6.28 / 本次所使用的Linux核心程式，L7-FILTER 需重新編譯核心，在本方案中，筆者採用 2.6.28版本 /  [http://www.kernel.org/](http://www.kernel.org/) 
* iptables 1.4.3 / Netfilter的管理應用程式，L7-FILTER 需重新編譯 iptables，新增 L7 的比對項目 [http://netfilter.org/](http://netfilter.org/) 
* IPP2P / 阻檔 P2P 軟體程式 / [http://www.ipp2p.org/](http://www.ipp2p.org/) 
* 相關 patch 檔 / Ipp2p 的相關修正檔（包含 iptables 及 kernel）的修正檔 /  [http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-iptables-1.4.0.patch](http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-iptables-1.4.0.patch) [http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-iptables-1.4.1.patch](http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-iptables-1.4.1.patch) [http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-kernel-2.6.28.patch](http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-kernel-2.6.28.patch) 

###什麼是 (netfilter/iptables) 
Netfilter 機制是 linux 系統上最有名的 L4 (Level 4) 防火牆，其中 netfilter 指的是核心處理的模組，而 iptables 即為設定管理 Netfilter 的使用者程式。所謂 L4 意指 Netfilter 運作 OSI (Open system Interconnection) 模型的第四層（國際標準組織所提出的共通標準化的通訊協定標準），OSI 模型共分七層，各層各司其職，概述如下（由下而上）：
####實體層 (Physical Layer) 
主要定義底層傳輸介面的電氣媒體 (MEDIA)，傳輸方法 (TRANSMISSION METHOD) 與佈線方式 (TOPOLOGY) 等等。 
####資料連結 (Data LinK Layer) 
定義如何確保資料正確傳輸，及將資料切割並加上來源與目的地位址與資料長度等訊息，包裝成頁框 (FRAME)，在此層中具有 MAC 資訊。 
####網路層 (Network Layer) 
負責資料繞徑 (ROUTING )，包括轉換地 (TRANSLATES ADDRESSES)，尋找最佳路徑 (BEST ROUTING) 與管理流量 (TRAFFIC)，在此層中具有 IP 等資訊。 
####傳輸層 (Transport Layer) 
確保資料到達順序與正確性，在此層中具有埠等資訊。 
####會談層 (Session Layer) 
定義連結建立與結束的對話。 
####表達層 (Presentation Layer) 
處理資料格式，包括格式轉換，加密 (ENCRYPTION) 與解密 (DECRYPTION)，壓縮 (COMPRESSION) 與還原。 
####應用層 (Application Layer) 
定義供應用程式存取的介面與功能。   
  
由上可知，netfilter 即是運作在傳輸層中，所能掌握的資訊即為 IP、埠及 MAC 等相關資訊。這也是為什麼 netfilter 要控管 P2P 軟體需要新增 IPP2P 模組的原因。在未新增 IPP2P 模組之前，netfilter 僅能以 IP 位址（來源或目的），埠資訊（來源或目的），MAC 資訊（來源或目的）控管來往的封包。如此對於使用固定埠的網路服務或許有控管的能力，但對於未使用固定埠的網路服務（如某些 P2P 軟體會使用動態埠【即每次使用會使用不同埠】）卻無能為力。所以欲控管如 P2P 等軟體，我們需要利用解析應用層的資訊，來辨識 P2P 的服務。在新增 IPP2P 模組之前，我們必需先修正 (patch) 核心，讓核心具有應用層解析的能力，這需要 L7－FILTER 的幫忙。先讓 netfilter 具有分析應用層封包的能力，後再使用 IPP2P 的模組，藉由分析應用層封包的特徵碼來辨識 P2P 軟體並加以控管。Netfilter分為核心模組與應用程式，以下先簡單說明 NETFILTER 機制： 

####核心模組 (MODULE) 

Netfilter 利用掛鉤 (HOOK) 的方式，將相關的核心模組掛鉤 (HOOK) 於核心處理封包的流程上，針對通過的封包實施檢查及過濾。系統架構如下圖所示： 
![](http://www.openfoundry.org/images/100713/image001.jpg)  
1. PREROUTING：當封包進入時即會經過 PREROUTING 檢查點，這也是封包進入後所遇到的第一個檢查點。接下來，封包會判斷是否已到達目的主機，如果確定則會進入 INPUT 檢查點，否則即進入 FORWARD 檢查點；   
2. INPUT：當封包發現己到達目標主機時即會進入本機並經過 INPUT 檢查點，否則將進入 FORWARD 檢查點，繼續往下個目標主機前進；   
3. OUTPUT：當封包經由本機發出即會經過 OUTPUT 檢查點；   
4. FORWARD：如封包發現並非到達目標主機，即會經過 FORWARD 檢查點；   
5. POSTROUTING：當封包要離開系統主機時會經過 POSTROUTING 檢查點，這也是封包進入後所遇到的最後一個檢查點。 

####應用程式 
Netfilter 提供 iptables 應用程式，用來設定 Netfilter 檢查點的過濾規則。設定規則是以表格 (table) 及鏈 (chain) 的概念來設定，Netfilter 總共提供了三個表格及五個規則鏈。 
* Filter：主要提供封包過濾，可過濾 TCP、UDP、MAC、ICMP 等類型封包，包含 INPUT、FORWARD 和 OUTPUT 等規則鏈。 
* nat：提供 SNAT 及 DNAT 等功能，可用來 IP 偽裝，讓網域內的多台電腦可共用一個公共 IP 上網。本表格包含 PREROUTING、POSTROUTING、OUTPUT 等規則鏈。   
* mangle：主要用來修改封包內容，包含 PREROUTING、POSTROUTING、FORWARD、INPUT 和 OUTPUT 等規則鏈。   
 
以下簡略說明 iptables 的語法結構，如下例：   
```Iptables [-t table] command [match][-j target/jump]```  
* [-t table]  
用來指定要設定那個表格的規則，如未指定即預設為 filter。    
* Command   
命令，通常後會接鏈 (chain) 名稱。常用命令如下：   
-A 在指定的鏈 (chain) 之後新增一個規則    
-D 在指定的鏈 (chain) 之後刪除一個規則   
-F 清除規則   
-L 顯示規則   
* [match] 比對條件。  
常用的比對條件如下：   
-d 指定套用規則的目的主機或 IP 位址   
-I 當封包進入 FORWARD、OUTPUT 或 POSTROUTING 所通過的網路介面名稱（如 eth0） （其餘相關的設定參數就請有興趣的讀者自行參閱手冊）   
* [-j] 目標（表示設定規則的目的），常用目的選項如下：    
ACCEPT 表示允許封包通過   
DROP 表示丟棄封包   
RETURN 表示直接離開目前規則，直接跳到下個規則比對   
QUEUE 表示將封包重導到本機的佇列 (QUEUE) 中，通常用來供其它應用程式處理   

如下為一設定範例： 
```iptables -A INPUT -p tcp --dport 80 -j ACCEPT```   
-A INPUT：在 INPUT 的鏈 (chain) 上新增一規則      
-p tcp --dport 80：比對條件（表示如果通訊協定為 tcp 且目的埠為 80）   
-j ACCEPT：即表示接受   
（整段的規則為如果封包為 tcp 型式且目標埠為 80 即讓它通過） 

####安裝說明 
基本上要建造一個可控管 P2P 軟體的防火牆，我們需要下列的工作： 
* 重新編譯核心，使核心能夠控管到 L7 的封包 
* 安裝 L7-FILTER 
* 安裝 IPP2P 
* 重新編譯 iptables，使 iptables 能設定 L7 的比對規則   
  
L7-FILTER 為 Netfilter 標準外掛的模組，利用特徵碼的方式來加強 Netfilter 解析 L7 應用層的封包能力來辨識出不同的應用程式。至此，讀者可能會有個疑問，既然 L7-FILTER 已可辨識 L7 應用程式，那應該可辨識出 P2P 軟體，為何還需要 IPP2P，這涉及到兩套軟體實作的方式，L7-FILTER 是採用正規表示法來比對每個 L7 應用程式的連接 (CONNECT) 前 20 個 byte 的方式，而 IPP2P 則是將常用的 P2P 軟體特徵碼直接寫入程式中，這意謂著 IPP2P 將會比 L7-FILTER 模組更能精確地辨識出 P2P 軟體並加以控制。根據 L7-FILTER 官方網址的說明，以 Kernel 2.6.28 的相容性最佳，因此本次我們將採用 2.6.28 的核心來編譯。相關步驟如下： 

1. wget [http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.28.tar.gz](http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.28.tar.gz) #取得 2.6.28 kernel 的原始碼 
2. wget [http://downloads.sourceforge.net/project/l7-filter/l7-filter%20kernel%20version/2.22/netfilter-layer7-v2.22.tar.gz?use_mirror=ncu](http://downloads.sourceforge.net/project/l7-filter/l7-filter%20kernel%20version/2.22/netfilter-layer7-v2.22.tar.gz?use_mirror=ncu) #取得 L7-FILTER 原始碼，目前最新的版本為 2.2 
3. 將 linux-2.6.28.tar.gz 解壓縮並置於 /usr/src/linux2.6.28 目錄下 
4. 將 netfilter-layer7-v2.22.tar.gz 解壓縮並將其中的 kernel-2.6.25-2.6.28-layer7-2.22.patch 複製到 /usr/src/linux2.6.28
5. cd /usr/src/linux-2.6.28 
6. patch -p1 #修正核心組態，新增 L7 MATCH 模組 
7. make oldconfig #利用系統原有的參數來設定核心組態（讀者有興趣可參閱 /boot/config-2.6.29.4-167.fc11.i686.PAE，此檔案記錄目前系統核心所使用的參數【不一定最精簡，但保證穩定】。 在設定的過程中，系統會詢問一些問題，除了 support (NETFILTER_XT_MATCH_LAYER7) [N/m/y/?] 設定為 m，其餘皆使用預設即可 
8. make #編譯核心，在編譯的過程中，筆者曾發生 getline() 的錯誤，這是因為在核心程式 (unifdef.c) 中有自定義一個 getline 函數，剛好與標準的函數庫 getline 名稱相同而造成錯誤。讀者只需在 scripts/unifdef.c 中，把 getline 名稱改成另外的名稱即可 
9. make modules #編譯核心模組 
10. make modules_install #安裝核心模組 
11. make install #重新安裝核心並重新設定開機組態（讀者需修改 /etc/grub.conf 將預設開機設為 kernel 2.6.28【將 default=1 改為 default=0】） 
12. reboot #重新開機後，即會使用新編譯的核心開機。讀者可利用 uname –r 指令來驗證是否為 2.6.28 的核心。 

IPP2P 為 netfilter 標準的外掛模組，主要目的在於控管 P2P 軟體。根據官方網站所述，支援的 P2P 軟體如下表： 

參數 P2P / 軟體 / 通訊協定 
--edk / eDonkey, eMule, Kademlia / TCP and UDP 
--kazaa / KaZaA, FastTrack / TCP and UDP 
--gnu / Gnutella / TCP and UDP 
--dc / Direct Connect / TCP only 
--bit / BitTorrent, extended BT / TCP and UDP 
--apple / AppleJuice / TCP only 
--winmx / WinMX / TCP only 
--soul / SoulSeek / TCP only 
--ares / Ares, AresLite / TCP only 

安裝步驟如下： 
1. wget [http://netfilter.org/projects/iptables/files/iptables-1.4.3.tar.bz2](http://netfilter.org/projects/iptables/files/iptables-1.4.3.tar.bz2) 
2. 解開並將 iptables 的原始碼置於 /home/johnwu/iptables-1.4.3 
3. cd /home/johnwu/iptables-1.4.3 
4. 先針對 iptables 做 configure。./configure --prefix=/usr/local/iptables143 --with-ksource=/usr/src/linux-2.6.28/ 
5. wget [http://www.ipp2p.org/downloads/ipp2p-0.8.2.tar.gz](http://www.ipp2p.org/downloads/ipp2p-0.8.2.tar.gz) 
6. 解開 ipp2p 壓縮檔並進入該目錄中 
7. 下載相關的 patch 檔 
    * wget [http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-iptables-1.4.0.patch](http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-iptables-1.4.0.patch) 
    * wget [http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-iptables-1.4.1.patch](http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-iptables-1.4.1.patch) 
    * wget [http://www.caronico.com/linux/ipp2p-0.8.2-iptables-1.4.3.patch](http://www.caronico.com/linux/ipp2p-0.8.2-iptables-1.4.3.patch) 
    * wget [http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-kernel-2.6.22.patch](http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-kernel-2.6.22.patch) 
    * wget [http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-kernel-2.6.28.patch](http://aur.archlinux.org/packages/ipp2p/ipp2p/ipp2p-0.8.2-kernel-2.6.28.patch) 
8. 修正 patch 檔，請依次執行下列指令： 
    * patch -p1 
    * patch -p1 
    * patch -p1 
    * patch -p1 
    * patch -p1
9. 修正 ipp2p 所附的 Makefile 
    * 將 ld -shared -o libipt_ipp2p.so libipt_ipp2p.o 改為 $(CC) -shared -o libipt_ipp2p.so libipt_ipp2p.o（不然編譯出來的執行檔會有問題） 
    * 在 ifndef $(IPTABLES_SRC) 的區段中，將 IPTVER 改為 1.4.3 
    如下示： 
```
 ifndef $(IPTABLES_SRC)
    IPTVER =1.4.3
    IPTABLES_SRC = $(wildcard /home/johnwu/iptables-$(IPTVER))
 endif
```
其中 /home/johnwu/iptables-$(IPTVER) 為 iptables 原始碼所在的目錄 
1. make #編譯成功後會產生ipt_ipp2p.ko及libipt_ipp2p.so 
2. cp libipt_ipp2p.so /usr/local/iptables143/libexec/xtables/ 
3. cd iptables-1.4.1 && make && make install #編譯 iptables 

###測試 
筆者的測試環境為 IPP2P 主機當成 NAT 主機，其餘客戶端主機均透過此主機連到外面去，在此即不多說 NAT 原理，有興趣的讀者可自行參閱相關文件。請依序下達下列指令：

>1. echo "1">/proc/sys/net/ipv4/ip_forward #開啟主機 ip forward功能

>2. /usr/local/iptables143/sbin/iptables -t nat -A POSTROUTING -o XXX.XXX.XXX.XXX -s 192.168.2.0/24 -j MASQUERADE #利用 MASQUERADE 功能設定 NAT 功能。在本例中，對外實際 IP 為 XXX.XXX.XXX.XXX，而 192.168.2.0 網域的內部主機均能以此真實 IP 上網。請讀者自行依據本身環境自行調整。在設定完成後，讀者可將內部主機的閘道器 (gateway) 指向此台主機，如果可正常上網即表示設定正確。

>3. /usr/local/iptables143/sbin/iptables -A FORWARD -m ipp2p --ipp2p -j DROP #設定阻檔全部支援的 P2P 軟體，或者讀者可視個別情況來決定欲阻擋的 P2P 軟體，如：iptables -A FORWARD -m ipp2p --bit　-j DROP（設定阻檔個別的p2p軟體，如本例即為阻檔 BitTorrent）

在讀者設定完成後，可利用 iptables –L 查看是否有下列的資訊，如下圖示： 

![](http://www.openfoundry.org/images/100713/image003.png)  
至此，讀者可將本機的預設閘道器 (gateway) 指向 ipp2p 所在的主機，後即可開啟 BT 軟體來測試是否可阻檔 P2P 軟體，限於時間及設備因素，筆者僅測試 BitTorrent 此款軟體。其餘的 bittorrent 軟體就請讀者自行測試了。