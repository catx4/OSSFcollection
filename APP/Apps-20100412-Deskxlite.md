# 視窗自動排列 Desk x-tile
作者：陽光雨，2010 年 4 月投稿。

###** 前言**

對於經常同時使用多個應用程式的使用者而言，桌面與視窗的管理就變得相對重要。舉例而言，假設在同時間要上網、要聽音樂、要下載一個大型的檔案、要 和夥伴視訊連線、要使用文書編輯等等，亦即同時間必須同時開啟多個應用程式；此時的現象不是桌面上左一個右一個的應用程式執行畫面，就是工具面版上出現一 堆縮小的應用程式。若要同時編輯多個文件時，這種情形會變得更加的複雜。那在ubuntu底下是如何來解決這種問題呢？本篇將分批介紹工作區桌面、x- tiles 以及 gnome-shell 等三種作法提供參考。

###** 工作區桌面介紹**

對於 windows 作業系統的使用者來說，經常會忽略善用工作區桌面這個 ubuntu 內建的功能。現在就讓我們來玩一下桌面的設定吧。

###** 一般的桌面設定**

當系統全新安裝完畢，這時預設是提供二個工作區桌面給使用者使用。如果覺得不夠多，也可以自行增加桌面，如此善用不同的桌面，就可以把不同的應用程 式開在不同的桌面上，比如聽音樂使用桌面一，下傳檔案使用桌面二，桌面三是專門使用瀏覽器，桌面四專責文書編輯，如此桌面會變得清爽且容易管理應用程式。 要切換不同的桌面，除了直接用滑鼠點選左下角的桌面圖示之外，也可以使用組合鍵 Ctrl+Alt+→（向左切換）或是 Ctrl+Alt+← 向右切換桌面。相關操作畫面如下所示。

