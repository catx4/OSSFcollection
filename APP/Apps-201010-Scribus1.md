# 用自由軟體 Scribus 來輸出文件（1）-基本操作

作者：陳瑞霖、李婉婷，2010 年 10 月投稿。



Scribus 是一套開源的桌上出版軟體（Desktop Publish Software）。從 2001 年開始開發，支援 CMYK、分隔線、ICC 色彩管理等專業出版功能，並且也能將檔案輸出成 PDF 格式。可與排版軟體的老大哥 Adobe InDesign、QuarkXPress 媲美。
 在這裡要特別說明的是，桌上出版軟體與文書處理軟體的定位有很大的不同。桌上出版軟體著重在版面的排列，你可以很自由的決定文字、圖片放在頁面的何處。而文書處理軟體的強項則是在於文字排序、索引方面的處理。

[![（來源：Wiki Commons）](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic01.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic01.png)  
（來源：[Wiki Commons](http://commons.wikimedia.org/wiki/File:Scribus_logo.svg)）

* 軟體名稱：Scribus
* 最新版本：1.3.3.14
* 軟體授權：GNU General Public License Version 2　(GPL2)　or any later version
* 系統支援：Debian and Ubuntu；Windows 2000/XP/Vista/7；Mac OS X；OS/2 and eComStation；OpenSUSE and SUSE Linux Enterprise；Red Hat/Fedora；CentOS；Mandriva






官方網站：[http://www.scribus.net/](http://www.scribus.net/)  
 下載網址：[http://sourceforge.net/projects/scribus/files/](http://sourceforge.net/projects/scribus/files/)

**以下操作使用 Ubuntu 10.04 版為範例，其他作業系統下的操作大致相同。**

本文將介紹 Scribus 的基本功能，如何建立新文件，插入文字、匯入圖片，以及加上簡單的美工設計。
 Scribus 版本則為 Ubuntu Software Center 下載的 Scribus Stable，版本號為 1.3.3.13。

### 建立新文件

以下範例要建立兩頁A4 大小的宣傳文件。

![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic02.png "Original Resolution - 502x312px / Original Resolution - 502x312px")  
▲ 圖 1

1\. 選擇頁面大小，在這邊我們選擇尺寸為 A4。
 2\. 頁數選擇 2。
 3\. 選擇左頁還是右頁是第一頁，這邊我們選擇左頁。

如果發現頁面太小，可以在功能表【頁面】->【管理頁面屬性】，〔頁面尺寸〕中做更改。假如要插入新的頁面，也是在功能表中【頁面】，選擇【插入】。

### 建立文字框

1\. 選擇〔新增文字框〕。
 2\. 用滑鼠拉出你要的文字框大小。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic03.png "Original Resolution - 425x356px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic03.png)   
▲ 圖 2  

接下來要使用故事編輯器，故事編輯器是 Scribus 內建的文字編輯器。在桌上出版的領域，文字通常使用文書處理器處理過後，才匯入桌上出版軟體做後續的編排。但假如只是簡單的文字編輯，其實內建的故事編輯器就能勝任。

1\. 點選工具列上的〔故事編輯器〕。
 2\. 打開故事編輯器後，先別急著輸入中文。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic04.png "Original Resolution - 525x379px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic04.png)
▲ 圖 3

1\. 對著左邊的欄位按滑鼠，選擇【編輯樣式】。
 2\. 選擇〔新建〕。

[![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic05.png "Original Resolution - 450x321px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic05.png)
▲ 圖 4

**由於 Scribus 處理中文能力不大好，很容易一不小心就當掉。建議不要直接在 Scribus 中打中文字. 建議先在其他的文書處理器如 gedit 上打上中文，再貼到 Scribus的〔故事編輯器〕裡。

選擇中文字型，請依據自己電腦系統中安裝的字型選擇，範例中使用 WenQuanYi Micro Hei（文泉驛微米黑）。要顯示中文字一定要選中文字型，才會正常顯示。

[![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic06.png "Original Resolution - 450x416px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic06.png)  
▲ 圖 5

選擇字體的顏色。

[![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic07.png "Original Resolution - 450x413px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic07.png)  
▲ 圖 6

1\. 選擇陰影效果。
 2\. 選擇文字排列效果，我們這裡選擇[置中]。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic08.png "Original Resolution - 450x416px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic08.png)  
▲ 圖 7

按下儲存後即完成樣式的新增。

[![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic09.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic09.png)  
▲ 圖 8

1\. 選擇各段落要套用的樣式。
 2\. 按下〔更新文字框並退出〕按鈕，即會儲存變更並退出故事編輯器。

[![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic10.png "Original Resolution - 450x323px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic10.png)

▲ 圖 9

### 從外部文件匯入文字到文字框中

由於 Scribus 是排版軟體，因此我們在這裡匯入已經寫好的稿件，在 Scribus 裡做後續的處理。

在先前的文字框下面新增一個文字框，按下右鍵，【匯入文字】

**在功能表中也能叫出匯入文字的功能，選擇【檔案】->【匯入】->【匯入文字】。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic11.png "Original Resolution - 350x408px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic11.png)
▲ 圖 10  

選擇要匯入的檔案。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic12.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic12.png)   
▲ 圖 11

由於 Scribus 預設的字型不是中文，因此匯入中文會無法顯示。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic13.png "Original Resolution - 450x338px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic13.png)   
▲ 圖 12

依照前面的操作步驟新增樣式，文中新增 Chinese 樣式並套用匯入的文字。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic14.png "Original Resolution - 450x323px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic14.png)    
▲ 圖 13

退出故事編輯器後，顯示一切正常了。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic15.png "Original Resolution - 450x328px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic15.png)    
▲ 圖 14

### 新增形狀

一份出版品，不只有文字，還要有圖片以及其他裝飾，才稱得上完整。接下來我們要介紹在文字框下加入色塊。

選擇〔插入形狀〕。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic16.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic16.png)    
▲ 圖 15

插入後是一團黑的長方形，文字都看不到了，別擔心，按滑鼠右鍵【屬性】。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic17.png "Original Resolution - 450x325px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic17.png)   
▲ 圖 16

挑選形狀的顏色, 以及所挑選顏色的陰影及透明度，在這裡我們陰影及透明度都設 20% 。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic18.png "Original Resolution - 277x596px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic18.png)   
▲ 圖 17

挑選自己想要的顏色，調整陰影及透明度後，我們可以看到文字了。
 對著我們剛編輯的形狀，按右鍵選擇【圖層】->【降低】。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic19.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic19.png)   
▲ 圖 18

這樣一來形狀就在下面，能點到文字框了。如果要對文字框內的文字做變動，就很方便了。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic20.png "Original Resolution - 450x325px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic20.png)   
▲ 圖 19

完成了簡單的文字編排以及加上形狀！接下來要加些圖案來修飾充滿文字的版面了。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic21.png "Original Resolution - 450x313px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic21.png)   
▲ 圖 20

### 匯入圖片

文字稿排好後，美編工作當然也少不了。點選[插入]按下【文字框】，接著拖曳出一個框框，按滑鼠右鍵選擇【匯入影像】。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic22.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic22.png)   
▲ 圖 21

選取你要匯入的圖片。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic23.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic23.png)   
▲ 圖 22

插入之後會發現一個問題，剛剛所畫的框可能太大或太小，這時可選擇[物件]，點選【調整框體適應圖片大小】，這樣就可以完整呈現你要的圖片了。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic24.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic24.png)   
▲ 圖 23

一樣按滑鼠右鍵，選擇【圖像效果】，會跳出視窗。這從裡你可以對剛匯入的圖片做出效果，像是將圖片做反轉，原本黑色的字就可以反轉成白色的了。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic25.png)
](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic25.png)▲ 圖 24

### 圖形編輯

排版時會用到許多色塊，讓版面不再單調，首先先畫出一個長方形。只要選取剛畫的圖形後，點選[視窗]找到【屬性】後按下去，會跳出一個視窗，很多有關於圖形的顏色、位置、形狀就可以在這邊做變化。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic26.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic26.png)   
▲ 圖 25

在「屬性」這個視窗裏，點選【形狀】->【編輯形狀】，所有有關於外框形狀的調整都可以在這裡達成，可以盡情發會創意，可以像這樣按下這個按鈕，把圖形壓扁。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic27.png)](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic27.png)  
▲ 圖 26

在顏色部份還可以做漸層，如下圖，在【普通】底下點選其中一個。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic28.png "Original Resolution - 259x559px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic28.png)   
▲ 圖 27

點選之後會出現右邊的長條形狀，可以看到長條底下有幾個小三角形，每個小三角形代表著不同的漸層的顏色位置，選取後可以改變那個位置的顏色，將小三角形左右移動可以調整漸層的寬度做出不同漸層變化。還可以自己增加小三角形，做出多種顏色的漸層效果。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic29.png "Original Resolution - 450x339px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic29.png)   
▲ 圖 28

將先前匯入的圖片跟剛做好的圖形疊在一起就完成了。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic30.png "Original Resolution - 450x325px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic30.png)   
▲ 圖 29

要製作出對稱圖形只需要選擇【屬性】->【X，Y，Z】，有個按鈕雙箭頭的按鈕，就可以調整上下翻轉或是水平翻轉的效果。
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic31.png "Original Resolution - 261x538px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic31.png)   
▲ 圖 30

只要善用圖形跟文字自己就可以做出不錯的文件排版了！
 [![alt](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic32.png "Original Resolution - 450x325px")](http://www.openfoundry.org/images/101012/Scribusbasic/scribusbasic32.png)  
▲ 圖 31

**在使用的時候請切記一定要隨時存檔喔！不然辛苦做好的心血很容易就會泡湯，因為通常排版軟體佔太多的系統資源，常常會有軟體當掉的風險，因此養成隨時儲存的好習慣是不可少的。

### 您也許有興趣閱讀以下文章:

*   [用自由軟體 Scribus 來輸出文件（5）- 文字應用](Apps-201102-Scribus5.md) 
*   [用自由軟體 Scribus 來輸出文件（4）- 製作模板並轉為 PDF 輸出 - 2011-01-05](Apps-201101-Scribus4.md)
*   [用自由軟體 Scribus 來輸出文件（3）-製作活動海報及手冊 - 2010-12-13](Apps-201012-Scribus3.md)
*   [用自由軟體 Scribus 來輸出文件（2）-製作大富翁棋盤 - 2010-11-04](Apps-201011-Scribus2.md)