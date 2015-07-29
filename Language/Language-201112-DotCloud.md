# 以 DotCloud 部署 Perl Web Ap
作者：Gugod，2011 年 12 月投稿。

### 前言

[DotCloud](http://dotcloud.com) 是個很新的 PaaS 供應商，與之前的 PaaS 系統相比，這一家所提供的平台其資料庫以及程式語言十分豐富，目前的[支援列表](http://docs.dotcloud.com/services/roadmap)包括了 Java、Perl、Python、Ruby、PHP、Node.JS、MySQL、PostgreSQL、Redis、MongoDB，目前的主流組合都可以滿足。

在 DotCloud 系統中，使用者需替 App 定義所需的服務節點（在 DotCloud 網站中，稱為 "Stack" 或 "Service"，因 Stack 意義為「堆疊」，容易造成混淆，在本中以一般分散式計算所用的名詞「節點」或「服務節點」代稱之）。例如，一個需要 Perl 與 MySQL 組合的 App，就需要建立兩個服務節點，其一負責執行 Perl 程式，其二則運行 MySQL。爾後如需擴增規模，可視實際負載情況選擇增加 Perl 節點或是 MySQL 節點。若需要替 App 增加頁面快取，可選擇增加 Static 節點（只存靜態檔案），並配合 Perl Worker 節點在背景產生靜態頁面。

作為替 Web App 特別打造的服務平台，DotCloud 獨特的彈性可見一斑。

### 準備開始

到 [DotClout](http://dotcloud.com) 申請免費帳號之後，便可以開啟 2 個節點，足夠一般試用評估。申請完畢後，需到[設定頁](https://www.dotcloud.com/accounts/settings)取得 API Key，會在之後的步驟中用到。

所有的節點控制，是透過一個叫做 `dotcloud` 的命令列指令進行，這個指令是個 python 程式，需要透過 pip 來安裝，因此系統中必須先安裝 python，之後需用 `pip` 來裝 dotcloud：

```
sudo easy_install pip && sudo pip install dotcloud

```

安裝後直接執行，會要求輸入 API Key。

```
$ dotcloud
Enter your api key (You can find it at http://www.dotcloud.com/account/settings):

```

此後執行便不必再度設定 API Key，設定檔會存在 `~/.dotcloud` 目錄中。

`dotcloud` 這個指令有許多小命令，比較常用的有：

```
list      列出目前開了哪些服務節點
push      上傳最新程式碼
rollback  回復到前一版
alias     設定網址別名
logs      讀 Server 端最新的 HTTP log
info      顯示 App 服務節點基本資料
status    顯示 App 服務節點目前上線狀態
ssh       登入服務節點

```

每個命令都可加上 `-h` 參數，會顯示詳細說明。執行 `dotcloud -h` 也會有完整的命令列表。

### 建立靜態頁面 Web App

先看看如何建立一個單純提供檔案的靜態 App，在 DotCloud 文件中，這稱為「Static」。

首先要先向 DotCloud 系統建立 App，假設專案名叫做 `helloapp`，就執行以下指令：

```
dotcloud create helloapp

```

接著要替 App 開個專案目錄放置網頁靜態檔案，假設名為 `"hello"`，並在此目錄中建立名為 `dotcloud.yml` 的設定檔案，列出如下：

```
hello/
hello/dotcloud.yml
hello/index.html

```

其中 `dotcloud.yml` 的內容為：

```
www:
  type: static

```

這個檔案的格式是 [YAML](http://yaml.org)，其目的是定義：本 Hello App 需要一個名為「www」的節點，所需的服務型別為「static」，所需的檔案都放在本目錄中（就是 `dotcloud.yml` 的所在目錄）。

假設 `index.html` 的內容為（就是 HTML Hello World 範例）：

```

    Hello World!

```

到此已經準備完畢，接著就是把這個 App 上線了，執行：

```
cd hello/ # （必需在 `dotcloud.yml` 所在目錄執行）
dotcloud push helloapp

```

這樣就行了，`dotcloud` 指令會自動利用 `rsync` 或 `git` 上傳整個 `hello` 資料夾，並且啟動伺服器，自動產生專用的網址。

### 建立 Perl Web App

要建立 Perl Web App，需以 [PSGI](https://metacpan.org/module/PSGI) 規格實做，不是 CGI。以 Hello World 為例的話，就算不用任何 MVC Web Framework，所需要的程式碼也相當簡單。在專案目錄中需要準備以下兩個檔案：

```
hello/app.psgi
hello/dotcloud.yml

```

`app.psgi` 內容為

```
sub {
    return [
        '200',
        [ 'Content-Type' => 'text/html ],
        [ "Hello World!" ]
    ];
}

```

`dotcloud.yml` 內容為

```
app:
  type: perl

```

同樣的，做完之後執行 `push` 命令就可以將 App 上線了：

```
cd hello/
dotcloud push helloapp

```

為了團隊合作方便，我們終究會需要使用某個 Web Framework，時至今日，Perl 社群中大部份的 Web Framework 都支援 PSGI。[Dancer](http://perldancer.org/) 或 [Mojoliciou](http://mojolicio.us/) 都是很新穎又容易上手的選擇。

### 替 Perl Web App 加上 MySQL

實際的應用中，多半會使用某種資料庫來儲存各種資料，以下以 MySQL 為例，簡介如何使用 DotCloud 所提供的 MySQL 服務節點，用 Perl 與 MySQL 實做出一個簡易留言版，並部署到 DotCloud。使用 [Dancer](http://perldancer.org/) 作為範例。

Dancer 可以透過 CPAN 來安裝：

```
cpan -i Dancer

```

完畢之後，先以 dancer 指令建立留言版 App 專案，名稱定為 EasyBoard：

```
$ dancer -a EasyBoard

```

Dancer 會開好目錄，並自動生成基本架構所需的程式碼以及預設的設定。執行以下命令，就可以啟動本機開發用的 App 伺服器，位於 `http://localhost:3000`：

```
cd EasyBoard; bin/app.pl

```

接下來，在專案目錄中寫好 `dotcloud.yml`，這次有兩個節點：

```
# EasyBoard/dotcloud.yml
app:
  type: perl
db:
  type: mysql

```

DotCloud 的 Perl 節點，需要使用者提供 `app.psgi` 這個檔案，Dancer 預設並沒有生成這個檔案，但也很容易解決：

```
echo "require 'bin/app.pl';" > app.psgi

```

此外要修改一下 `Makefile.PL`，把 `Plack` 加入 `PREREQ_PM` 當中：

```
PREREQ_PM => {
    'Test::More' => 0,
    'YAML'       => 0,
    'Dancer'     => 1.3080,
    'Plack'      => 0.9985
},

```

雖然還沒有寫真正的程式碼，但到此就算基本準備完成了，可以立刻將 App 部署到 DotCloud。首先要建立 App（dotcloud 限制 App 名稱中的英文全要用小寫）：

```
dotcloud create easyboard

```

然後上傳程式碼：

```
dotcloud push easyboard

```

在上傳的過程中，可以看見訊息顯示在 `app.0` 這個節點有安裝 CPAN 模組的過程，首次執行的話，可能會花上一小段時間，但通常不會太久。等做完後，會得到類似以下訊息報告 App 的網址：

```
Deployment finished. Your application is available at the following URLs
app: http://easyboard-gugod.dotcloud.com/

```

這時可以先用瀏覽器打開此網址確認一下畫面。看到 Dancer 預設的歡迎畫面的話，就表示成功了。

在 Perl 的許多 Web Framework 中，都不包含「Model」的部份，因為早在 MVC Web Framework 風行之前，CPAN 上就已經有為數眾多的 ORM 系模組，Web Programmer 只要選用就可以了，因此多數的Perl Web Framework 並不特別再多做新的 ORM。

#### 熟悉環境

dotcloud 有指令可以讓開發者取得 mysql 節點的一些資訊，執行：

```
dotcloud info easyboard.db

```

會輸出如下：

```
>  dotcloud info easyboard.db
config:
    mysql_masterslave: true
    mysql_password: E6oPYcv96CQbsbUY1GSK
created_at: 1323387548.1464889
datacenter: Amazon-us-east-1d
image_version: 1120eda9aa82 (latest)
instances:
    easyboard.db.0:
        role: master
        state: up
ports:
-   name: ssh
    url: ssh://
 mysql@easyboard-gugod.dotcloud.com :19668
-   name: mysql
    url: mysql://root:
 password@easyboard-gugod.dotcloud.com :19667
type: mysql

```

其中可以看到 `config` 中有 `mysql_password` 這一項，事實上，也可以直接連進 mysql console：

```
dotcloud run easyboard.db -- mysql -uroot -ppassword

```

輸出如下：

```
>  dotcloud run easyboard.db -- mysql -uroot -ppassword
# mysql -uroot -ppassword
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 34
Server version: 5.1.41-3ubuntu12.10-log (Ubuntu)

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>

```

基本上在程式中只要使用這裡提供的 mysql 主機、帳號密碼資訊就可以了，但是，沒有人會希望把帳號密碼直接用字串寫在程式碼裡，這樣是很不好的。DotCloud 平台對此提供的解決辨法，是將各節點設定值寫在 `/home/dotcloud/environment.json` 這個檔案中。開發人員必須修改一部份程式，檢查這個檔案是否存在，並解出其中變數的值。

要快速條列出所有變數的方式，請執行下列指令：

```
dotcloud var list easyboard

```

輸出如下：

```
>  dotcloud var list easyboard
DOTCLOUD_APP_SSH_HOST=easyboard-gugod.dotcloud.com
DOTCLOUD_APP_HTTP_HOST=easyboard-gugod.dotcloud.com
DOTCLOUD_ENVIRONMENT=default
DOTCLOUD_DB_SSH_HOST=easyboard-gugod.dotcloud.com
DOTCLOUD_APP_HTTP_URL=http://easyboard-gugod.dotcloud.com/
DOTCLOUD_DB_MYSQL_URL=mysql://root:
 password@easyboard-gugod.dotcloud.com :19667
DOTCLOUD_APP_SSH_PORT=1282
DOTCLOUD_DB_MYSQL_LOGIN=root
DOTCLOUD_DB_MYSQL_PASSWORD=password
DOTCLOUD_PROJECT=easyboard
DOTCLOUD_DB_MYSQL_PORT=19667
DOTCLOUD_DB_MYSQL_HOST=easyboard-gugod.dotcloud.com
DOTCLOUD_DB_SSH_URL=ssh://
 mysql@easyboard-gugod.dotcloud.com :19668
DOTCLOUD_DB_SSH_PORT=19668
DOTCLOUD_APP_SSH_URL=ssh://
 dotcloud@easyboard-gugod.dotcloud.com :1282

```

主要會用到的，是 `DOTCLOUD_DB_MYSQL_HOST`、`DOTCLOUD_DB_MYSQL_POST`、`DOTCLOUD_DB_MYSQL_LOGIN`、`DOTCLOUD_DB_MYSQL_PASSWORD` 這四項。注意，DotCloud 並沒有自動替專案建出 App 所要用的資料庫，它只是先開好 MySQL 伺服器而已。而事實上，App 大可以在此伺服器上使用複數個資料庫。

如果真有必要，每個節點都可以 ssh 進去管理，執行：

```
 dotcloud ssh thegoodapp.data

```

就可以連進 thegoodapp 這個 app 之中名為 "data" 的節點。以本文的 easyboard app 為例，可以連進 "easyboard.app" 與 "easyboard.db" 兩個節點：

```
 dotcloud ssh easyboard.app
 dotcloud ssh easyboard.db

```

基本上，每個服務節點，都可視為一台獨立的主機，其中執行著自動設定好的伺服器。開發人員只要用一個設定檔，便可以指定要使用幾個節點，以及每個節點上要執行何種項目。徹底省下安裝時間，以及設定的麻煩。

#### 程式主體

在了解環境之後，寫完程式自然就是時間問題了，在本例中刻意不使用 ORM，直接透過 DBI 與 SQL 來說明，ORM 的選擇留給讀者自行決定。為了不讓程式碼佔去太多篇幅，完整的範例程式可在以下網址取得，本文中只針對重點部份說明，請讀者對照本文與程式碼，便能理解：

```
https://github.com/gugod/EasyBoard

```

首先將資料庫準備好，先連到 mysql 建立好所需的資料庫與 Table：

```
dotcloud run easyboard.db -- mysql -uroot -ppassword

```

連進 mysql console 之後，輸入以下的 SQL 指令來建立資料庫與 Table：

```
set NAMES UTF8;
create database easyboard;
create table if not exists entries (
    id integer primary key auto_increment,
    name varchar(255) not null default 'Someone',
    body text not null
) DEFAULT CHARACTER SET = utf8 COLLATE = utf8_bin;

```

在 `lib/EasyBoard.pm` 開頭的部份可看見如下的程式片段，主要是在讀取 DotCloud 提供的設定值。

```
my $DOTCLOUD_ENV = undef;
my $DOTCLOUD_ENV_FILE = "/home/dotcloud/environment.json";

if (-f $DOTCLOUD_ENV_FILE) {
    local $/ = undef;
    open(my $fh, "<", $DOTCLOUD_ENV_FILE) or die $!;
    my $json = ;
    $DOTCLOUD_ENV = JSON::decode_json($json);
    close($fh);
}

```

如果 DotCloud 的設定檔案存在，就會在程式啟動時被讀取。接下來便可以利用其中的設定值連接至資料庫。在其之後定義的 `database` 函式，會連到 mysql 並傳回 DBI Handle，如果在程式的 process 中已經連接上，便會重複使用這個DBI Handle 而不會重連。如果發現程式是處於 DotCloud 節點上，就會使用 DotCloud 的設定值，否則就連到本機的 mysql（是為開發模式）。

```
my $dbh;
sub database {
    return $dbh if $dbh;

    my $dsn = "DBI:mysql:database=easyboard";
    my ($user, $pass) = ("root", "");

    if ($DOTCLOUD_ENV) {
        $dsn .= ";host=" . $DOTCLOUD_ENV->{DOTCLOUD_DB_MYSQL_HOST};
        $dsn .= ";port=" . $DOTCLOUD_ENV->{DOTCLOUD_DB_MYSQL_PORT};
        $user = $DOTCLOUD_ENV->{DOTCLOUD_DB_MYSQL_LOGIN};
        $pass = $DOTCLOUD_ENV->{DOTCLOUD_DB_MYSQL_PASSWORD};
    }

    $dbh = DBI->connect($dsn, $user, $pass, { mysql_auto_reconnect => 1, mysql_enable_utf8 => 1 })
        or die "Fail to connect to db: $!";

    $dbh->do("SET NAMES UTF8");
    return $dbh;
}

database(); # connect to database on startup

```

Controller 與 View 的部份，就是去資料庫中取出留言板內容，與 DotCloud 無關，因此不多著墨。

在 App 中使用到的 CPAN 模組，需在 `Makefile.PL` 中列舉。另外，也可以在 `dotcloud.yml` 中列出所需要的其他模組，範例如下：

```
app:
  type: perl
  requirements:
    - Template::Toolkit
    - JSON
    - Dancer::Session::Cookie
    - http://example.com/something/not/on/cpan/yet.tar.gz

```

這部份的設定是 DotCloud 專用，不過可以指定模組網址，如果需要安裝不在 CPAN 上的模組，便可利用此方法。

所有程式準備完畢之後，執行 `dotcloud push easyboard` 便會將程式碼部署完畢。

### 結尾

DotCloud 是今年才開始營運的全新 PaaS 廠商，但到目前為止，其產品的便利性及完整度都令人十分讚嘆，所提供的說明文件也足夠一般開發人員參考。雖然目前提供的服務節點種類仍限於主流使用的幾 種，但如 Erlang、CouchDB、ElasticSearch 等節點也將逐漸完成。對於只是想要試用、評估特定服務的開發人員來說，可以直接省去安裝伺服器軟體的時間。