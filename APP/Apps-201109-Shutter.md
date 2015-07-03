# 擷圖與編修一次完成的開源軟體 Shutter
作者：陳筱婷，2011 年 9 月投稿。


* 軟體名稱：Shutter
* 版本：0.87.3
* 官網：[http://shutter-project.org](http://shutter-project.org)
* 系統環境：Ubuntu 10.04

Shutter 是在 Ubuntu底下運行的開源擷圖軟體，軟體介面人性化，支援區域選取、桌面擷圖、視窗擷圖三種擷圖方式。在擷取圖片後能進行編輯，可加入文字、線條、圖案方塊與標誌，並能利用套件進行更多種編修，包含旋轉、灰階、重新定義尺寸等。Shutter 是在 Ubuntu 底下運行的開源擷圖軟體，軟體介面人性化，支援區域選取、桌面擷圖、視窗擷圖三種擷圖方式。在擷取圖片後能進行編輯，可加入文字、線條、圖案方塊與標誌，並能利用套件進行更多種編修，包含旋轉、灰階、重新定義尺寸等。圖片儲存格式支援常見的 jpg、png、gif，另有 bmp、tiff、ani 等共 15 種格式。軟體預設使用 png 格式儲存，若欲用其他格式儲存，可在偏好設定內改成符合自己使用習慣的圖片格式。

### （一）安裝教學與軟體環境

<Shutter 這套軟體雖可透過 Ubuntu 內建的軟體中心安裝，但經由軟體中心所安裝的 Shutter 為舊版，建議由終端機安裝新版，其步驟如下：

1.「應用程式」→「附屬應用程式」→「終端機」，開啟終端機。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter1_1.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter1_1.png)

▲ 圖1：開啟終端機

2.輸入指令 sudo add-apt-repository ppa:shutter/ppa 並按下 Enter，接著輸入使用者密碼以確認安裝
[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter1_2.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter1_2.png)  

▲ 圖2：輸入 sudo add-apt-repository ppa:shutter/ppa 指令後準備輸入密碼

3.輸入指令 `sudo apt-get update` && `sudo apt-get install shutter` 並按下 Enter，由於剛剛已經輸入過密碼，這次系統會直接運行而不會再次詢問密碼。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter1_3.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter1_3.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">3</span>：<span style="font-family: Times New Roman,serif;">Shutter</span> 安裝中

<span style="font-family: Times New Roman,serif;">4.</span> 安裝完成後，便可在「應用程式」→「附屬應用程式」中執行 <span style="font-family: Times New Roman,serif;">Shutter</span> 了。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter1_4.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter1_4.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">4</span>：執行 S<span style="font-family: Times New Roman,serif;">hutter</span>

<span style="font-family: Times New Roman,serif;">5\. Shutter</span> 的介面相當簡單易懂，上方為功能表列與按鈕列，右下角可設定擷圖延遲秒數，右上角區則提供擷圖之後的編輯、發佈功能。

功能表列由左至右為【檔案】、【編輯】、【檢視】、【擷取】、【開始】、【求助】<span xml:lang="zh-TW">。</span>

按鈕列由左至右分別為：〔<span style="font-family: Times New Roman,serif;">Redo</span>〕（重新擷取上一次執行的擷取動作，在擷取動畫或固定大小的視窗時相當實用）、〔擷取選取區域〕（可自由選取想擷取的區域大小）、〔<span style="font-family: Times New Roman,serif;">Desktop</span>〕（擷取整個螢幕畫面）、〔擷取視窗〕（只擷取單一個視窗畫面）。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter1_5.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter1_5.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">5</span><span xml:lang="zh-TW">：S</span><span style="font-family: Times New Roman,serif;">hutter</span>的環境畫面

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter1_6.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter1_6.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">6</span><span xml:lang="zh-TW">：</span>功能表列與按鈕列

### （二）螢幕擷圖教學

#### <span style="font-family: Times New Roman,serif;">1.</span> 擷取選取區域

點下〔擷取選取區域〕右方的小三角形後，可選擇〔進階選取工具〕或〔簡易選取工具〕。若 選擇簡易選取工具，在按下〔擷取選取區域〕之後，影像就會立即被擷取、儲存；若使用進階選取工具，在按下〔擷取選取區域〕之後，還能再調整選取區域的大小 和位置到滿意為止，之後便不需再使用編修軟體剪裁。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter2_1_1.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter2_1_1.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">7</span><span xml:lang="zh-TW">：</span>選擇簡易選取工具或進階選取工具

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter2_1_2.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter2_1_2.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">8</span><span xml:lang="zh-TW">：</span>在進階模式下可調整選取範圍

