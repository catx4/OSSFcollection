# RubyGems─管理你的紅寶石
作者：林健欣 (godfat)，2011 年 12 月投稿

### 前言

![RubyGems Logo](http://www.openfoundry.org/images/111213/gems/rubygems_logo.jpg)  
[RubyGems](https://rubygems.org) 是 Ruby 社群最常使用的套件管理系統，如同 Perl 的 CPAN 或 Python 的 EasyInstall / pip，使用者可以很簡易地安裝及管理套件。與其它的套件管理系統一樣，RubyGems 也有版本與相依性管理。

RubyGems 大致可以分為兩個部份。一個是 gem 命令，另一個則是在 runtime 中管理可用的 gems 與其版本。在這篇淺談中，將不會介紹所有的功能與選項，使用者請自行參閱 `gem help`。

### 安裝

在 Ruby 1.9 之前，RubyGems 需要額外安裝，因為它不是官方的套件。而在 Ruby 1.9 之後，已被收錄至 Ruby 標準發行版中。差別在於收錄的版本可能比較舊。不過沒關係，因為 RubyGems 本身也可以由 RubyGems 散佈，因此只要先取得一份可以執行的 RubyGems，接著要管理都很容易。


#### 01\. Mac OS X

在 Mac OS X 中，雖然 Ruby 的版本是 1.8.7，但是 RubyGems 也已經預先裝好了。

#### 02\. Linux 發行版

在 Linux 上使用該發行版的套件管理系統即可。如果已是 Ruby 1.9 則不需要再安裝 RubyGems，若是較舊的 Ruby 1.8，通常 Linux 上的套件管理系統也有提供 RubyGems。

若真的沒有，可以下載 [tarball](https://rubygems.org/pages/download)，解開後執行下列指令安裝：

```
$ ruby setup.rb

```

#### 03\. Windows

Windows 可直接使用 [RubyInstaller](http://rubyinstaller.org/downloads)，不管是 1.9 或是 1.8，RubyGems 都已經被包在裡面了。

### 使用範例

#### 01\. 顯示安裝的 gems

請於命令列模式下輸入下列指令：

```
$ gem list

```

預設是顯示目前本機安裝的 gems。

若要查詢遠端的 gems，則需加上 `-r` (remote) 參數如下：

```
$ gem list -r

```

表示我們只關心遠端 (remote) 的 gems，而非本地 (local) 的 gems。

例如，下列指令將列出 [rubygems.org](https://rubygems.org) 上所有 rake 開頭的 gems:

```
$ gem list -r rake

```

由於效率考量，只會印出最新的版本。

如需察看之前的版本，例如某個新版的 gem 有問題，但又只能安裝較舊版本時，可以再加上 `-a` (all) 參數，可以輸入下列指令：

```
$ gem list -ra rake

```

如果只關心 rake 本身，而不在意其他 rake 開頭的 gem，那我們可以用 regular expression 來處理，如下列指令：

```
$ gem list -ra '^rake/div>

```

#### 02\. 安裝 gems

請於命令列模式下輸入下列指令：

```
$ gem install [套件名稱]

```

預設會安裝目前最新的版本。

亦可以使用 "-v" 參數來指定安裝特定版本，如下：

```
$ gem install -v 1.7.2 [套件名稱]

```

#### 03\. 更新 gems

請於命令列模式下輸入下列指令：

```
$ gem update [套件名稱]

```

如果想要更新所有已安裝的 gems，則僅使用 `update` 參數即可，如下：

```
$ gem update

```

#### 04\. 移除 gems

請於命令列模式下輸入下列指令：

```
$ gem uninstall [套件名稱]

```

預設會移除目前最新的版本。

亦可以使用 "-v" 參數來指定移除特定版本，如下：

```
$ gem uninstall -v 1.7.2 [套件名稱]

```

#### 05\. 清除舊版本的 gems

在管理 gem 的安裝與更新過程中，可能會保有舊版本的 gems，如果想要清除舊版本，而僅留下最新版本，請於命令列模式下輸入下列指令：

```
$ gem cleanup [套件名稱]

```

如果想要清除所有已安裝的舊版 gems，則使用 `cleanup` 參數即可，如下：

```
$ gem cleanup

```

但不是所有套件都可以藉此完整清除。比方說如果我們升級 rails，並刪除舊版 rails，單單執行 `gem cleanup rails` 是不夠的。

因為實際上 rails 比較接近 metagem，意思就是這個 gem 本身是依賴一群彼此相依的 gems，也就是包括 activerecord、activesupport、actionpack、actionmailer 等套件。因此，假設想清除 rails 3.0.5，請於命令列模式下輸入下列指令：

```
$ gem list '^active|^action|^rails' | sed -E 's/\(.+\)//' | xargs gem uninstall -v 3.0.5

```

#### 06\. RubyGems 本身的更新

請於命令列模式下輸入下列指令：

```
$ gem update --system

```

就可以自動升到最新的 RubyGems。不過有時候會有點問題，此時需要再執行：

```
$ update_rubygems

```

降級的方法也很簡單，只需要指定版本即可。假設我們想要安裝 1.7.2，則先透過 RubyGems 本身安裝 RubyGems 1.7.2：

```
$ gem install -v 1.7.2 rubygems-update

```

再用 1.7.2 的 update_rubygems 來更新：

```
$ update_rubygems _1.7.2_

```

`_1.7.2_` 用於指定特定版本。假設系統中同時有 Rails 3.1.1 和 Rails 2.3.14，可以藉由 `rails _2.3.14_` 來指定 2.3.14。

### 版本管理

除了前文提到的下列指定特定版本的管理：

```
$ gem instsall -v 1.7.2 [套件名稱]
$ gem uninstall -v 1.7.2 [套件名稱]

```

其實也可以指定一個範圍的版本。比方說如果需要安裝「最新的」Rails 2 的話，可以執行：

```
$ gem install -v '<3' rails

```

這裡 `<3` 的意思是任何版本編號小於 3 的版本。既然有小於運算子，就會有大於運算子，也同樣有小於等於運算子： `<=` 和大於等於運算子： `<=`。除此之外，還有一個比較不尋常，可能只有 RubyGems 在用的 `~>` 運算子。目前這運算子沒有一個正式的名稱，應該用什麼名字還在[討論當中](https://github.com/rubygems/rubygems/pull/124)。此運算子的意思是不在乎最小位數的數字。例如寫作 `~>3.0` 表示可以接受任何 3.x 版本。因此實際上也等價於 `~>3.5`。用大於小於來表示的話，則是 _3 <= the version < 4_。

使用這樣的運算子有兩個理由。一個是比寫一個範圍要來得簡單，另一個是明確表示自己想要的版本究竟是什麼，但是差一點點無所謂！也因此有人提議這個運算子應該叫 approximate operator，也就是近似運算子。

除了用 `gem` 命令管理 gems 以外，很重要的另一點是如何在 ruby runtime 裡選擇各種 gems 的版本。這裡同樣可以使用各種版本的運算子。像是：

```
gem 'rake', '~>0.8.7'
require 'rake'

```

在第二行的 `require 'rake'` 會根據上次指定的版本限制，也就是第一行，來選擇應該讀入的 rake 版本。另外也可以用等於或是完全不寫運算子：

```
gem 'rake', '=0.9.2' # 等價於下行：
gem 'rake',  '0.9.2'

```

不過由於 Rails 的版本控制有點混亂且複雜，往往還會有其他版本問題。因此社群也特地開發了 [Bundler](https://github.com/carlhuda/bundler) 來協助 Rails 3 管理這一團混亂。由於這超出本篇範圍，在此就不多討論。

### 推薦的 Gems 執行環境的設定

RubyGems 可藉由設定 ".gemrc"（依據使用者名稱的不同，預設存放於使用者的家目錄下）來調整執行時的行為。一個常見的用法是新增下列內容，來略過 rdoc 及 ri 的安裝：

```
gem: --no-rdoc --no-ri

```

如果安裝 gems 時還要產生 rdoc 和 ri，是很花時間的。若沒有使用 rdoc 或 ri 的習慣，建議把該選項新增至 ".gemrc" 中。

而我個人的 ".gemrc" 還多設了兩個選項如下：

```
gem: --user --env-shebang --no-rdoc --no-ri

```

其中第一個 `--user` 的意思是優先使用屬於使用者個人的 gems，而非系統的。也因此，我在執行 `gem install` 時不需要 `sudo`，因為 gems 被安裝至 ~/.gem 底下，而非系統中。我個人比較喜歡用這種方式管理我安裝的 gems，這樣就不會受到系統的影響，自己要修改 gems 來除錯時，也不會影響到其他人。

第二個 `--env-shebang`，則是 假如 rake 這個 gem 有個 `rake` 的命令，那這個程式本身是一個 Ruby 程式，因此也會有個 shebang。預設的行為下，這 shebang 會指向當時所使用的 Ruby 完整路徑。問題是，Mac 的 [Homebrew](https://github.com/mxcl/homebrew) 在每次更新 Ruby 時，完整路徑都會改變，因為路徑本身含有版本資訊。若不希望每次更新 Ruby 都要重新更新 shebang，不妨就直接用 `#!/usr/bin/env ruby` 吧！

除此之外，還有非常多不同的設定。

### 簡單易用下的複雜性

我沒用過很多套件管理系統，但 RubyGems 相較於其他用過的軟體，可以說是最方便、簡單且功能強大。不過當然沒有絕對完美的軟體。RubyGems 付出的代價是極為複雜的內部運作，使得執行效率變得不甚理想。

如果在執行每個 Ruby 程式之前先讀入 RubyGems，例如在程式的最前面寫上 `require 'rubygems'`，則程式的啟動效能會讓很多人無法接受。因此很多人開發其他取代 RubyGems 的軟體，例如 [rip](https://github.com/defunkt/rip) 和 [coral](https://github.com/mislav/coral) 等，將這些工作簡化，僅調整 `$LOAD_PATH` 而非像 RubyGems 還會處理版本相依的問體。

Ruby 1.9 在把 RubyGems 納入核心時，甚至寫了一個 `gem_prelude`，是輕便型的 RubyGems，理由是很多 Ruby 核心的開發者不願意付出啟動 RubyGems 所需的運算成本。

不幸的是，正因為 `gem_prelude` 不是完整的 RubyGems 實作，不會計算複雜的版本相依問題，因此在某些情況下，會讀到非指定版本的 gems。如果我們只用最新的 gems，或是電腦裡只安裝所需的 gems 時，不會出現問題。但如果安裝許多版本，又指定了特定版本，`gem_prelude` 就有可能忽略一些細節。

這個問題在 Rails 上特別嚴重，因為使用到各套件的底層，使得 Rails 對於套件的版本相當敏感，有時版本只差一點點就有問題。這在 ruby-core mailing list 裡討論很久，一方認為正確性比較重要，而另一方則認為啟動速度比較重要。後來 RubyGems 經過幾次瘦身和最佳化，總算啟動得夠快了，核心開發者才同意拿掉 `gem_prelude`。因此現在已經不會有這個問題。

### 展望

如果問 Ruby 裡哪一個套件是最重要、最具有影響力的，我想一定就是 RubyGems 吧。有了套件管理系統，才有方便的散佈方式，也才有方便使用各種套件的方法。而在 scripting 的世界裡，方便幾乎可說是最重要的一件事。不是說沒有 RubyGems 就不會有今天的 Ruby，如果真的沒有，那一定會有誰發明出類似的東西。但一個恰恰堪用的套件管理系統，跟一個方便強大的套件管理系統，作用不可同日而語。

有些 Ruby 核心開發者，甚至[希望能把標準程式庫](http://redmine.ruby-lang.org/projects/ruby/wiki/StdlibGem)， 改放到 RubyGems 裡面。這不代表這些標準程式庫不受青睞了，因為它們仍然會被預先安裝到標準 Ruby 發行版裡面。重點在於，這樣一來這些標準程式庫就可以不受限於 Ruby 本身的發行狀況，需要修正什麼錯誤時，可以自行推出修正檔，而非請大家等候下一版本的 Ruby。事實上，rake 在 Ruby 1.9 中，正是這個狀況，安裝好 Ruby 後就可以注意到 rake 已經被預先安裝好了，使用者也可以自行透過 `gem` 移除或是升級。希望之後有更多的標準程式庫可以用這種方式呈現。

當然也是有反對意見，因為這很可能會使得版本變得更混亂，比方說如果 net/http 在 Ruby 2.0 變成了一個標準 gem，那開發者是否可以假設一台跑著 Ruby 2.0 的電腦有沒有安裝 net/http？會不會被使用者移除掉了？會不會被使用者升級到一個版本差異過大的版本？

不過個人覺得這個問題是可以被接受的。如果版本差異真的這麼重要，那麼或許應該使用 bundler 更嚴格地管理版本，而不是仰賴一個根本不知道是什麼版本的標準程式庫。

礙於篇幅，這篇只稍微談了一小部份的 RubyGems，也沒有提到如何製作自己的 gems。儘管如此，也希望這篇有成功讓大家更了解 RubyGems。希望大家都能開開心心地用 RubyGems 完成自己的工作！
