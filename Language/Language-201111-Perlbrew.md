# Perlbrew - perl installation management too
作者：林佑安 (c9s)，2011 年 11 月投稿。

### 簡介

2010 年二月，台灣的 CPAN 模組開發者─劉康民 (gugod) 釋出了 App::perlbrew 模組，該模組以 Ruby 的 rvm 概念出發，意即使用者可以利用 Perlbrew 將不同版本的 Perl 安裝在 $HOME 路徑內，並且任意切換不同版本的 Perl。

使用 Perlbrew 有幾個優點：

*   不需使用 sudo 來安裝 CPAN 模組
*   可以使用每個月更新的 Perl
*   可嘗試新的程式語言功能 可以不受 Vendor Perl 限制（平台內建的 Perl）
*   可在不同版本的 Perl 下測試模組
*   可整合至 bash、zsh、csh 環境變數

### 安裝

將下面這行 Shell 指令貼入終端機內執行：

```
$ curl -L http://xrl.us/perlbrewinstall | bash

```

或者由 CPAN Shell 安裝：

```
$ sudo cpan App::perlbrew
$ perlbrew init

```

安裝完畢後，預設的 Perlbrew 根目錄會在 ~/perl5/perlbrew（使用者的家目錄）下。

並且將以下指令加到 SHELL 的環境設定中，如 bashrc、cshrc 或 zshrc：

```
# for bash / zsh
source ~/perl5/perlbrew/etc/bashrc

# for csh
source ~/perl5/perlbrew/etc/cshrc

```

### 使用範例

#### 01\. 列出支援的版本列表

請於命令列模式下輸入下列指令：

```
$ perlbrew available

```

輸出範例為：

```
perl-5.15.3
perl-5.14.2
perl-5.12.4
perl-5.10.1
perl-5.8.9
perl-5.6.2
perl5.005_04
perl5.004_05
perl5.003_07

```

#### 02\. 安裝指定的版本

從支援列表中選擇欲安裝的版本，例如 5.14.2。於命令列模式下輸入下列指令：

```
$ perlbrew install 5.14.2

```

輸出範例為：

```
Fetching perl-5.14.2 as /Users/c9s/perl5/perlbrew/dists/perl-5.12.4.tar.gz
Installing /Users/c9s/perl5/perlbrew/build/perl-5.14.2 into ~/perl5/perlbrew/perls/perl-5.14.2
This could take a while. You can run the following command on another shell to track the status:
    tail -f ~/perl5/perlbrew/build.log

```

如果想了解安裝進度，可以使用上面指示的 tail 指令來查看。

#### 03\. 列出目前安裝的版本

請於命令列模式下輸入下列指令：

```
$ perlbrew list

```

#### 04\. 切換至指定的版本

接著，可於命令列模式下輸入下列指令，以切換至 5.14.2 版本：

```
$ perlbrew switch perl-5.14.2

```

並確認目前的版本：

```
$ perl -v
This is perl 5, version 14, subversion 2 (v5.14.2) built for darwin-2level

```

#### 05\. 切換回系統預設的版本

請於命令列模式下輸入下列指令：

```
$ perlbrew off

```

如此會將 perlbrew 關掉。

### 其它相關擴展

#### 01\. cpanminus

cpanminus 是由日本 @miyagawa (bulknews.typepad.com) 所開發的一個極小、不需設定、無相依性、快速的 CPAN 模組安裝工具。

如果你喜歡 cpanminus，可使用下列指令安裝 perlbrew 提供的 cpanm：

```
$ perlbrew install-cpanm

```

該 cpanm 可將模組安裝至目前使用的 Perl 版本函式庫內。

可以透過 `which` 命令觀察現在所使用的 cpanm：

```
$ which cpanm
/Users/c9s/perl5/perlbrew/bin/cpanm

```

透過該 cpanm 安裝模組，是不需要 `sudo` 的：

```
$ cpanm Moose

```

使用該 cpanm 安裝模組，則會將模組安裝到目前使用版本的 Perl 函式庫內，這些函式庫放置在 ~/perl5/perlbrew/perls 底下：

```
% ls -l ~/perl5/perlbrew/perls
drwxr-xr-x  5 c9s  staff  170  9 25 13:12 perl-5.14.1
drwxr-xr-x  6 c9s  staff  204 10  7 11:27 perl-5.14.2-llvm
drwxr-xr-x  5 c9s  staff  170  9 25 02:33 perl-5.15.3

```

若要使用 tree 命令觀察路徑結構，則如下：

```
$ tree ~/perl5/perlbrew/perls/perl-5.14.1/lib | head
/Users/c9s/perl5/perlbrew/perls/perl-5.14.1/lib
├── 5.14.1
│   ├── AnyDBM_File.pm
│   ├── App
│   │   ├── Cpan.pm
│   │   ├── Prove
│   │   │   ├── State
│   │   │   │   ├── Result
│   │   │   │   │   └── Test.pm
│   │   │   │   └── Result.pm

```

你也可以使用 perl -V 查看目前使用的 @INC（函式庫搜尋路徑）