#### <span style="font-family: Times New Roman,serif;">2.</span> 桌面擷圖

點選〔<span style="font-family: Times New Roman,serif;">Desktop</span>〕可直接擷取當前桌面影像。若在擷取影像前需要移動鼠標進行操作，可在右下角輸入欲延遲秒數，再點選〔<span style="font-family: Times New Roman,serif;">Desktop</span>〕。<span style="font-family: Times New Roman,serif;">Shutter</span> 會在設定秒數倒數結束時才擷取畫面，若沒有要進行複雜的操作，等待 <span style="font-family: Times New Roman,serif;">3</span> 至 <span style="font-family: Times New Roman,serif;">5</span> 秒便已足夠。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter2_2_1.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter2_2_1.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">9</span><span xml:lang="zh-TW">：</span>擷取桌面圖片

此外，<span style="font-family: Times New Roman,serif;">Shutter</span> 也提供擷取 <span style="font-family: Times New Roman,serif;">Ubuntu</span> 其它桌面的功能。按下〔<span style="font-family: Times New Roman,serif;">Desktop</span>〕右方的小三角形，可選擇擷取其它桌面畫面，或是一次合併擷取 4 個桌面的工作畫面。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter2_2_2.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter2_2_2.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">10</span><span xml:lang="zh-TW">：</span>可選擇擷取其它桌面畫面，或是一次擷取所有桌面。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter2_2_3.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter2_2_3.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">11</span><span xml:lang="zh-TW">：</span>擷取所有桌面影像

#### <span style="font-family: Times New Roman,serif;">3.</span> 擷取單一視窗

點選〔擷取視窗〕，在欲擷取畫面的視窗按下滑鼠；或點選〔擷取視窗〕右方的小三角形，選擇一個目前工作中的視窗，按下滑鼠即完成。這樣擷取下來的影像是一完整視窗而不含桌面背景，十分便利。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter2_3_1.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter2_3_1.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">12</span><span xml:lang="zh-TW">：</span>點選〔擷取視窗〕後，於欲擷取畫面的視窗按下滑鼠

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter2_3_2.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter2_3_2.png)

▲ 圖<span style="font-family: Times New Roman,serif;">13</span><span xml:lang="zh-TW">：</span>或是按下箭頭，選擇一個目前工作中的視窗

最後，若您有需要重複擷取圖片，可使用功能列最左方的〔<span style="font-family: Times New Roman,serif;">Redo</span>〕功能。按下〔<span style="font-family: Times New Roman,serif;">Redo</span>〕按鈕，<span style="font-family: Times New Roman,serif;">Shutter</span> 會自動以上次的步驟再次擷取圖片，省去重新操作的時間。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter2_4_1.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter2_4_1.png)

▲ 圖<span style="font-family: Times New Roman,serif;">14</span><span xml:lang="zh-TW">：</span>〔<span style="font-family: Times New Roman,serif;">Redo</span>〕按鈕

### （三）筆記、註記、編號

除了擷取螢幕影像外，<span style="font-family: Times New Roman,serif;">Shutter</span> 還提供了影像編輯與註記功能，特別適合撰寫圖說。點選右上方的〔<span style="font-family: Times New Roman,serif;">Edit</span>〕功能鍵，跳出另一視窗以編輯剛剛的影像。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_1.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_1.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">15</span><span xml:lang="zh-TW">：</span>點選〔<span style="font-family: Times New Roman,serif;">Edit</span>〕  

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_2.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_2.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">16</span><span xml:lang="zh-TW">：</span><span style="font-family: Times New Roman,serif;">Shutter</span> 的編輯環境

在視窗的左方有一排工具列，可以在剛剛擷取的影像上畫線、加入文字、進行螢光標注等功能。

<span style="font-family: Times New Roman,serif;">1.</span>〔選取工具〕－選取、拖曳物件時使用。

<span style="font-family: Times New Roman,serif;">2.</span>〔鉛筆工具〕－任意畫不規則線條。

<span style="font-family: Times New Roman,serif;">3.</span>〔螢光標注〕－類似鉛筆工具。筆畫顏色預設為黃色、透明度 <span style="font-family: Times New Roman,serif;">50</span>％，適合用於強調某行文字時使用。

<span style="font-family: Times New Roman,serif;">4.</span>〔直線〕－加入直線，可於下方工具列改變顏色、線條寬度。

<span style="font-family: Times New Roman,serif;">5.</span>〔箭頭〕－加入箭頭，可於下方工具列改變顏色、線條寬度。

