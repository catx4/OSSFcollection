# Pandoc 用命令行轉換標記語言！
作者：林雪凡，2012 年 9 月投稿。

* 軟體名稱：Pandoc
* 介紹版本：1.9.2
* 官網：[http://johnmacfarlane.net/pandoc/](http://johnmacfarlane.net/pandoc/)
* 授權：GPL-3.0

### 簡介

Pandoc 是一款命令行轉檔軟體。不過和一般人對轉檔軟體的印象不同，它所轉換的對象不是影片，而是標記語言。

Pandoc 能將 Markdown、reStructuredText、textile、HTML、DocBook、LaTeX 格式轉換為……

1.  基於 HTML 的標記語言：包括 XHTML、HTML、S5 幻燈片。
2.  文書編輯格式：包括 docs、odt。
3.  Epub 電子書。
4.  LaTeX 格式。透過 LaTeX 格式還可以進一步轉為 PDF 格式。
5.  許多輕量級標記語言，包括：Markdown、reStructuredText、AsciiDoc、Mediawiki、Textile 與 Emacs Org-Mode。

### 安裝說明

Windows 與 Mac OS X 使用者，請從下面的網頁下載安裝： [http://code.google.com/p/pandoc/downloads/list](http://code.google.com/p/pandoc/downloads/list)

至於 Linux 與 BSD 使用者，請檢查您的套件庫，從套件庫安裝即可。倘若套件庫版本過舊，官網也有提供使用源碼包安裝的方式。詳情請參看這頁： [http://johnmacfarlane.net/pandoc/installing.html
](http://johnmacfarlane.net/pandoc/installing.html)

### 使用說明

前面說過了，Pandoc 是款命令行軟體，所以請打開終端機，咱們開始下指令吧。

#### 基本指令下法

先看下面這行：

<pre>pandoc demo.rst -o demo.md</pre>

這就是最簡單的 Pandoc 用法。

`pandoc` 是 Pandoc 的主程式，Pandoc 的所有操作都是透過這個程式執行的。`demo.rst` 指定了轉檔時的輸入檔案，`-o demo.md` 則是輸出檔案。如果不用 `-o` 參數指定輸出檔名，Pandoc 就會直接將結果輸出到標準輸出上（一般說來，這是指終端機上）。

Pandoc 會透過副檔名，自動判斷輸入與輸出的檔案格式。像上面那一行，輸入檔案會被自動視為 [reStructuredText](http://docutils.sourceforge.net/docs/user/rst/quickref.html) 而輸出檔案則會被自動視為 [markdown](http://markdown.tw/)。

您也可以透過 -f 與 -t 參數強制指定輸入與輸出的格式，比方說：

<pre>pandoc demo.txt -o output.txt -f markdown -t html</pre>

以上這句指令，會將輸入檔案「demo.txt」視為 markdown 格式，並將它轉為 html 格式後，存到 output.txt 位置上。

-f、-t 後面可接哪些格式名稱？請參看這裡： [http://johnmacfarlane.net/pandoc/README.html#general-options
](http://johnmacfarlane.net/pandoc/README.html#general-options)

#### 產生 PDF

在 Pandoc 支援的格式中，想用 Pandoc 產生 pdf 檔又多了幾個小撇步，讓我試好久，請聽我說明解釋。

pandoc 的 PDF 產生功能是透過 LaTeX 來做的，所以除了主程式外，使用前還得先裝別的外掛程式。在 Linux 上，這個程式就是 texlive 包，請自行抓來裝。如果光裝 texlive 包 Pandoc 轉 pdf 時還是跑不起來，就將 texlive-cjk 包和 texlive-xetex 包也裝上，這樣就該沒問題了。

兩個包合計將近 500 MB，真是夠大的……

裝好了之後，就請立刻執行看看吧：

<pre>pandoc demo.rst -o demo.pdf</pre>

若您的 demo.rst 是純英文的，照上面操作就可以順利產生 pdf 檔案了。然而如果原始文件檔中含有其他字元（比方說中文字），執行之後 pdf 產生不出來，反而會吐出「Undefined control sequence」如何如何的錯誤訊息，這是因為預設的 latex 引擎不支援中文的緣故。

為了解決這個問題，必須改用能支援 Unicode 的 xelatex 引擎。

此外預設的字型也不包括中文，為了順利顯示，這邊還需要另外指定中文字型。總之就是下面這行（指定的字型是文泉驛微米黑，您可以替換成您希望的。請記得用英文名稱）：

<pre>pandoc demo.rst -o demo.pdf --latex-engine=xelatex -V mainfont="WenQuanYi Micro Hei"</pre>

如此一來就能順利產生出中文的 pdf 檔案了。

不過，儘管順利產生了 pdf，但某些問題依然存在（比方說中文的 line break），這些東西必須透過校調模版才能解決。接下來筆者會介紹一下 Pandoc 的模版，但關於如何用 XeTeX 設定中文 pdf 文件已經超出本文範圍，網路上有很多這方面資料，個人推荐新手參考這一頁來修改模版：[http://electronic-blue.wikidot.com/doc:xetex](http://electronic-blue.wikidot.com/doc:xetex)

#### 使用模版 (template)

模版 (template) 是用來告訴 Pandoc，它應該要用什麼格式來產生目的檔案用的。

說得更具體一點：字型如何？字要多大？頁面寬度多寬？加重語氣時要用黑體還是斜體？……這些都有賴模版進行設定。

不同的目標檔案格式，其模版的基本格式也不一樣。比方說 html 的模版就會是 CSS 與 html 的混合，latex 就是 .tex 格式……諸如此類。此外，模版中還會混有 pandoc 專屬的內部程式碼。

嗯……這樣說好像很複雜。不過只要擁有相應格式的撰寫經驗，修改預設模版其實並不難，試試看就知道了。但第一步，您要先找到這些模版被放在哪裡。

在我的 (Linux) 電腦上，預設模版被堆放在這個位置：

<pre>/usr/share/pandoc-1.9.2/templates</pre>

您也可以用以下指令叫出某格式的模版，來閱讀或另存：

`pandoc -D html | less #` 閱讀 html 格式的模版

`pandoc -D html > mytemplate.txt #` 另存到 `mytemplate.txt` 中

得到範本後，就可以依樣畫葫蘆地修改他們了。

怎麼修改等會兒再說。總之，當您將模版修改好後，就可以透過 --template 參數套用它。如同下面這行：

<pre>pandoc demo.rst -o demo.html --template=mytemplate.txt</pre>

您可以用模版功能來進行絕大多數的格式自定義，自由度相當高。至於少部份無法透過模版架構進行自訂的東西，在 Pandoc 指令後方往往也能追加一些特殊參數來處理。具體有哪些參數可用，還請參看官網的參數列表：[http://johnmacfarlane.net/pandoc/README.html#options](http://johnmacfarlane.net/pandoc/README.html#options)

#### 定義與修改模版

如果您剛剛試著看過任何一個 Pandoc 模版，這時恐怕正感到頭暈，因為模版裡面堆滿了錢……不，應該說是「$」符號。這些「$」符號被兩兩成對地運用著，像我們平常使用括號那樣夾著某些東西。

這就是 Pandoc 所使用的模版巨集語句！包括以下幾種：

1\. if 判斷→ $if（變數名）$　$else$　$endif$

*   如果變數有被指定，則 $if（變數名）$ 後的內容會被實際寫入檔案中。反之，如果變數沒被指定，則 $else$ 後面的內容會被寫入。判斷式的最後需用 $endif$ 收尾。

2\. for 迴圈→ $for（變數名）$　$endfor$

*   運用在多值變數上。會重複寫入 $for（變數名）$ 與 $endfor$ 之間的內容。變數有多少個值就會重複寫入幾次。

3\. 變數→ $變數名$

*   會被替換為指定的變數值。
*   變數名可以自己取，比方說取為 largefontsize 等等。

有這些東西就能玩出很多花樣了，比方說：

<pre>$if(largefontsize)$ font-size: $largefontsize$; $endif$</pre>

這行表示，當使用者有指定 largefontsize 時，才將 font-size: $largefontsize$; 這行寫入 CSS 中……哎，可玩的東西很多啦。

說了那麼多，那麼該如何將變數具體指定為某個值呢？

我們回來看看 pdf 篇最後的範例：

<pre>pandoc demo.rst -o demo.pdf --latex-engine=xelatex -V mainfont="WenQuanYi Micro Hei"</pre>

這行中，「-V mainfont=...」也可以寫成「--veriable mainfont=...」。更直接點說，這段指令將一個名叫 mainfont 的變數，設定為 "WenQuanYi Micro Hei" 的值……

這樣應該完全理解了吧？變數就是如此了。

您可以看看官方說明 ([http://johnmacfarlane.net/pandoc/README.html#templates](http://johnmacfarlane.net/pandoc/README.html#templates))，內容和我這邊寫的差不多；不過下面多了些常見變數的命名與介紹，那些東西我就不費事重複了，需要變數資料的人請自己查一下。

#### 瑕疵與繞過

Pandoc 轉檔很方便，但程式本身還是有些問題。至少在筆者的經驗中，1.9.2 版透過命令行輸入時，若輸入的字串中包含任何非英文字元，就會報錯。

但這並不是 Pandoc 內部不能處理非英文路徑，而似乎是 Pandoc 的「命令行介面」不能傳入非英文字元，這理由在下也搞不清楚。

如果不幸碰上，稍微更名一下就能將它給繞過了。

#### 深入閱讀

以下是推荐的閱讀清單。

1.  支援的轉檔格式表，用一張大圖表示：
     [http://johnmacfarlane.net/pandoc/diagram.png](http://johnmacfarlane.net/pandoc/diagram.png)
2.  經 Pandoc 擴充後的 Markdown 格式手冊：
     [http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown](http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown)。Pandoc 的 Markdown 格式並不「官方」，這份自行擴充後的 Markdown，顯然被 Pandoc 作者視為得意特色，語法說明書長度比起 Pandoc 其他部份的說明全部加起來都還要多（笑）。
3.  Pandoc 參數說明：
     [http://johnmacfarlane.net/pandoc/README.html#options](http://johnmacfarlane.net/pandoc/README.html#options)
4.  Pandoc 指令範例：
     [http://johnmacfarlane.net/pandoc/demos.html](http://johnmacfarlane.net/pandoc/demos.html)