```
$ perl -V
Summary of my perl5 (revision 5 version 14 subversion 2) configuration:
... 略 ...
Built under darwin
Compiled at Oct  4 2011 13:56:16
%ENV:
    PERLBREW_HOME="/Users/c9s/.perlbrew"
    PERLBREW_PATH="/Users/c9s/perl5/perlbrew/bin:/Users/c9s/perl5/perlbrew/perls/perl-5.14.2-llvm/bin"
    PERLBREW_PERL="perl-5.14.2-llvm"
    PERLBREW_ROOT="/Users/c9s/perl5/perlbrew"
    PERLBREW_VERSION="0.29"
    PERLDOC="-otext"
    PERL_MM_USE_DEFAULT="1"
@INC:
    /Users/c9s/perl5/perlbrew/perls/perl-5.14.2-llvm/lib/site_perl/5.14.2/darwin-2level
    /Users/c9s/perl5/perlbrew/perls/perl-5.14.2-llvm/lib/site_perl/5.14.2
    /Users/c9s/perl5/perlbrew/perls/perl-5.14.2-llvm/lib/5.14.2/darwin-2level
    /Users/c9s/perl5/perlbrew/perls/perl-5.14.2-llvm/lib/5.14.2

```

Perl 相關的環境變數也會列舉出來。

#### 02\. local::lib

local::lib 模組是用來讓你將所有模組安裝至某特定路徑下的工具，因此，利用 local::lib ，你可以在不需要 Root permission (sudo) 的情況下，安裝模組至某一目錄供你的 Perl 使用。

local::lib 的範例如下：

```
  # Install LWP and its missing dependencies to the '~/perl5' directory
  perl -MCPAN -Mlocal::lib -e 'CPAN::install(LWP)'

```

以上可安裝 LWP 模組至 ~/perl5 目錄下。

你也可將 local::lib 環境變數列印出來：

```
  $ perl -Mlocal::lib
  export PERL_MB_OPT='--install_base /home/username/perl5'
  export PERL_MM_OPT='INSTALL_BASE=/home/username/perl5'
  export PERL5LIB='/home/username/perl5/lib/perl5/i386-linux:/home/username/perl5/lib/perl5'
  export PATH="/home/username/perl5/bin:$PATH"

```

在使用 Perlbrew 的情況下，如果你在不同版本的 Perl 中使用同一個 local::lib 路徑，很可能會遇到編譯的二進位檔案不相容的問題。

也因此，Perlbrew 提供了新功能─`lib`，在不同版本的 Perl 中，你可以建立出獨立的 local::lib 空間，而不受到影響：

```
$ perlbrew lib create nobita

```

以上指令可在目前版本的 Perl 中，建立一個名為 nobita 的 local::lib 函式庫空間。

若要指令版本建立 local::lib 函式庫空間，也可執行以下指令：

```
$ perlbrew lib create perl-5.12.3@shizuka

```

若要列出所有的 local::lib 空間：

```
$ perlbrew lib list

```

若要切換使用的 local::lib 空間：

```
$ perlbrew lib use nobita

```

如此一來，你可以利用 cpanm 將模組安裝至不同的 local::lib 函式庫空間內，在不同專案中，很可能會使用到不同版本、不同相依性的模組，你便可以利用這些功能來測試專案、模組之間的相容性。

#### 03\. Perl Delta

關於 Perl 版本的變動，可以使用 perldoc 查閱相關資訊：

```
$ perldoc perl

```

可查閱 perl 文件的索引，如以下這些文件項目，便包含了該版本修改、新增的地方：

```
perldelta           Perl changes since previous version
perl5141delta       Perl changes in version 5.14.1
perl5140delta       Perl changes in version 5.14.0
perl51311delta      Perl changes in version 5.13.11
perl51310delta      Perl changes in version 5.13.10
perl5139delta       Perl changes in version 5.13.9

```

欲查閱項目，可下以下指令：

```
perldoc perl5140delta

```

或者可在 [Meta CPAN](https://metacpan.org/module/perl) 或 [CPAN Search](http://search.cpan.org/dist/perl/pod/perl5141delta.pod) 上找到。

### 結論

由於近年來 Perl 版本釋出快速，Perlbrew 可讓使用者及早使用新版本的 Perl，這些都帶動了 Perl 程式語言以及社群蓬勃發展。

開發者可以使用不同版本的 Perl 來確保模組的運行，也可使用較舊版的 Perl 來為模組除錯，確保 Perl5 中重要的向後相容；也可為了改善效能安裝較新的 Perl。

因此，Perlbrew 可稱為現代 Perl 工具鏈中的一大利器。

### 參考

perldoc local::lib

perldoc App::perlbrew

Perl source: porting/release_manager_guide.pod

[perl5.12.0](http://www.slideshare.net/obrajesse/perl-5120)

[perl5.16 and beyond](http://www.slideshare.net/obrajesse/perl-516-and-beyond)

[Modern Perl Toolchain](http://www.slideshare.net/alex.muntada/modern-perl-toolchain)

[Perlbrew.pl](http://www.perlbrew.pl/)

[Perlbrew YAPC Asia 2010](http://www.slideshare.net/gugod/perlbrew-yapcasia2010talk)

[Perlbrew development and the git flow](http://gugod.org/2011/09/perlbrew-development-and-git-flow/)