![](http://www.openfoundry.org/images/100413/desk01.png)

視窗右下角預設的二個桌面

![](http://www.openfoundry.org/images/100413/desk02.png)

在桌面圖示上按滑鼠右鍵，可以叫出突現式功能表，請點選「偏好設定」，進行桌面數量與顯示的設定。

![](http://www.openfoundry.org/images/100413/desk03.png)

這裡可以設定工作區有幾列，每列有幾個工作區，例如二列，每列四個，那就一共有八個工作區桌面可以使用。

###** 華麗的桌面Compiz**

如果覺得這種呆板的工作區切換不夠華麗，其實也可以啟動華麗的 3D 特效桌面 Compiz，由於 Compiz 的設定非常多且複雜，因此我們底下採用簡單的 Compiz 設定，讓工作區的切換更有活力。

首先先安裝簡易的設定工具，請打開終端機執行底下的指令。

`sudo apt-get install simple-ccsm`

（備註：很多人很不喜歡使用指令，其實，不用指令也可以直接使用「系統」→「管理」→「Synaptic 套件管理程式」，找 simple-ccsm 這個套件來安裝。）
 安裝好之後，各操作畫面如下。

![](http://www.openfoundry.org/images/100413/desk04.png)

首先點選「系統」→「偏好設定」→「外觀」，我們檢查一下，是不是可以順利啟動3D的桌面特效，因為有些顯示卡是無法支援的。所以請點選自訂，讓系統檢查一下並且視情況安裝專屬的限制性顯卡驅動程式。

![](http://www.openfoundry.org/images/100413/desk05.png)

如果沒有問題，順利驅動就會出現上圖的畫面，這時可點選「保留設定」，表示使用自訂的特效，這時再點選「偏好設定」。

![](http://www.openfoundry.org/images/100413/desk06.png)

這是剛才安裝的簡易設定畫面，也可以點選「系統」→「偏好設定」→「Simple CompizConfig Settings Mangger」來啟動同一個管理視窗畫面。此時請點選第3個步驟，這是要進行桌面的設定。至於其它的設定是有關視窗動畫、視窗切換的特效等等，有興趣可 以自行試試。

![](http://www.openfoundry.org/images/100413/desk07.png)

這時我們把外觀改成桌面立方體，桌面的數量改為四個。

[![](http://www.openfoundry.org/images/100413/desk08.png)](http://www.openfoundry.org/images/100413/desk08.png)

上圖是當我按下 Ctrl+Alt+↓ 這個組合鍵，會把四個桌面整個攤平讓你選擇要哪一個桌面，進行選擇也很容易，Ctrl+Alt 組合鍵繼續按著不要放開，再按下 ← 或 → 鍵就可以轉換桌面嚕。

這種效果是不是更華麗了?

###** X-TILE 介紹與使用**

利用不同的工作區桌面來解決同時啟動多應用程式的視窗混亂問題是一個不錯的解決方案，但是如果我就是必須在同一個桌面上同時開啟多個應用程式時怎麼 辦呢？比如要同時開啟數個文件檔案、試算表等等，並且在不同的文件視窗中進行資料的複製、貼上。在一個桌面上開數個應用程式，且這數個應用程式必須同時顯 示在桌面上，這時就不得不利用滑鼠，把視窗拉來拖去，改變大小等等煩人的動作，以便讓視窗同時顯示在桌面上。如果要更簡單的完成這個沒有技術性的工作，這 時 x-tile 就派上用場了。它可以讓你的應用程式，同時排列在桌面上，比如四個應用程式，只要點一下滑鼠就可以把應用程式視窗四個上、下、左、右自動排好在桌面上等你 使用。這不是非常方便作業嗎。

這個 x-tile 並沒有收錄在 ubuntu 的套件庫裡，所以必須自行上網下載安裝。

**◎ [官方網站](http://open.vitaminap.it/en/x_tile.htm)**

到達官網之後，視窗稍微向下拉一些，我們要下載 x-tile_1.3.2-1_all.deb 這個檔案。

[![](http://www.openfoundry.org/images/100413/desk09.png)](http://www.openfoundry.org/images/100413/desk09.png)

下載回來之後，點二下啟動 GDebi 視窗化安裝程式，點選安裝套件把它裝起來吧。

[![](http://www.openfoundry.org/images/100413/desk10.png)](http://www.openfoundry.org/images/100413/desk09.png)

安裝好之後，在上方的面板上按右鍵，準備把這個工具加入到面板上提供使用。

![](http://www.openfoundry.org/images/100413/desk11.png)

此時會打開加入面板視窗，找到 x-tile 把它加入到面板上。

![](http://www.openfoundry.org/images/100413/desk12.png)

這時在面板上就多了一個 xtile 的圖示。此時如果需要把視窗排列，只要對著它按滑鼠的右鍵，就可以啟動排列視窗的選項。如下圖。

![](http://www.openfoundry.org/images/100413/desk13.png)

如果對著圖示按滑鼠左鍵，會出現 x-tile 的管理視窗界面。如果視窗非常多，可以要求要哪幾個進行排列，或是把哪個最大化等等功能。由於已經全面中文化了，使用上相當方便。

[![](http://www.openfoundry.org/images/100413/desk14.png)](http://www.openfoundry.org/images/100413/desk14.png)

底下是自動排列四個視窗的結果。

[![](http://www.openfoundry.org/images/100413/desk15.png)](http://www.openfoundry.org/images/100413/desk15.png)

###** Gnome-Shell全新的桌面管理**

Gnome 已經發佈了 [2.3 版](http://library.gnome.org/misc/release-notes/2.30/index.html.zh_TW)， 預計在六個月之後，將發表最新的 gnome3.0。在這個 3.0 的版本中，依目前的規劃，除了原有功能的加強之後，其中最引人注目的就是新的桌面管理 gnome-shell，如果你現在使用的版本是 9.10 或是 10.04 beta 版，現在也可以體驗看看。首先要先安裝 gnome-shell，這個套件已經包含在套件庫裡，所以只要很簡單的指令就可以安裝起來，請輸入底下的安裝指令。

`sudo apt-get install gnome-shell`

安裝好之後，在終端機裡輸入

`gnome-shell --replace`

這時就可以啟動gnome-shell來取代原有的桌面管理。如下二圖。

[![](http://www.openfoundry.org/images/100413/desk16.png)](http://www.openfoundry.org/images/100413/desk16.png)

[![](http://www.openfoundry.org/images/100413/desk17.png)](http://www.openfoundry.org/images/100413/desk17.png)

這個gnome-shell使用上非常容易，可以把滑鼠移到左上角去點選，或是直接按下 super 按鍵（這個按鍵就是位於左邊介於 ctrl 與 alt 之間的那個按鍵）也可以啟動管理頁面。畫面右下有一個灰灰的 ┼ 符號，每點一下就可以增加一個桌面，接著可以點選應用程式或是直接把應用程式的圖示拖拉到要執行的桌面上，也可以執行該應用程式。要刪除桌面也很容易，在 每個桌面的中間有個灰灰的—符號，點一下桌面就不見了。

不過要注意，因為目前還算是體驗，穩定度還不是很好，如果發現錯誤也可以回報給官網。

####** 作者簡介**
 陽光雨
 自由是人類共有的生命價值。（[田美的地瓜網](http://www.tmes.mlc.edu.tw/wiki)及[陽光雨部落格](http://tw.myblog.yahoo.com/jw%21sXaNiUyBBBi0nyOxQNnl1QUjfA--)）