<span style="font-family: Times New Roman,serif;">6.</span>〔矩形〕－加入矩形方塊，可於下方工具列改變填色、顏色、線條寬度。

<span style="font-family: Times New Roman,serif;">7.</span>〔橢圓〕－加入橢圓方塊，可於下方工具列改變填色、顏色、線條寬度。

<span style="font-family: Times New Roman,serif;">8.</span>〔加入文字〕－輸入文字，可於下方工具列改變字型、色彩、大小。

<span style="font-family: Times New Roman,serif;">9.</span>〔抹去〕－將欲擦去的範圍（如個人資料、帳號密碼）去除。

<span style="font-family: Times New Roman,serif;">10.</span>〔馬賽克〕－將欲擦去的範圍（如個人資料、帳號密碼）模糊化。

<span style="font-family: Times New Roman,serif;">11.</span>〔數字步驟〕－依序加入數字，適合用於軟體操作步驟說明。

<span style="font-family: Times New Roman,serif;">12.</span>〔裁剪〕－若對剛剛擷取的影像大小不滿意，可在此處再進行剪裁。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_3.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_3.png)

▲ 圖<span style="font-family: Times New Roman,serif;">17</span><span xml:lang="zh-TW">：</span>左方工具列，提供各種線條與方塊工具。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_4.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_4.png)

▲ 圖<span style="font-family: Times New Roman,serif;">18</span><span xml:lang="zh-TW">：</span>下方小工具，可調整填色、線條顏色、線條寬度與字型。

若想加入另外的影像，點選【插入圖片】，再選擇從【工作階段匯入】（<span style="font-family: Times New Roman,serif;">Shutter</span> 先前擷取的影像）或是【從檔案系統匯入】（本機資料夾）。<span style="font-family: Times New Roman,serif;">Shutter</span> 也內建了許多常見的符號，點選〔插入圖片〕右方的小三角形，即可在影像中插入許多的圖章，包括勾選 <span style="font-family: Times New Roman,serif;">(apply)</span>、錯誤 <span style="font-family: Times New Roman,serif;">(error)</span>、星星 <span style="font-family: Times New Roman,serif;">(star)</span>、警告 <span style="font-family: Times New Roman,serif;">(warning)</span> 等，若找不到符合需求的符號，在 <span style="font-family: Times New Roman,serif;">Cursors</span>（鼠標）、<span style="font-family: Times New Roman,serif;">Forms→Callouts</span>（對話視窗）、<span style="font-family: Times New Roman,serif;">Tango icon library</span>（按鈕圖庫）中還有更多的標誌可以使用。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_5.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_5.png)

▲ 圖<span style="font-family: Times New Roman,serif;">19</span><span xml:lang="zh-TW">：</span>點選按鈕以插入符號或圖片

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_6.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_6.png)

▲ 圖<span style="font-family: Times New Roman,serif;">20</span><span xml:lang="zh-TW">：</span>選擇從工作階段匯入或檔案系統匯入圖片

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_7.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_7.png)

▲ 圖<span style="font-family: Times New Roman,serif;">21</span><span xml:lang="zh-TW">：</span>選擇從內建符號庫插入符號

接著我們進行實際操作，將欲擷取的視窗畫面擷取下來，點選右上方的〔<span style="font-family: Times New Roman,serif;">Edit</span>〕，開啟編輯功能。

<span style="font-family: Times New Roman,serif;">1.</span> 使用矩形工具插入長方形，並去除填色或將透明度調至最小。

<span style="font-family: Times New Roman,serif;">2.</span> 點選文字工具，在對話框中輸入想要顯示的文字。

<span style="font-family: Times New Roman,serif;">3.</span> 選擇螢光標示工具，在欲強調的重點上畫上螢光色。

<span style="font-family: Times New Roman,serif;">4.</span> 使用箭頭工具，由左而右拉出箭頭，使重點處更顯目。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_8.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_8.png)

▲ 圖<span style="font-family: Times New Roman,serif;">22</span><span xml:lang="zh-TW">：</span>擷取下來的視窗畫面

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_9.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_9.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">23</span><span xml:lang="zh-TW">：</span>插入長方形與圖說

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_10.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_10.png)

▲ 圖<span style="font-family: Times New Roman,serif;">24</span><span xml:lang="zh-TW">：</span>加上螢光標記與箭頭

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter3_11.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter3_11.png)

▲ 圖<span style="font-family: Times New Roman,serif;">25</span><span xml:lang="zh-TW">：</span>完成圖說

### （四）進階功能與偏好設定

