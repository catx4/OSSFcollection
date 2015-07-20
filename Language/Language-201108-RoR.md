# 麥克阿忠的 Ruby on Rails 初探
作者：麥克阿忠，2011 年 8 月投稿。



### 起源與命名

Ruby 原意為紅寶石，而在電腦程式碼界裡頭是一個簡單快速、物件導向的指令碼語言。Ruby 的起源可以追溯到 1995 年，由日本工程師「松本行弘」開發釋出，並遵循 GPL 協定和 Ruby License。之所以命名為 Ruby，是因為 Perl 的發音與 6 月的誕生石 pearl（珍珠）相同，因此 Ruby 以 7 月的誕生石 ruby（紅寶石）命名。

### Ruby 的理念與語言特性

Ruby 發明者松本行弘曾說：

> 「人們特別是電腦工程師們，常常從機器著想。他們認為：『這樣做，機器就能執行的更快；這樣做，機器執行效率更高；這樣做，機器就會怎樣怎樣怎樣。』實際上，我們需要從人的角度考慮問題，人們怎樣編寫程式或者怎樣使用機器上應用程式。我們是主人，他們是僕人。」

減少瑣碎的時間來提升開發效率與直接溝通的人性化語法，是 Ruby 開發時所遵照的理念。所以發明者松本行弘認為 Ruby > (Smalltalk + Perl )/2，可表示為能像 Smalltalk 一樣完全、完整的物件導向，指令碼執行又有 Perl 強大的文字處理功能的程式語言。

語法：

####取絕對值
    -199.abs
    => 199

####計算字串長度含空白
    "ruby is cool".length
    => 12

####取含 c 字串的位置
    "Rick Astley".index("c")
    => 2

Ruby 可以將任何的東西視為物件，不必再另外宣告基礎型別。

### 什麼是 Ruby on Rails

Rails 意指為鐵道，所以 Ruby on Rails 可想像為遵行在已規劃好的鐵路上，以穩定、快速、便捷的運作整個 Web 專案。

Rails 的創始人「大衛．海納梅爾．韓森」於 2004 年 7 月從 37signals 公司的管理工具 Basecamp 分離出 Ruby on Rails，然後再以開源方式發佈。Ruby on Rails 簡稱 RoR 或是 Rails，使用 Ruby 語言所開發編寫的開源 Web 應用框架，嚴格按照 MCV 架構開發。其架構採取 Model、View、Controller 分離的開發式，不但減少了開發者與美工之間的落差，也簡化許多繁雜的動作。

### 建立所需的環境

Ruby 可在包含 Linux、Mac OS X 與 Microsoft Windows 下運作，本篇將在這三個系統下介紹安裝 RoR 的環境。

### 在 Ubuntu 11.04 下安裝 Ruby on Rails

#### 進行系統更新與安裝 MySQL


    $ sudo apt-get update

    $ sudo apt-get upgrade

    $ sudo apt-get install git

    $ sudo apt-get install mysql-server libmysqlclient15-dev

    $ sudo apt-get install curl

    $ sudo apt-get install build-essential zlib1g-dev libssl-dev libreadline5-dev

#### 安裝 RVM (Ruby Version Manager)

    $ bash <

    $ echo "[[ -s $HOME/.rvm/scripts/rvm ]] && source $HOME/.rvm/scripts/rvm" >>~/.profile && . ~/.profile

    $ source ~/.bash_profile

再打開該檔案 .bashrc 於最後一行加上 source ~/.bash_profile

#### 安裝 REE (Ruby Enterprise Edition)

    $ rvm install ree

    $ rvm ree --default

#### 安裝 Rails

    $ sudo apt-get install libbuilder-ruby

    $ gem install rails -v=3.0.7

    $ gem install mysql

以上即完成了 Ubuntu 11.04 下的開發環境了。

## 在 Mac OS X 下安裝 Ruby on Rails

在撰寫文章之際，新版 Mac 系統已經正式出版，名為 Mac OS X Lion，版本代號為 10.7，因尚未得知版本是否相容，本篇將以 10.6.8 的方式安裝。

#### 安裝 Xcode

安裝 Xcode 的用意是要與 Homebrew 搭配使用。

Mac OS X Install CD >> 選擇安裝，若無 CD 者到 [http://developer.apple.com/xcode/](http://developer.apple.com/xcode/) 安裝最新版的 Xcode，升級至 Lion 版者可獲得免費升級，其餘版本有可能會需要付費，除非您是專職 Apple 軟體開發者或是有意願在 Mac 平台上開發者，可以付費購買。

#### 安裝 Homebrew

     ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"

     brew install git

     brew update

#### 安裝 MySQL

     brew install mysql

     unset TMPDIR

     mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)"

     cp "$(brew --prefix mysql)"/com.mysql.mysqld.plist ~/Library/LaunchAgents

     launchctl load -w ~/Library/LaunchAgents/com.mysql.mysqld.plist

     "$(brew --prefix mysql)"/bin/mysql_secure_installation

* Set root password? [Y/n] Y

* New password: 123456

* Re-enter new password: 123456

* Remove anonymous users? [Y/n] Y

* Disallow root login remotely? [Y/n] Y

* Remove test database and access to it? [Y/n] Y

* Reload privilege tables now? [Y/n] Y

