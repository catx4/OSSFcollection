# 如何將網頁上的小圖片放大成可印刷的圖片

作者：線人，2010 年 5 月投稿，2010 年 7 月 更新。


筆者曾經於某一間雜誌社工作，不時會遇到要替雜誌內文找插圖，而要將從網頁上取得的圖片當成內文插圖的情況。可是，透過網頁上取得的圖片，一般 來說圖片解析度都很低，若勉強將圖片印刷出來，效果簡直慘不忍睹！即使大家並非從事傳媒，有時亦有可能遇上要舉辦活動時，需要製作較大型的海報及橫幅，因 此有可能要將某些網頁圖片放大成較高解析度的圖片。

若我們真的找不到網頁上小圖片的原相或解析度較高的版本，便有需要利用某些軟體將這些小圖片放大成可印刷的解析度了。透過這種方法得到解析度較高的 圖片雖然並非完美，不過若用於印刷上，效果至少比直接拿網頁上的小圖片來用好得多。本文要介紹的 SmillaEnlarger 就是能夠將小圖片放大的自由軟體，本身亦提供適用於 Windows 的壓縮檔、適用於 Mac OS X 平台的安裝檔，以及可編譯成 Linux 軟體的原始碼。

*   軟體名稱：SmillaEnlarger
*   最新版本：0.9.0
*   軟體授權：GNU General Public License (GPLv3)
*   系統支援：Windows、Mac OS X、GNU/Linux
*   官方網站：[http://sourceforge.net/projects/imageenlarger/](http://sourceforge.net/projects/imageenlarger/)

###**將小圖片「變大」**

對使用者來說，SmillaEnlarger 最大的優點就是提供一個頗容易明白的操作介面，使用者毋須花太多時間，便能明白如何利用這個軟體將小圖片「變大」。以下筆者以 Windows 版本的 SmillaEnlarger 作示範。（從軟體的官方網站下載 Windows 版本的壓縮檔後，將內裡的「SmillaEnlarger」資料夾複製至 PC 硬碟的任何自選位置，然後再執行資料夾裡的 SmillaEnlarger.exe，便可啟動這個軟體。）

從 SmillaEnlarger 的操作介面，使用者會見到右邊有一個「圖片放大區」。

[![](http://www.openfoundry.org/images/100525/smillaenlarger01.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger01.jpg)

使用者可利用檔案總管，將任何需要放大的圖片，用鼠標將之拖曳至「圖片放大區」，就可以將這圖片載入 SmillaEnlarger。如無意外，「圖片放大區」會顯示剛載入的圖片，此外操作介面左邊的「縮圖區 (Thumbnail Preview)」亦會顯示該圖片的縮圖。

[![](http://www.openfoundry.org/images/100525/smillaenlarger02.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger02.jpg)

接下來，使用者可以在操作介面左上方的「輸出尺寸 (Output Dimensions)」中選擇將這圖片放大。「輸出尺寸」提供了多種將圖片放大的準則，可供使用者選擇。當中最常用的包括「指定縮放比率 (Specify zoom factor) 」（將圖片放大多少 %），以及「指定輸出圖片的寬度 (Specify width of result) 」。

[![](http://www.openfoundry.org/images/100525/smillaenlarger03.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger03.jpg)

為了將放大後的輸出圖片看起來更自然，SmillaEnlarger 更提供了多種「放大參數」。在操作介面上方的「放大參數 (Enlarger Parameter) 」選單，使用者可選擇【sharp】、【painted】及【sharp & noisy】三種效果。

[![](http://www.openfoundry.org/images/100525/smillaenlarger04.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger04.jpg)

選取想要的效果後，使用者按一下〔Preview!〕，便可從右邊的「圖片放大區」，觀看所選取的「放大參數」所帶來的實際效果。

[![](http://www.openfoundry.org/images/100525/smillaenlarger05.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger05.jpg)

使用者對「圖片放大區」所顯示的放大效果感到滿意後，便可以按一下〔Enlarge & Save〕，SmillaEnlarger 便會正式將圖片「放大」，並將輸出圖片自動以新的檔名儲存。當然，使用者亦可在操作介面左方的「將結果存入(Write Result To) 」，指定輸出圖片的檔名。

[![](http://www.openfoundry.org/images/100525/smillaenlarger06.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger06.jpg)

###**將相片裁切部分「變大」**

除了將網頁上圖片完整地「放大」，SmillaEnlarger 更提供將相片中的一部分「放大」功能。使用者可以先用上述的方法，將一張相片載入於 SmillaEnlarger。  

[![](http://www.openfoundry.org/images/100525/smillaenlarger07.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger07.jpg)

然後，使用者只需用滑鼠，在右邊「圖片放大區」中要放大的部分的左上角，按著滑鼠左鍵不放，「圖片放大區」的鼠標位置便會出現一個粗框。使用者再將鼠標移至要放大部分的右下角，然後放開滑鼠左鍵，便完成選取放大區域的步驟。

[![](http://www.openfoundry.org/images/100525/smillaenlarger08.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger08.jpg)

有需要時，使用者更可在操作介面下方的「裁切格式 (Cropping Format) 」，選擇要裁切的部分的形狀，例如正方形。

[![](http://www.openfoundry.org/images/100525/smillaenlarger09.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger09.jpg)

以下筆者選擇了「裁切格式」為「3:4」，這是所選部分的長方形比例。若「裁切格式」為「4:3」，所選部分的大小一樣，不過會變成橫向長方形。

[![](http://www.openfoundry.org/images/100525/smillaenlarger10.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger10.jpg)

然後，使用者便可根據以上的方法，選擇「放大參數」，再按一下〔Enlarge & Save〕，便可將所選的部分輸出為新圖片。

###**進階設定**

剛才提到使用者可透過操作介面左方的「放大參數」，快速選擇將圖片做放大的效果。若使用者對放大效果的要求更高，可按一下右方的「參數 (Parameter) 」標籤，那裡提供更多可供使用者調整的參數，包括「銳度 (Sharpness)」、「平滑度 (Flatness) 」、「減噪 (DeNoise) 」等。

[![](http://www.openfoundry.org/images/100525/smillaenlarger11.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger11.jpg)

SmillaEnlarger 更支援超過一個圖片，甚至整個資料夾的多個圖片自動放大的功能。使用者先透過操作介面選取適當的放大設定，然後將超過一個圖片或一整個資料夾拖曳至「圖片 放大區」，SmillaEnlarger 隨即自動根據使用者所選的設定，將所選的圖片自動放大。按一下右方的「Jobs」 標籤，便可查閱 SmillaEnlarger 放大工作的進度。

[![](http://www.openfoundry.org/images/100525/smillaenlarger12.jpg)](http://www.openfoundry.org/images/100525/smillaenlarger12.jpg)

以上示範所用的圖片來源，請參考下列連結：

*   [2009 c Rainbowcatz](http://www.flickr.com/photos/rainbowcatz/3371503199/)，以「姓名標示-非商業性-相同方式分享 2.0 通用版」授權散布。
*   [2008 c Lainmoon](http://www.flickr.com/photos/lainmoon/2866968259/)，以「姓名標示-非商業性-相同方式分享 2.0 通用版」授權散布。

####**◎ 作者簡介**

線人，求學時期曾經接觸過 Apple II 、 PC 等電腦系統，如今從事與智慧型手機有關的工作，每天的生活都是在不同作業系統（Windows、Mac OS、Linux，還有不同智慧型手機系統）之間遊走。