# 讓你成為 flickr 上最受歡迎的攝影師－用 Gimp 製作 HDR （高動態範圍）照片

作者：Mischa Hsu，2008 年 9 月投稿。

你是否曾經希望照片的動態範圍 [1] 能夠更大，讓極亮處與最暗部的細節都能夠清楚的顯示在同一張照片裡？在傳統相機上，我們可以利用搖黑卡 [2] 遮蓋亮處的方式控制進光量，讓亮處與暗部的進光量都在底片的容忍範圍。不過進入數位時代後，我們再也不用這麼麻煩了，只要在同一場景拍攝不同曝光的照片，再將這些照片合成一張，就可以得到擁有極高動態範圍的 HDR [3] 照片，這正是數位暗房方便而有趣之處。看到這兒，那些對自己的照片有處女情節的人，就知道該轉檯了，請前往各大攝影和 3C 網站繼續筆戰數位照片的後製問題，滿足我那愛看人吵架的卑劣性格 [4]。

一般來說，如果並不非常要求成果品質，我們可以利用 Photo$hop 簡單地達成這個工作，Photo$hop 內有一個選單可以直接讓你選擇照片組轉出 HDR 照片。不過我們是支持開放源碼的人士，世界上所有的東西都得開放源碼！所以我們今天連 Nikon 官方處理 RAW 檔 [5] 的 Commercial ware/View NX 都不用，利用 UFRaw 和 Gimp 以完全的開放源碼流程來達成這個工作。

### ◎ 製作影像來源

要製作 HDR 照片之前，我們得先有一系列曝光不同的照片，讓我們可以將亮處曝光正常的照片和保留暗部細節的照片合成為一張具有高動態範圍的照片。你可以費力地架起三角架，小心翼翼地用不同的曝光設定拍攝同一場景的照片，並暗暗祈禱不要動到相機讓角度跑掉，或是突然有人衝進拍攝場景。不過現在可是數位時代，幹嘛這麼費力呢？我們還是拍張 RAW 檔，然後轉出曝光不同的 JPEG 檔好了。當然有人認為直接拍攝多張 JPEG 的合成效果更佳，不過我們是粗人，粗略的做就好了，這種費工的事，還是留給龜毛的人做吧。

當然轉檔這種事，像我們這麼 Open 的人，當然要用開放源碼的軟體（當然也有另一種可能，就是官方根本不鳥堅持用 Unix-like 系統的使用者，根本沒軟體可用，所以只能用第三方軟體。），這時 UFRaw 就該上場了。UFRaw 是個開放源碼的 RAW 檔處理軟體，它是個 Standalone 軟體，但同時也可以當成 Gimp 的外掛使用，讓 Gimp 擁有處理 RAW 檔的能力。

Software Profile

* 軟體名稱：Unidentified Flying Raw (UFRaw)

