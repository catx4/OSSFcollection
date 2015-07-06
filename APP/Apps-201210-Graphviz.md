# Graphviz - 用指令來畫關係圖吧！
作者：林雪凡，2012 年 10 月投稿。



* 軟體名稱：Graphviz
* 介紹版本：2.28.0
* 官網：[http://www.graphviz.org/](http://www.graphviz.org/)
* 授權：[GPL-1.0/](http://www.openfoundry.org/licenses/2062-cpl-and-epl)

### 簡介

Graphviz 是一個運用廣泛的命令行繪圖軟體，不過說是繪圖軟體，它能繪的圖並不是一般人想像中的漫畫或 logo，而是數學意義上的 "graph"，比較通俗的說法就是「關係圖」。

舉例來說，像是下面這種圖：

<div>[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_01.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_01.jpg)</div>

▲ 圖1：Unix 家族。Graphviz 官網的示範圖片之一。

用手畫會很痛苦，而 Graphviz 可以替使用者搞定它。Graphviz 提供一套語言，讓您能直接陳述圖片上的節點、邊、方向等性質。之後，由它來為您產生整張圖片。

Graphviz 能畫的圖片有許多種，您可在[官網](http://www.graphviz.org/Gallery.php)挖到更多範例。

### 安裝

Graphviz 支援 Windows、Mac OS X、FreeBSD、Solaris、Linux 等多種作業系統。

若您是 Linux 使用者，基於這款軟體的名氣，您的套件管理器中幾乎一定會有，從套件庫中安裝吧！倘若真找不到，請看[官網下載頁面](http://www.graphviz.org/Download.php)，試試原始碼。

若您是 Windows 用戶，請前往這裡下載安裝檔：[http://www.graphviz.org/Download_windows.php](http://www.graphviz.org/Download_windows.php)

Mac OS X 的使用者請往這邊走：[http://www.graphviz.org/Download_macos.php](http://www.graphviz.org/Download_macos.php)

### Graphviz 的使用

<pre>1  # Graphviz
2  ＞＜cmd＞ ＜inputfile＞ -T ＜format＞ -o ＜outputfile＞
3 
4  # 舉例：輸出 png
5  ＞ dot input.dot -T png -o output.png
6  # 舉例：一樣是輸出 png，只不過檔名是 txt
7  ＞ dot input.dot -T png -o output.txt
</pre>

首先，我們看看上面的 ＜cmd＞ 部份。

Graphviz 的 ＜cmd＞ 有好幾種，每種使用方法都完全相同，差別只在於渲染出來的圖片效果不一樣。man 中的簡介是這樣寫的......

**dot**

渲染的圖具有明確方向性。

**neato**

渲染的圖缺乏方向性。

**twopi**

渲染的圖採用放射性佈局。

**circo**

渲染的圖採用環型佈局。

**fdp**

渲染的圖缺乏方向性。

**sfdp**

渲染大型的圖，圖片缺乏方向性。

您可以透過 man ＜cmd＞ 取得進一步說明。但還是親自用用比較容易理解。在本文中，凡沒有說明的圖，預設都是 以 dot 渲染出來的。

繼續往下看。在 Graphviz 中，若您不指定 -T 參數，Graphviz 並不會自動猜測您想要產生什麼格式，只會以預設格式渲染。可選格式相當多，包括（但不限於）jpg、png、svg 等，全部列表可見[官網說明頁](http://www.graphviz.org/doc/Dot.ref)的最下方。

-o 可讓您指定儲存檔案的檔名。如果您不用 -o 選項指定輸出檔名，Graphviz 則會將結果輸出到標準輸出上。

除非用法很特殊，否則這兩個參數，每次都要記得下達才行。

### dot 語言說明

指揮 Graphviz 繪圖時，所使用的語言叫作 "dot"。下邊就來介紹如何使用它。

#### 有向圖與無向圖

使用 dot 語言，第一步就是決定要畫哪種圖。

圖分兩種：有向圖與無向圖。

<div>[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_02.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_02.jpg)</div>

▲ 圖2：有向圖

<div>[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_03.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_03.jpg)</div>

▲ 圖3：無向圖

有向圖以 digraph 宣告圖片，節點間的關係寫為 "->"。

<pre>1  /*demo1。順便一提，在 dot 語言中可使用 C++ 中允許的註解。本行為 C 風格註解*/
2  digraph demo1{ //這也是註解，C++ 風格的。
3        a -> b -> c;
4        c -> a;
5  }
</pre>

無向圖以 graph 宣告圖片，節點間的關係可以寫為 "--"。

<pre>1  //demo2
2  graph demo2{
3       a -- b -- c;
4       c -- a;
5  }
</pre>

其中 demo1 與 demo2 是圖片的名稱，一般使用時常常留白不打。

#### 使用引號

上文中的 a, b, c 除了作為程式內的識別字以外，也會成為節點的顯示名稱 (label)。不過如果這名稱中混了中文或夾了空格，Graphviz 就有可能搞錯你的意思。

為防不必要的誤解，所以平常最好都用英文引號括住。就像下面這樣：

<pre>1  //demo3
2  digraph {
3      "總 攻" -> "受";
4      "強 攻" -> "受";
5      "健氣攻" -> "受";
6  }
</pre>

[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_04.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_04.jpg)

▲ 圖4：混合了空白的示範，採用 fdp 渲染。

這樣就沒問題了！

#### 子圖與簡化技巧

來看個複雜一點的例子，這是一份地中海海域的大略連接圖：

<pre>1  graph G{
2      "黑海" -- "亞速海";
3      "黑海" -- "博斯普魯斯海峽"
4      "達達尼爾海峽" -- "愛琴海"
5      subgraph cluster_T{ // 新東西
6          label = "黑海海峽"; // 新東西
7           "達達尼爾海峽" -- "馬爾馬拉海" -- "博斯普魯斯海峽";
8      }
9       subgraph cluster_M{
10          label = "地中海海域";
11          "中部地中海" -- {"愛琴海" "愛奧尼亞海" "西西里海峽"}; // 也是新東西
12          "西部地中海" -- {"西西里海峽" "第勒尼安海" "利古里亞海" "伊比利海" "阿爾沃蘭海"};
13          "愛奧尼亞海" -- "亞得裡亞海";
14          "阿爾沃蘭海" -- "直布羅陀海峽";
15     }
16  }
</pre>



[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_05.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_05.jpg)



▲ 圖5：地中海海域連接圖，使用 fdp 渲染，種子為 8 (-Gstart=8)。

這張圖有些新東西可以看。

第一個是 subgraph 關鍵字。一如名字所示，他是用來定義「次級圖片」用的。

次級圖片在 dot 的官方文件中常被叫作 cluster subgraph，特指圖示中被方框包裹起來的那兩塊，其定義方式和一般的 graph 非常相似，不過使用上有兩件事需要留意：

1.  他的命名得以 cluster 前綴開頭，否則語法雖然能過關，但生不出圖面上您預期的效果。
2.  如果父圖是無向圖，他本身也得是無向圖；反之如果父圖是有向圖，這邊也得乖乖照著來。

第二個重點是下面這段：

<pre>1        "中部地中海" -- {"愛琴海" "愛奧尼亞海" "西西里海峽"};</pre>

用大括號括起，用空格分開－這是一口氣將好幾個節點群組起來同時操作的方法，其等效於：

<pre>1        "中部地中海" -- "愛琴海";
2        "中部地中海" -- "愛奧尼亞海";
3        "中部地中海" -- "西西里海峽";
</pre>

您甚至可以用以下程式碼畫出圖6：

<pre>1  digraph G{
2     {a b c} -> {d e f}
3  }
</pre>

[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_06.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_06.jpg)

▲ 圖6：大括號效果示意圖

這語法糖很方便好吃，可以靈活運用。

第三個不同處在於 label = XXX 這行。這是「屬性」的指定方式。

關於屬性，我們下章再講。

#### 屬性

有了前面介紹過的技巧，所有圖面關係都可以順利地繪製出來。

然而，通常我們畫圖的時候，還會對圖片做一些特別的處理。好比說把字加粗、把圖變色、把標籤或連接線的外型 改變、把某些節點水平對齊......諸如此類。

要控制這些東西，就要用到屬性。

屬性有四種：

1.  用在節點上 (Node, N)
2.  用在線段上 (Edge, E)
3.  用在根圖片上 (Graph, G)
4.  用在子圖片上 (Cluster subgraph, C)

您可以閱讀[手冊](http://www.graphviz.org/doc/info/attrs.html)中的表，判斷哪些屬性能用在哪些地方。

那麼，屬性要怎麼用呢？

#### 屬性的套用

如果要設定根圖片或子圖片的屬性，得像前面範例中所示的那樣，在圖片的大括號範圍內指定......

<pre>屬性名稱=值;</pre>

這樣就行了。

對於節點 (node) 的屬性，有以下幾種指定法：

<pre>1  節點名 [節點屬性名=值];
2  節點名 [節點屬性名=值, 節點屬性名=值];
3  node [節點屬性名=值，節點屬性名=值];
</pre>

屬性指定的語句必須要被中括號括起。當一次指定多值時，需用英文逗點隔開。

第三行中的 node 是個關鍵字，用來代稱「圖片範圍內」所有「還沒創建」的節點，或者您也可將它理解為：在當前大括號的範圍內，所有尚未創建節點的屬性預設值，會被這個語句給變更。

線段 (edge) 的屬性指定，與節點屬性指定方式很類似：

<pre>1  節點名 -> 節點名 [線段屬性名=值];
2  節點名 -- 節點名 [線段屬性名=值, 線段屬性名=值];
3  edge [線段屬性名=值，線段屬性名=值];
</pre>

其中 edge 是關鍵字。

這邊順便補充一個關於線段的觀念：有些線段相關的屬性，具有 head 值與 tail 值。而這邊說的 head 與 tail，得將它想 像成一個「箭頭」的形狀（就像是「a -> b」這樣）。

對於線段來說，這個箭頭的頭部才是 head。所以囉，這可能和您最初想像的不一樣，因為這邊說的「Head」其實是 兩個節點中，後面的那一個。

#### 屬性範例

把先前的看過的例子加上一些屬性試試。

<pre>1  graph G{
2      "黑海" [shape = circle, color = blueviolet, fontcolor = blueviolet, fontsize = 20];
3      "黑海" -- "亞速海" [label = "刻赤海峽"];
4 
5      subgraph cluster_T{
6          label = "黑海海峽";
7          fontsize = 24;
8          fillcolor = darkslategray;
9          style = filled;
10         fontcolor = white;
11         node [fontcolor = white, color = white];
12         "博斯普魯斯海峽" -- "馬爾馬拉海" -- "達達尼爾海峽" [color = white];
13         "博斯普魯斯海峽" [shape = parallelogram];
14         "達達尼爾海峽" [shape = parallelogram];
15     }
16 
17      "黑海" -- "博斯普魯斯海峽" [color = red ,penwidth = 2];
18      "達達尼爾海峽" -- "愛琴海" [color = red ,penwidth = 2];
19 
20      subgraph cluster_M{
21          label = "地中海海域";
22          fontsize = 24;
23          "西部地中海" [shape = Mcircle, style = filled, color = grey, fillcolor = aquamarine, fontsize = 20];
24          "中部地中海" [shape = Mcircle, style = filled, color = grey, fillcolor = aquamarine, fontsize = 20];
25          "直布羅陀海峽" [shape = parallelogram, fontcolor = red];
26          "西西里海峽" [shape = parallelogram];
27          "中部地中海" -- {"愛琴海" "愛奧尼亞海" "西西里海峽"};
28          "西部地中海" -- {"西西里海峽" "第勒尼安海" "利古里亞海" "伊比利海" "阿爾沃蘭海"};
29          "愛奧尼亞海" -- "亞得裡亞海";
30          "阿爾沃蘭海" -- "直布羅陀海峽";
31      }
32  }
</pre>



[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_07.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_07.jpg)


▲ 圖7：地中海海域連接圖（加入屬性）。使用 fdp 渲染，種子為 3 (-Gstart=3)。

諸多屬性中，最常用的大概是 label 了。

label 可以決定節點、線段或子圖片上要顯示些什麼。如果您的節點名很長的話，可以在程式內部取個簡短的名稱， 之後透過短名稱操作它，另外透過 label 指定它的顯示內容。

color、fillcolor、fontcolor 這些屬性都是控制顏色用的，不過 fillcolor 只有在 style 被指定為 "filled" 時才會生效。

shape 可以指定節點的形狀，形狀列表[官網這邊](http://www.graphviz.org/content/node-shapes)有一份資料可參考。

線段屬性方面。有向圖中的箭頭可透過 arrowhead 與 arrowtail 屬性來指定頭尾樣式。至於線段本身，則可透過 style 屬 性，指定不同類型的虛線與短截線。使用者還可以用 dir 屬性讓箭頭方向反過來。

另外還有一個 image 屬性，可以指定讓 node 顯示圖片，需要時也可參考看看。

......屬性實在太多，無法一一介紹，請查官網手冊獲得全面[訊息](http://www.graphviz.org/doc/info/attrs.html)。

#### rank

dot 語言中有一個叫作 rank 的概念。

所謂的 rank，在 dot 語言中，含意比較接近於「等級」。他主要用在 dot 渲染器中。

請看以下的圖：

<[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_08.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_08.jpg)  
▲ 圖8：rank 示例。

很明顯可以看出來，圖片被從上到下分為四層－這就是 rank。

下方是與上圖對應的 dot 陳述......

<pre>1  digraph demo{
2     a -> b -> c -> d;
3     b -> {e f};
4  }
</pre>

觀察程式碼，可看出 rank 是如何被指定的。

其基本規則在於：每個線段的頭端，都會比尾端多出一個等級（在圖上面就是往下面一層）。

但等等，如果等級指定的語句彼此矛盾呢？

修改以上程式碼為......

<pre>1  digraph demo{
2     a -> b -> c -> d [label = "rank 增加"];
3     b -> {e f} [label = "rank 增加"];
4     f -> a [label = "不影響 rank"];
5  }
</pre>

[![alt](http://www.openfoundry.org/images/121002/Graphviz/graphviz_09.jpg)](http://www.openfoundry.org/images/121002/Graphviz/graphviz_09.jpg)  

▲ 圖9：rank 示例。

看上面的結果，顯然 rank 的指定是「先說先贏」的。

除了基本規則外，rank 也可以透過屬性來加以調節，有必要時就把手冊抓出來查查看。

### 參考資料

1.  官網屬性手冊，要查屬性就來這看：
     [http://www.graphviz.org/content/attrs](http://www.graphviz.org/content/attrs)。
2.  官網純文字版 Graphviz 使用說明兼 dot 語言手冊：
     [http://www.graphviz.org/doc/Dot.ref](http://www.graphviz.org/doc/Dot.ref)。
3.  如果您偏好 pdf 版本的說明書，請到這邊下載：
     [http://www.graphviz.org/doc/dotguide.pdf](http://www.graphviz.org/doc/dotguide.pdf)。