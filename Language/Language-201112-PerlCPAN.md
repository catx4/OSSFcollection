# Perl CPAN - Comprehensive Perl Archive Network
作者：BlueT / Matthew Lien / 練喆明，2011 年 12 月投稿。

### 簡介

![](http://www.openfoundry.org/images/111213/CPAN/cpan_logo.jpg)

[CPAN - Comprehensive Perl Archive Network](http://cpan.org/) 的中文名稱為「Perl 綜合典藏網」，是各種程式語言線上典藏網的始祖，在它之後許多語言也複製類似的模式，比如 R 語言的 [CRAN](http://cran.r-project.org/)、Tex 語言 [CTAN](http://ctan.org/)、Python 語言的 [PyPI](http://pypi.python.org/) 與 PHP 語言的 [PEAR](http://pear.php.net/) 等。

CPAN 網站自從 1995 年十月上線以來，已經累積了 101,232 個 Perl 模組，共 23,715 個套件，9330 個作者，在全世界有 269 個鏡像伺服器，而且這些數字還在迅速累加。幾乎開發者需要的所有功能在 CPAN 網站上都能找得到實作，而且因為 Perl 程式語言秉持 TMTOWTDI (There's More Than One Way To Do It) 的哲學，針對不同特性強化的實作往往不只一個，讓開發者與使用者可以選擇最適合自己的模組。

CPAN 的使用分成兩個部份：CPAN 網站，以及使用者本機電腦上的工具程式。

*   CPAN 網站：線上列表、查詢模組和各種相關資訊，甚至有線上一鍵安裝的功能，以及針對不同需求發展的介面，例如：
    *   經典站：CPAN.org
    *   搜尋網站：Search.CPAN.org
    *   類似 Google 介面：MetaCPAN.org
*   工具指令程式：本機端的工具指令程式，可以自動化查詢與安裝模組。大部分為文字介面，但也有開發圖形化介面的工具。
    *   經典老工具：
        *   `cpan`: 通常只要裝了 Perl 就會附上這個最傳統的工具。
    *   強大的 CPANPLUS
        *   `cpanp`: 自 2007 年開始加入預載的強大 CPAN 工具。
    *   方便好用的 cpanminus
        *   `cpanm`: 新的工具，資源消耗量少，不需繁雜的設定，極為推薦。
    *   ActiveState 公司的 perl package manager
        *   `ppm`: 專有工具，不支援某些模組。

### 安裝

如同 Perl 本身，CPAN 相關工具存在或支援大部分現有的軟硬體。以下將針對三種類型的作業系統簡略介紹如何安裝 Perl 以及上面提到的 `cpanminus`。古老的 cpan 不用另外安裝，只要有 Perl 就有 `cpan` 指令工具。

####  安裝 Perl

#### A. 微軟 Windows 系統

微軟的 Windows 系統上並沒有預設安裝 Perl，所以必須自行安裝。可以選擇的有老牌商業公司 ActiveState 推出的 [ActivePerl](http://www.activestate.com/activeperl/downloads) 以及 OpenSource 的 [Strawberry Perl](http://strawberryperl.com/) 。我們將以 Strawberry Perl 為安裝範例，它甚至包涵了 C 編譯器和 `make` 工具。

[![安裝 Perl 於 Windows 上](http://www.openfoundry.org/images/111213/CPAN/cpan_windows.png)](http://www.openfoundry.org/images/111213/CPAN/cpan_windows.png)

▲ 圖1：安裝 Perl 於 Windows 上

#### B. Unix-like 系統（Linux、*BSD、Hurd 及其他，除了 MacOSX）

Perl 被喻為 Unix 系統上的瑞士刀。幾乎所有 Unix-like 的系統都預設有 Perl，不需自行安裝。若進行手動安裝，則必須安裝「make」這個工具以及 C 編譯器。

在 Ubuntu 與 Debian 系的系統上可以透過 apt 安裝，請於命令列模式下，輸入下列指令：

```
$ sudo apt-get install make gcc

```

#### C. MacOSX 系統

在 MacOSX 上 Perl 已經預設安裝好了，不需要另外安裝。

不過如果需要安裝 Perl 模組的話，必須安裝 OSX 安裝光碟中隨附的 developer 套件 xcode（只需安裝 'unix dev tools'）。

[![安裝 Perl 於 MacOS 上](http://www.openfoundry.org/images/111213/CPAN/cpan_macos.jpg)](http://www.openfoundry.org/images/111213/CPAN/cpan_macos.jpg)

▲ 圖2：安裝 Perl 於 MacOS 上

####  安裝 cpanminus

#### A. 微軟 Windows 系統

只能用 cpan 安裝 App::cpanminus 模組。請於命令列模式下，輸入下列指令：

```
> cpan App::cpanminus

```

#### B. Unix-like 系統（Linux、*BSD、Hurd 及其他，除了 MacOSX）

在 Unix-like 系統上，則有許多不同的安裝方式：

* 以 cpan 安裝

請於命令列模式下，輸入下列指令：

```
$ cpan App::cpanminus

```

*  安裝到系統

下載最新版的 cpanminus，並用它來安裝到系統路徑。

請於命令列模式下，輸入下列指令：

```
$ curl -L http://cpanmin.us | perl - --sudo App::cpanminus

```

*  安裝到使用者的個人目錄

如果您在個人目錄使用 perlbrew ( http://www.perlbrew.pl/ ) 安裝了自己的 perl，您就不需要「--sudo」了。

請於命令列模式下，輸入下列指令：

```
$ curl -L http://cpanmin.us | perl - App::cpanminus

```

* 下載獨立執行檔

下載後即可使用。請於命令列模式下，輸入下列指令：

```
$ curl -LO http://xrl.us/cpanm && chmod +x cpanm

```

### 使用範例（以 cpanminus 為例）

#### 安裝模組

* 使用 perlbrew，或使用之帳號擁有系統寫入權限時

請於命令列模式下，輸入下列指令：

```
$ cpanm 模組名稱

```

* 非使用 perlbrew 且使用之帳號沒有系統寫入權限時

若您不是使用 `perlbrew` 且帳號沒有系統寫入權限時，則需要加上 "-S" (sudo) 參數。

請於命令列模式下，輸入下列指令：

```
$ cpanm -S 模組名稱

```

* 安裝所有相依模組

一般來說 `cpanm` 會幫您安裝好您所指定的模組，以及所有相依模組。但若您只想安裝相依模組，而不安裝指定模組本身的話，您可以這樣做。


####  從網址安裝，並安裝到特定目錄中

若您有特定需求，希望將此模組及其相依模組安裝到特定位置的話，可以用這個指令。

* 使用 perlbrew，或使用之帳號擁有系統寫入權限時

請於命令列模式下，輸入下列指令：

```
$ cpanm -L ~/another/lib/ http://example.com/path/[模組檔案名稱].tar.gz

```

* 非使用 perlbrew 且使用之帳號沒有系統寫入權限時

若您不是使用 `perlbrew` 且帳號沒有系統寫入權限時，則需要加上 "-S" (sudo) 參數。

請於命令列模式下，輸入下列指令：

```
$ cpanm -S -L ~/another/lib/ http://example.com/path/[模組檔案名稱].tar.gz

```

#### 從本地檔案安裝

若您沒有網路時，您可以直接安裝預先下載好的模組壓縮檔。

* 使用 perlbrew，或使用之帳號擁有系統寫入權限時

請於命令列模式下，輸入下列指令：

```
$ cpanm ~/tarball/[模組檔案名稱].tar.gz

```

* 非使用 perlbrew 且使用之帳號沒有系統寫入權限時

若您不是使用 `perlbrew` 且帳號沒有系統寫入權限時，則需要加上 "-S" (sudo) 參數。

請於命令列模式下，輸入下列指令：

```
$ cpanm -S ~/tarball/[模組檔案名稱].tar.gz

```

#### 從已解開的目錄裡安裝

若您在某個資料夾中開發自己的模組，可以直接從資料夾裡安裝。

* 使用 perlbrew，或使用之帳號擁有系統寫入權限時

請於命令列模式下，輸入下列指令：

```
$ cpanm ./path/[模組目錄名稱]

```

* 非使用 perlbrew 且使用之帳號沒有系統寫入權限時

若您不是使用 `perlbrew` 且帳號沒有系統寫入權限時，則需要加上 "-S" (sudo) 參數。

請於命令列模式下，輸入下列指令：

```
$ cpanm -S ./path/[模組目錄名稱]

```

#### 執行自體版本升級

有句經典名言是這麼說的：「無論多完美的軟體，在其所有的程式碼中，一定至少有一個 Bug，以及至少一行可以被精簡的程式碼」。當您想更新 `cpanminus` 以取得修正過的新版本，您可以這樣做。

* 使用 perlbrew，或使用之帳號擁有系統寫入權限時

請於命令列模式下，輸入下列指令：

```
$ cpanm --self-upgrade

```

* 非使用 perlbrew 且使用之帳號沒有系統寫入權限時

若您不是使用 `perlbrew` 且帳號沒有系統寫入權限時，則需要加上 "-S" (sudo) 參數。

請於命令列模式下，輸入下列指令：

```
$ cpanm -S --self-upgrade

```

###其他相關的好東西

#### 檢查模組更新

時光飛逝，人總會成長，Perl 模組也一樣。若您想找出哪些模組已經有新版本（BugFix 或新功能），您可以安裝 App::cpanoutdated。

請於命令列模式下，輸入下列指令：

```
$ cpanm App::cpanoutdated

```

#### 列出所有過時的模組

請於命令列模式下，輸入下列指令：

```
$ cpan-outdated

```

#### 自動更新

甚至可以結合 cpanminus 做自動更新！

a. 使用 perlbrew，或使用之帳號擁有系統寫入權限時

請於命令列模式下，輸入下列指令：

```
$ cpan-outdated | cpanm

```

b. 非使用 perlbrew 且使用之帳號沒有系統寫入權限時

若您不是使用 `perlbrew` 且帳號沒有系統寫入權限時，則需要加上 "-S" (sudo) 參數。

請於命令列模式下，輸入下列指令：

```
$ cpan-outdated | cpanm -S

```

####. 顯示模組的版本差異

時間總會在人的臉上留下痕跡，對於 Perl 模組來說也一樣。世界瞬息萬變，幸運的是，您可以清楚知道 Perl 模組每一個版本的成長中，到底改變了些什麼。

若您想瞭解每一個 Perl 模組的 Changelog，您可以安裝 cpan-listchanges。

請於命令列模式下，輸入下列指令：

```
$ cpanm cpan-listchanges

```

#### 列出「目前所使用的」和「CPAN 上最新版的」之間的變動

請於命令列模式下，輸入下列指令：

```
$ cpan-listchanges 模組名稱

```

* 列出版本 x 到版本 y 之間的變動

若不知道最新版本號，可以用「HEAD」表示。

請於命令列模式下，輸入下列指令：

```
$ cpan-listchanges 模組名稱@舊版本號..新版本號

```

例如：

```
$ cpan-listchanges My::
 Module@0.123..HEAD 

```

* 顯示自始至今的所有變動

請於命令列模式下，輸入下列指令：

```
$ cpan-listchanges --all 模組名稱

```

* 列出所有需更新之 Perl 模組的變動

可以跟 cpan-outdated 結合，列出所有需要更新的 Perl 模組的變動。

請於命令列模式下，輸入下列指令：

```
$ cpan-outdated -p | xargs cpan-listchanges

```

#### 移除模組

人都有不堪的過去，而且往往難以抹去。但如果您是想把從前裝錯或是不想要的模組抹去，那倒是可行的。

若想要移除 Perl 模組，您可以安裝 App::pmuninstall。

請於命令列模式下，輸入下列指令：

```
$ cpanm App::pmuninstall

```

#### 移除指定之模組

需要移除模組時，請於命令列模式下，輸入下列指令：

```
$ pm-uninstall 模組名稱

```

例如：

```
$ pm-uninstall App::pmuninstall

```

#### 強制移除指定之模組

請於命令列模式下，加上參數 "-f"，並輸入下列指令：

```
$ pm-uninstall -f 模組名稱

```

1.  移除指定之模組時，不檢查模組相依性

請於命令列模式下，加上參數 "-n"，並輸入下列指令：

```
$ pm-uninstall -n 模組名稱

```

### 優缺點

CPAN / CPANPLUS / cpanminus

*   CPAN 是最傳統的工具，Bug 最少，具備所有的常用功能。Perl 預設內建。缺點是稍嫌麻煩。
*   CPANPLUS 功能強大，Perl 已預設內建。但對於一般使用者來說操作上仍稍嫌複雜。
*   cpanminus 功能陽春，預設未包涵 Perl，但方便好上手。
*   安裝在系統 vs 個人環境
    *   系統
        *   優點 : 許多作業系統都提供了統一且方便好用的軟體管理工具，通常其中也包涵了很多預先編譯好的 Perl 模組套件，讓所有的升級及控管更容易，而且只要安裝一次，系統上的所有使用者都能受惠。
        *   缺點 : 大部分作業系統套件庫中，所包涵的 Perl 模組都不是最新版的，許多 BugFix 嶄新功能無法使用。且除非您有系統管理員權限，否則無法更動任何 Perl 模組。
    *   個人環境
        *   優點 : 使用者能自由控制並使用自己喜歡的環境。每個使用者的每個 Perl 版本環境皆完全獨立，互不影響。
        *   缺點 : 若許多使用者的許多環境都需要相同模組，就必須每個都重複安裝，無法共享。重複的 Perl 模組與套件安裝會耗費許多不必要的磁碟空間，以及編譯時的 CPU 資源 

### 結語
    學會 Perl，行遍全球。[CPAN](http://cpan.org) 十多年來累積了蘊藏量極為豐富的套件庫，甚至可以因應各種不同需求而直接引入或橋接其他語言，各種想得到與想不到的模組都已經被撰寫完成，便於直接使用。面對如此大量的模組庫藏，有時難免感到眼花撩亂，但只要善用 CPAN 網站上的評分系統與 RT（錯誤追蹤系統 [http://rt.cpan.org](http://rt.cpan.org)）資訊，通常就能協助您找到合適的模組，大幅減輕您的開發壓力，縮短開發時間，並提昇寫作品質。