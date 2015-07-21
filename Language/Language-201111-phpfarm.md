# phpfarm - install multiple versions of PHP beside each other
作者：吳柏毅 (Appleboy)，2011 年 11 月投稿。

## 前言

有時候在網路下載現成的 PHP 論壇或其他圖片管理軟體，例如 [phpBB](http://www.phpbb.com/) 或 [Gallery](http://gallery.menalto.com/)，安裝後卻出現很多錯誤訊息。初學者面對這種情形一定非常困擾，心中猜想到底是該軟體的問題，還是自己安裝步驟不正確。

身為 PHP 軟體工程師，需要注意 PHP 各版本之間的差異，例如函式是否已經被官方移除等。這些改變都會造成開發者或使用者的困擾。因此，在每次釋出新版軟體前，應該進行 PHP 各版本的相容性測試，有以下兩種作法：

1.  使用不同台機器測試不同 PHP 版本。
2.  在同一台機器上安裝多個不同的 PHP 版本。

第 1 種方式需要很多硬體資源和安裝時間，除此之外，不同的機器應部署相同的測試環境。相對來說，第 2 種作法的好處顯而易見，除了可以一次解決軟體相依性的問題，執行跨版本的測試也比較容易。

本篇文章將以 phpfarm 介面解決上述遇到的問題。後續的指令及操作環境皆於 Ubuntu 發行套件下進行。

### 安裝 phpfarm

####下載-phpfarm-專案####


**I. 從 SourceForge 網站下載**

phpfarm 可以從 SourceForge 下載，專案網址為：

```
http://sourceforge.net/projects/phpfarm

```

**II. 透過 git 下載**

請先確認系統已正確安裝 `git`，若無則用下列指令進行安裝：

```
$ sudo apt-get install git

```

接著使用 `git` 的指令下載 phpfarm 專案：

```
$ git clone git://git.code.sf.net/p/phpfarm/code phpfarm

```



####進行編譯


phpfarm 專案目錄中，主要有 "inst" 及 "src" 兩個目錄。"inst" 目錄存放編譯過的執行檔，而 "src" 則存放編譯需要的 script 腳本語言及原始碼。

**I. 簡易編譯**

先介紹最簡單的編譯方式：

```
$ cd phpfarm/src
$ ./compile 5.3.2

```

phpfarm 會從 php.net 官方網站中，下載 PHP 5.3.2 版本的原始檔進行編譯。如果編譯過程沒有發生錯誤，則可以在 " inst/bin/ " 目錄下找到 "php-5.3.2" 的目錄及相關檔案。

接著使用下列指令檢查 PHP 版本（需注意安裝路徑）：

```
$ ./inst/bin/php-5.3.2 -v
PHP 5.3.2 (cli) (built: Oct 22 2011 18:38:53) (DEBUG)
Copyright (c) 1997-2010 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2010 Zend Technologies

```

**II. 進階編譯**

上述簡易編譯 PHP 版本的功能，可能無法符合某些人的需求。例如，編譯後的 PHP 版本，預設並沒有支援 MySQL 相關函式，因此必須在 phpfarm 安裝執行檔中手動增修。

官方網站似乎沒有增修教學，但實際上可以在 "src/compile.sh" 這個檔案中找到相關設定。執行 "./compile 5.3.2" 時，會先載入 "src/options.sh"，不同版本可以指定不同的設定檔。

因此，如果要編譯 php 5.3.2，則需要在 "src" 目錄下，新增名為 "custon-options-5.3.2.sh" 的檔案，內容範例如下：

```
configoptions="\
--enable-bcmath \
--enable-calendar \
--enable-exif \
--enable-ftp \
--with-mysql \
--with-mysqli \
--enable-mbstring \
--enable-pcntl \
--enable-soap \
--enable-sockets \
--enable-sqlite-utf8 \
--enable-wddx \
--enable-zip \
--with-zlib \
--with-gettext \

```

也可以任意更改版本編號來編譯不同版本，檔案架構如下：

```
# 原始設定檔
$ custom-options.sh
# PHP 5 設定檔
$ custom-options-5.sh
# PHP 5.3 設定檔
$ custom-options-5.3.sh
# PHP 5.3.1 設定檔
$ custom-options-5.3.1.sh

```

**III.開始安裝**

如果希望在任何地方都可以使用 phpfarm 編譯後的 PHP 版本，必須設定環境變數。

假設完整的 php-5.3.2 路徑為 "/otp/phpfarm/inst/bin/php-5.3.2"（此路徑依據先前下載及編譯 phpfarm 時的路徑），則需修改 PATH 環境變數值如下：

```
$ export PATH=/opt/phpfarm/inst/bin:$PATH

```

需要注意的是，此設定方式適用於 bash 及其相容的環境中。

### 使用範例

####01\. 於 Apache 中執行不同的 PHP 版本

本篇將介紹在 Apache 中以 FastCGI 的方式整合多個 PHP 版本。

**I. 安裝 FastCGI**

請於命令列模式下輸入下列指令：

```
$ sudo apt-get install apache2.2-bin apache2.2-common apache2-mpm-worker libapache2-mod-fcgid libxml2-dev

```

安裝好之後，我們透過 a2enmod 指令啟動 fastcgi：

```
$ sudo a2enmod fastcgi
$ sudo a2enmod actions

```

**II. 設定 FastCGI**

接著，新增或修改 "/etc/apache2/conf.d/php-cgisetup.conf" 的檔案，內容如下：

```
FastCgiServer /var/www/cgi-bin/php-cgi-5.3.0
FastCgiServer /var/www/cgi-bin/php-cgi-5.3.1
FastCgiServer /var/www/cgi-bin/php-cgi-5.3.2
ScriptAlias /cgi-bin-php/ /var/www/cgi-bin/

```

**III. 設定 PHP-CGI**

上述步驟建立了 3 個不同的 PHP CGI 版本。請依序將此 3 個檔案輸入相關的 PHP-CGI 設定。

先改變檔案屬性為可執行，並且分別將以下程式碼填入，以 php-cgi-5.3.2 為例： ， #!/bin/sh PHPRC="/etc/php5/cgi/5.3.2/" export PHPRC

```
PHP_FCGI_CHILDREN=3
export PHP_FCGI_CHILDREN

PHP_FCGI_MAX_REQUESTS=5000
expoty PHP_FCGI_MAX_REQUESTS
exec /opt/phpfarm/inst/bin/php-cgi-5.3.2

```

其它的檔案可仿照上述內容。記得將檔案屬性設定為可執行，並修正版本編號。

需要特別注意 PHPRC 設定，請務必依照此設定建立一個空目錄。在本範例中，需建立 "/etc/php5/cgi/5.3.2" 的空目錄。

**IV. 在 VirtualHost 設定單一 PHP 版本**

完成前述動作之後，最後一個步驟就是設定 VirtualHost。

Ubuntu 的 Virtual Host 放置於 "/etc/apache2/sites-available" 目錄。可以在該目錄下，建立 phpfarm 的 Virtual Host，內容如下：

```
AddHandler php-cgi .php
Action php-cgi /cgi-bin-php/php-cgi-5.3.2
Options Indexes FollowSymLinks MultiViews ExecCGI
AllowOverride all
Order allow,deny
allow from all
SetHandler php-cgi

```

接著執行：

```
$ sudo a2ensite phpfarm

```

未來若要測試 PHP 5.3.2 版本時，只需確認環境是運行在 Apache phpfarm 這個 Virtual Host 下即可。執行後可用 phpinfo() 來觀看 PHP 版本資訊。

[![安裝完成圖](http://www.openfoundry.org/images/111122/phpfarm_finish.jpg "安裝完成圖")](http://www.openfoundry.org/images/111122/phpfarm_finish.jpg)

若要測試其它 PHP 版本，僅需修改 phpfarm 的 Virtual Host 設定，修正 "Action php-cgi" 這一行即可。

#### 02\. 在 phpfarm 上安裝 PHP extensions

本範例將介紹安裝 Pecl-APC 模組於 phpfarm 管理的版本中。



**I. 安裝 php-pear**

首先，請於命令列模式下輸入下列指令，以安裝 php-pear：

```
$ sudo apt-get install php-pear

```

使用 pear 搜尋套件之前，請先手動更新 pear channel：

```
$ sudo pear channel-update doc.php.net
$ sudo pear channel-update pear.php.net
$ sudo pear search -a apc

```

pear 會依序於 channel 中搜尋 apc 套件。顯示如下：

```
Retrieving data...0%
no packages found that match pattern "apc", for channel doc.php.net.
Retrieving data...0%
no packages found that match pattern "apc", for channel pear.php.net.
Retrieving data...0%
.Matched packages, channel pecl.php.net:
=======================================
Package Stable/(Latest) Local
APC 3.1.9 (stable) Alternative PHP Cache

```

接著，使用下列指令下載 Pecl-APC 原始檔：

```
$ sudo pear download pecl/APC
downloading APC-3.1.9.tgz ...
Starting to download APC-3.1.9.tgz (155,540 bytes)
.................................done: 155,540 bytes
FileAPC-3.1.9.tgz downloaded
$ sudo tar -zxvf APC-3.1.9.tgz

```




**II. 安裝 Pecl-APC**

手動安裝 PHP extension 之前，先來介紹 phpize 指令是做什麼用的。phpize 屬於 php-devel，主要用在設定 php 外掛模組，所以安裝 php-devel 相關套件後就可使用 phpize（執行檔預設存放於 /usr/bin/phpize）。如果透過 phpfarm 安裝 PHP 環境，phpize 會被安裝到 /opt/phpfarm/inst/bin/phpize-$version。此外，要執行 phpize 之前，必須先安裝 [autoconf](http://www.gnu.org/s/autoconf/) 套件才能順利執行。

安裝 autoconf 前，請於命令列模式下輸入下列指令：

```
$ sudo aptitude install autoconf

```

接著安裝 APC extension：

```
$ cd APC-3.1.9
$ /opt/phpfarm/inst/bin/phpize-5.3.2
Configuring for:
PHP Api Version: 20090626
Zend Module Api No: 20090626
Zend Extension Api No: 220090626
$ sudo ./configure --with-php-config=/opt/phpfarm/inst/bin/php-config-5.3.2
$ make && make install
......
Installing shared extensions: /opt/phpfarm/inst/php-5.3.2/lib/php/extensions/debug-non-zts-20090626/
Installing header files: /opt/phpfarm/inst/php-5.3.2/include/php/

```

安裝完畢之後，就會產生 apc.so，如果要啟動此 extension，請務必在 php.ini 加入以下設定：

```
extension_dir=/opt/phpfarm/inst/php-5.3.2/lib/php/extensions/debug-non-zts-20090626
extension=apc.so

```

透過以下指令可以知道 php.ini 檔案的位置：

```
$ /opt/phpfarm/inst/bin/php-5.3.2 --ini
Configuration File (php.ini) Path: /opt/phpfarm/inst/php-5.3.2/lib
Loaded Configuration File: /opt/phpfarm/inst/php-5.3.2/lib/php.ini

```

### 結論

對於在寫大型 Open source project 的開發者來說，有 phpfarm 這類軟體測試不同環境下軟體的狀況，真的是一大福音。畢竟開發出來好用的軟體應該相容於各 PHP 版本，並能解決相依性問題，而在單一台電腦安裝多重版本可以省下很多測試時間。