#### 安裝 RVM (Ruby Version Manager)

     bash <

     echo "[[ -s $HOME/.rvm/scripts/rvm ]] && source $HOME/.rvm/scripts/rvm" >> ~/.profile && . ~/.profile

     source ~/.profile

#### 安裝 REE ( Ruby Enterprise Edition )

     rvm install ree

     rvm ree --default

#### 修正在使用 RVM 裝 REE 之後不能使用中文的問題

     brew install readline

     brew link readline

     rvm --reconfigure --force -C --with-readline-dir=/usr/local install ree

#### 安裝必要的 RubyGems

     gem install rails

     gem install rails -v=3.0.7

     gem install mysql2

     gem install passenger

     gem install nokogiri

     gem install capistrano

     gem install capistrano-ext

     gem install delayed_job

> gem install hoptoad_notifier

> gem install facebooker2

> gem install factory_girl

> gem install sphinx

以上即完成了 Mac OS X 下的開發環境。

### 在 Windows 下安裝 Ruby on Rails

安裝非常簡單，此段將簡單說明需要的步驟。

#### 下載一鍵安裝的 rubyinstaller.exe

到 RubyForge 網站 [http://rubyforge.org/frs/?group_id=167](http://rubyforge.org/frs/?group_id=167) 下載最新版本的 Ruby 程式。

#### 安裝 Rails

開啟「文字命令」模式輸入

C:/> gem install rails –include-dependencies

請記得保持網路暢通，此動作將透過網路下載相關的檔案來安裝。

#### 安裝 MySQL

到 MySQL 官方網站下載最新版本 [http://dev.mysql.com/downloads/
](http://dev.mysql.com/downloads/)

以上即完成了 Windows 下的安裝

#### 磨刀小試

Ruby on Rails 開發所使用的編輯器有很多種，其中最簡便的就是用系統預設的文字編輯器 vi 或是 vim 即可開發。

先下一些指令來檢視您的環境版本。

檢視 Ruby 的版本：

$ ruby -v

ruby 1.8.7 (2011-02-18 patchlevel 334) [i686-linux], MBARI 0x8770, Ruby Enterprise Edition 2011.03

檢視 Rails 的版本：

$ rails -v

Rails 3.0.7

檢視 rake 版本：（此 V 為大寫）

$ rake -V

rake, version 0.8.7

若以上的指令能顯示版本資訊，表示您可以用 Ruby on Rails 進行開發了。

開一個新專案：

建立一個新成立的網站，我們必須要當它為一個專案來進行開發，所以請在您欲指定的家目錄下新開一個專案，

請下這個指令：

$ rails new demo

create

create README

create Rakefile

create config.ru

create .gitignore

create Gemfile

create app

create app/controllers/application_controller.rb

create app/helpers/application_helper.rb

create app/mailers

create app/models

create app/views/layouts/application.html.erb

create config

create config/routes.rb

......

......

rails 將會為您產生關於 MCV 架構的檔案目錄。然後進入您的 demo 目錄下輸入

$ rails s

=> Booting WEBrick

=> Rails 3.0.7 application starting in development on http://0.0.0.0:3000

=> Call with -d to detach

=> Ctrl-C to shutdown server

[2011-07-21 14:14:09] INFO WEBrick 1.3.1

[2011-07-21 14:14:09] INFO ruby 1.8.7 (2011-02-18) [i686-linux]

[2011-07-21 14:14:09] INFO WEBrick::HTTPServer#start: pid=2967 port=3000

然後打開 Web browser 在網址輸入

http://localhost:3000[

](http://localhost:3000)看到以下的畫面，表示您完成初探 Ruby on Rails 的第一步了！

[![http://www.openfoundry.org/images/110809/ruby_on_rails.png](http://www.openfoundry.org/images/110809/ruby_on_rails.png)](http://www.openfoundry.org/images/110809/ruby_on_rails.png)

▲ 圖1

### 結語

Ruby 相較於其它程式語言較為直覺、容易閱讀，也易於上手。一般來說，只要能在物件導向論點中精實的學習，可以加速一倍以上的開發時效。在 Web 開發中，大都是用 Rails 做為 FrameWork 來共同開發大型專案，因 MCV 為開發的出發點，大部分資深的程式設計師都會比較不習慣，相信了解它的架構之後，個人的學習曲線將能一躍直上。

下一篇將進入如何用 Ruby on Rails 寫出一個網站，敬請期待。

### 資源連結

Ruby on Rails 實戰聖經 [http://ihower.tw/rails3/](http://ihower.tw/rails3/)

Ruby on Rails 指南手冊 [http://guides.ruby.tw/rails3/index.html](http://guides.ruby.tw/rails3/index.html)

二十分鐘 Ruby 體驗 [http://www.ruby-lang.org/zh_TW/documentation/quickstart/](http://www.ruby-lang.org/zh_TW/documentation/quickstart/)

Ruby on Rails 教學影片 [http://railscasts.com/ RailsCasts](http://railscasts.com/)

從 PHP 轉換 Ruby 的方法 [http://railsforphp.com](http://railsforphp.com)

### 作者簡介

麥克阿忠，資深網站程式開發者，興趣攝影。目前擔任 Ruby on Rails 網站開發員，主要使用 Ubuntu Server 進行 Web 應用程式開發。
 作者部落格 [http://about.me/MichaelChen520](http://about.me/MichaelChen520%20)
 歡迎對 Ruby 有興趣的同好一同來交流指教。