* 官方網站：[http://ufraw.sourceforge.net/](OEBPS/Text/%20http%3a/ufraw.sourceforge.net)

* 支援平台：Linux、BSD、Mac OS X、Windows

* 大小：1.38 MB

首先，我們先用 UFRaw 開啟一張 RAW 檔，圖中使用的是 Nikon D80 的 NEF 檔（RAW 的一種）。

[![](http://www.openfoundry.org/images/080928/1.png)](http://www.openfoundry.org/images/080928/1.png)

然後調整右方上的 EV 值 [6]，一般來說，RAW 的寬容度在正負二格左右，你可以依自己的需求調整，圖中是減 EV 值的效果。

[![](http://www.openfoundry.org/images/080928/2.png)](http://www.openfoundry.org/images/080928/2.png)

決定要使用的 EV 值後，將圖片儲存成 JPEG 檔。

[![](http://www.openfoundry.org/images/080928/3.png)](http://www.openfoundry.org/images/080928/3.png)

依照上述方式，輸出加 EV 值的圖片，在以下的範例中，我們所使用的照片分別是加一格和減一格 EV。

### ◎ 使用 Gimp 合併你的 HDR 照片

Software Profile

* 軟體名稱：Gimp

* 官方網站：[http://www.gimp.org/](OEBPS/Text/%20http%3a/www.gimp.org)

* 支援平台：Linux、BSD、Mac OS X、Windows

* 大小：14.1 MB

接下來我們就要將這三張照片合成一張高動態範圍的照片，我們先用 Gimp 開啟原圖，我們可以看到原圖天空有些過曝，而暗處又曝光不足。開啟原圖後，接著利用【檔案】->【Open as Layers...】將亮處曝光正常的照片（在這裡是 -1EV 的照片）載入新的圖層，並轉移到這個圖層上。

[![](http://www.openfoundry.org/images/080928/6.png)](http://www.openfoundry.org/images/080928/6.png)

### ◎ 調整亮部曝光

由於我們只要使用這張照片的亮處，所以我們得先把不用的地方遮蓋掉。我們可以使用遮罩來達成這個功能。遮罩一般來說會讓上色的地方變為透明，讓基底的影像可以顯示出來。而未上色的地方，則使用遮罩所在圖層的影像。

由於遮罩只認上色深淺，因此我們可以使用去彩度 [7] 的方式製作遮罩的選取範圍。只要使用【色彩】->【Desaturate...】就可以將這個圖層轉為灰階。

[![](http://www.openfoundry.org/images/080928/7.png)](http://www.openfoundry.org/images/080928/7.png)

按下【Desaturate...】後，Gimp 會問你變成灰階的依據，只要選擇「lightness」後，按一下〔Desaturate〕按鈕即可。

![](http://www.openfoundry.org/images/080928/8.png)

然後我們將轉成灰階的圖全選後，按一下【編輯】->【複製】將灰階圖複製起來，這是我們等一下加入遮罩的基礎。

[![](http://www.openfoundry.org/images/080928/10.png)](http://www.openfoundry.org/images/080928/10.png)

接著記得將這張圖回復到去彩度之前。

[![](http://www.openfoundry.org/images/080928/11.png)](http://www.openfoundry.org/images/080928/11.png)

然後在亮部圖層上，按一下滑鼠右鍵，並選擇【新增圖層遮罩...】。

[![](http://www.openfoundry.org/images/080928/12.png)](http://www.openfoundry.org/images/080928/12.png)

接著在「新增圖層遮罩」視窗中，選擇「白色 [完全不透明]」後，按一下〔新增〕按鈕，這樣就可以在這個圖層加上一個遮罩，你在這個圖層上的改變都會作用在遮罩上。

![](http://www.openfoundry.org/images/080928/13.png)

接著利用【編輯】->【貼上】將我們剛剛複製的灰階圖貼入新圖層。

[![](http://www.openfoundry.org/images/080928/14.png)](http://www.openfoundry.org/images/080928/14.png)

接著按一下底部的錨型按鈕，將貼入的灰階圖層，固定下一圖層的遮罩上。

[![](http://www.openfoundry.org/images/080928/15.png)](http://www.openfoundry.org/images/080928/15.png)

接著可以看到新加入的圖層作用在原圖上，使亮處的天空曝光正常，而底下草地的部分則使用原圖的影像。

[![](http://www.openfoundry.org/images/080928/16.png)](http://www.openfoundry.org/images/080928/16.png)

### ◎ 調整暗部曝光

基本上，這個程序和調整亮部曝光幾乎完全相同，只有一些部分要注意。首先，從【檔案】->【Open as Layers...】將要當成暗部的圖片（這裡是 +1EV 的圖片）載入新的圖層中。

[![](http://www.openfoundry.org/images/080928/17.png)](http://www.openfoundry.org/images/080928/17.png)

然後以相同的方式去彩度。

[![](http://www.openfoundry.org/images/080928/18.png)](http://www.openfoundry.org/images/080928/18.png)

不過要注意的是，黑色的地方會變成透明而不使用，而現在我們需要的是暗部的草地部分被遮蓋掉了，所以我們必須使用【色彩】->【反相】，將亮暗對調，使天空能被遮蓋，而草地部分能夠顯示在基底的影像上。

[![](http://www.openfoundry.org/images/080928/19.png)](http://www.openfoundry.org/images/080928/19.png)

複製已把亮暗對調的灰階圖。

[![](http://www.openfoundry.org/images/080928/20.png)](http://www.openfoundry.org/images/080928/20.png)

然後回復暗部圖層的彩色。

[![](http://www.openfoundry.org/images/080928/21.png)](http://www.openfoundry.org/images/080928/21.png)

接下來的步驟和加入亮部遮罩的過程相同，將灰階圖作為暗部的遮罩，然後你就可以看到暗處亦正常曝光了。。

[![](http://www.openfoundry.org/images/080928/22.png)](http://www.openfoundry.org/images/080928/22.png)

接著選擇【影像】->【影像平面化】將所有圖層合在一起，你就得到一張高動態範圍的照片了。

[![](http://www.openfoundry.org/images/080928/23.png)](http://www.openfoundry.org/images/080928/23.png)

以下是原圖和調整過的 HDR 圖比較

![](http://www.openfoundry.org/images/080928/ori.jpg)

![](http://www.openfoundry.org/images/080928/hdr.jpg)

[1] 動態範圍（Dynamic Range）是指一張影像最亮至最暗處能夠清晰顯示場景細節的範圍。

[2] 搖黑卡是傳統底片攝影為取得高動態範圍常用的一種技巧，主要是使用一張色卡遮蓋鏡頭進光量較高的部分，使得成影時，亮處不會過度曝光。

[3] HDR（High Dynamic Range）高動態範圍是指使影像能保留較一般數位照片更多明暗細結的技術，一開始是希望影像所呈現的效果能更接近人眼所見的場景。

[4] 關於數位影像是否可以高度後製之爭，請洽任何攝影網站，此為月經戰文之一，相信非常容易找到。

[5] RAW 檔在這裡是指從數位相機的感光原件上所得到經過最少處理的圖像，適合用來後製，因為它有著比 JPEG 保留更多細節的能力，而且受到機身內建影像處理的影響最小。每一家數位像機廠商的 RAW 檔格式都不盡相同，例如 Nikon 的 RAW 檔就稱為 NEF 檔。一般來說 RAW 檔無法直接作輸出使用，要輸出或顯示在網頁時，必須先轉成 JPEG 或 TIFF檔。而要處理 RAW 就必須安裝解碼器或使用官方軟體，不過也有第三方軟體，例如：UFRaw，提供這個功能。

[6] EV 就是 Exposure Value，指「曝光值」，也就是影像成像時受光的多寡。

[7] Desaturate，也就是將圖變成灰階影像。

note:  
本文中所使用的照片攝影者為廖祐宏先生，原圖為拍攝者所有，僅授權本文教學之用，禁止移作他用。