<span style="font-family: Times New Roman,serif;">Shutter</span> 除了在影像上加上記號、文字以外，也能透過套件進行許多編修，例如調整為灰階、泛黃效果、重新定義尺寸等。點選【擷取】→【<span style="font-family: Times New Roman,serif;">Run a plugin</span>】，開啟套件模組對話視窗，並選擇想要的效果。以灰階為例，點選「灰階」，並按下「<span style="font-family: Times New Roman,serif;">Run</span>」，剛剛擷取的視窗影像便被轉為灰階。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter4_1.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter4_1.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">26</span><span xml:lang="zh-TW">：</span>【擷取】→【<span style="font-family: Times New Roman,serif;">Run a plugin</span>】

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter4_2.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter4_2.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">27</span><span xml:lang="zh-TW">：</span>選擇想要進行的編修功能

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter4_3.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter4_3.png)

▲ 圖<span style="font-family: Times New Roman,serif;">28</span><span xml:lang="zh-TW">：</span>選擇「灰階」並按下「<span style="font-family: Times New Roman,serif;">Run</span>」

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter4_4.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter4_4.png)

▲ 圖<span style="font-family: Times New Roman,serif;">29</span><span xml:lang="zh-TW">：</span>轉為灰階的影像

點選「<span style="font-family: Times New Roman,serif;">polaroid</span>」功能，選擇想要加上的文字，旋轉的尺度，<span style="font-family: Times New Roman,serif;">Shutter</span> 會自動加上白框，製造出一張拍立得風格的相片。<span style="font-family: Times New Roman,serif;">Shutter</span> 可以開啟本機內的圖片進行編修，並不限於編輯擷圖下來的影像，若要開啟本機圖片，請點選【檔案】→【<span style="font-family: Times New Roman,serif;">Open</span>】，再選擇欲開啟的檔案。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter4_5.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter4_5.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">30</span><span xml:lang="zh-TW">：</span>「<span style="font-family: Times New Roman,serif;">polaroid</span>」（拍立得）細節設定

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter4_6.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter4_6.png)  

▲ 圖<span style="font-family: Times New Roman,serif;">31</span><span xml:lang="zh-TW">：</span>「<span style="font-family: Times New Roman,serif;">sepia</span>」（棕色濾鏡）細節設定

<span style="font-family: Times New Roman,serif;">Shutter</span> 比 <span style="font-family: Times New Roman,serif;">Ubuntu</span> 內建的螢幕擷圖功能更豐富，也更彈性化。在【偏好設定】中可調整許多設定，進入【檔案】→【偏好設定】後，即可進行個人化的調整。您可以選擇改變預設儲存的圖片格式、存檔的位置、檔案名稱、是否顯示滑鼠指標、是否在擷圖後自動顯示主視窗、在登入時啟動、最小化至提示列等。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter5_1.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter5_1.png)

▲ 圖<span style="font-family: Times New Roman,serif;">32</span><span xml:lang="zh-TW">：</span>改變影像儲存格式、路徑

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter5_2.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter5_2.png)

▲ 圖<span style="font-family: Times New Roman,serif;">33</span><span xml:lang="zh-TW">：</span>設定是否在使用時最小化、擷圖完是否跳出提示等。

【鍵盤】中提供了設置快速鍵的功能。若您需要大量擷取影像，只要將「擷圖後自動顯示主視窗」功能關閉，並啟用快速鍵功能，即可讓 <span style="font-family: Times New Roman,serif;">Shutter</span> 以背景模式運行，並進行連續擷圖，最後再於預設儲存資料夾中存取剛剛擷取的圖片，相當方便。

[<span style="font-family: Times New Roman,serif;">![](http://www.openfoundry.org/images/111004/Shutter/shutter5_3.png)</span>](http://www.openfoundry.org/images/111004/Shutter/shutter5_3.png)

▲ 圖<span style="font-family: Times New Roman,serif;">34</span><span xml:lang="zh-TW">：</span>快速鍵設定

比起 <span style="font-family: Times New Roman,serif;">Unbuntu</span> 內建的擷圖工具以及其它擷圖軟體（如 <span style="font-family: Times New Roman,serif;">Ksnapshot</span>），<span style="font-family: Times New Roman,serif;">Shutter</span> 的特色是擷圖與編修合一，擷取完影像之後可立即編輯，且內建許多圖說工具（順序步驟、箭頭、符號等），能省下許多撰寫圖說的時間。下次若想儲存桌面影像、製作簡報、傳畫面給朋友，不妨試試這套簡單易用的擷圖軟體。