# 麥克阿忠的 Ruby on Rails 真功夫─手刻 CRUD
作者：麥克阿忠，2011 年 10 月投稿。

### 前言

[上篇](http://www.openfoundry.org/tw/tech-column/8440-ruby-on-rails-scaffold-)我 們介紹 Ruby on Rails 的快速上手方式，使用 scaffold 指令開發出完整的資料庫基礎操作的應用介面。但針對實際上的需求，仍需根據實務再作修正修正，為了使初學者能夠從中理解 Ruby on Rails 的運作過程，以打下 Ruby 的功夫底子，本篇特別示範真功夫教學。

### 什麼是 CRUD ？

CRUD 意指在資料庫的四個資料基本異動動作，分別為：

*   Create：建立資料
*   Read：讀取資料
*   Update：更新資料
*   Destory：刪除資料

一般而言，傳統資料庫的組成是各資料列結合成資料表，再由資料表集合成資料庫。如同手上的電話簿，欲加上朋友的聯絡人，就必需 Create（建立資料），若要打電話讀取電話號碼則 Read（讀取資料），若某人的電話更新了就要 Update（更新資料），而不再聯絡者就 Destory（刪除資料）。設計資料庫應用程式介面原則上不離這四個基礎操作。設計一項表單就要開發出建立、檢視、更新、移除。

所以，有經驗的程式設計師會花許多時間建立屬於自己的 MVC，或是使用現成流行的 MVC Framework 以簡化開發的流程。如上述的狀況，網頁表單的設計可以簡化為類似 Partial 的方式以重複利用。

### 準備

在實作手刻 CRUD 前，請先確定目前的環境。例如檢查 Ruby 的版本，

```
$ ruby -vruby 1.8.7 (2011-02-18 patchlevel 334) [i686-linux], MBARI 0x8770, Ruby Enterprise Edition 2011.03
```

以及檢查 Ruby on Rails 的版本，

```
$ rails -vRails 3.0.7
```

還有 Rake 的版本，

```
$ rake -Vrake, version 0.8.7
```

### 實作

#### 規畫資料庫

在前兩篇系列文章中，我們提到 Ruby on Rails 中的 new 用法，現在我們需要再建立一個新的專案來實作這次的手刻 CRUD 的內容。在本例中我們將以「電話簿」的實例來建立一個 Web 應用程式，首先我們必須規劃資料庫的結構，然後一步步建立資料庫介面所需的 Ruby 程式碼。

[![圖1](http://www.openfoundry.org/images/111018/RailsIII/rorcrud01.png "電話簿範例的資料庫結構")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud01.png)  

▲ 圖1：電話簿範例的資料庫結構

如上圖，規劃出的欄位有 id、name、phone、birthday 等四個欄位，而型態皆是平常使用到的整數型態(INT)，文字型態 (TEXT) 以及日期型態 (DATE)。MySQL 的使用者可參考此表中的長度欄位，若是使用 SQLite3 則可忽略。

#### 建立新專案

輸入下列指令以建立本篇所需的檔案：

```
$ rails new phonebook
```

然後進入工作目錄開始上工囉，

```
$ cd phonebook
```

註：如果您升級至 rails 3.1.0，會遇到 Could not find a JavaScript runtime 錯誤，解決方法是到該目錄下的 Gemfile 檔加入二行：

```
gem 'execjs'gem 'therubyracer'
```

然後執行下列指令即可，

```
$ bundle install
```

#### 資料庫與 Ruby on Rails 的連結

應用程式的資料庫函式可以透過 Database adapter 介面來連結不同的資料庫，不過前提是該程式語言有提供這樣的連結方式。如同 PHP 除了會搭配 MySQL 之外也提供 ODBC 函式庫連結 MS SQL。

在 Ruby on Rails 中預設的資料庫是 SQLite3，因此不用特別連結其它資料庫。如果偏好 MySQL，可在建立新專案時下指令：

```
$ rails new myproject -d mysql
```

"-d" 的參數就是指定特定的資料庫，目前 Rails 可指定的資料庫選項有：mysql / oracle / postgresql / sqlite3 / frontbase / ibm_db / jdbcmysql / jdbcsqlite3 / jdbcpostgresql / jdbc。

如果有特別的資料庫則必須先以下列指令查詢是否已安裝，

```
$ gem list*** LOCAL GEMS ***......jquery-rails (1.0.14)json (1.6.1)............mysql (2.8.1)mysql2 (0.3.7, 0.3.6, 0.2.13, 0.2.7)............rack (1.3.3, 1.3.2, 1.2.3)......
```

只要看到有 mysql 字眼，就表示你的 Ruby on Rails 有支援，若沒看到，可下指令安裝，

```
$ gem install mysql2
```

不過在一般 Linux 套裝作業系統環境下，有時不只需要上述指令，還需另外安裝套件才能運作。

開設完新專案之後，讀者可以到 config/database.yml 檢視資料庫的設定。

[![圖2：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud02.png "MySQL 的設定範例")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud02.png)  

▲ 圖2：MySQL 的設定範例

#### 頁面與網址的關係

建立頁面前我們要先了解網址的結構，例如以瀏覽器開啟此網址 "http://localhost/phone/show/1"，此段的網址可解釋如下："http://DomainName/controller/action/id"。

我們所要建立的頁面是由一個 controller 跟一個 action構成，因此網址也必須這樣呈現："http://localhost/phone/index"

以下列指令產生上述的頁面：

```
$ rails generate controller phone index
```

其中 phone 是 controller，index 是 action，而 action 在一般說來它是所謂的某個頁面，並對應於 app/views/index.html.erb 檔，副檔名必需是 erb。而 controller 是 action 的集合，控制 action 跟資料庫間輸出的程序。

註：rails generate 指令可以用簡化作 「rails g」。

[![圖3：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud03.png "以Rrails generate 指令來產生相對應的程式")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud03.png)  

▲ 圖3：以 Rrails generate 指令來產生相對應的程式

產生的檔案如圖 3，這樣子就開設好了一個如網址 phone/index 的頁面。請記得用下列指令開啟虛擬伺服器。

```
$ rails server
```

[![圖4：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud04.png "網頁呈現於瀏覽器上的畫面")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud04.png)  

▲ 圖4：網頁呈現於瀏覽器上的畫面

如圖 4，我們就可以在此修改頁面。在圖 3 中，Ruby on Rails 已提示我們到哪個路徑修改網頁。所以開啟 phonebook/app/views/phone/index.html.erb 可以看到以下的內容：

### Phone#index

    Find me in app/views/phone/index.html.erb


有興趣的讀者可以試著修改頁面。

#### 建立 Rails 與資料庫互動的模組

Ruby on Rails MVC 架構的 M 指的是 model，而此 model 是處理資料庫的程序，因此我們要輸入指令來產生資料表：

```
$ rails g model pbook name:string phone:integer birthday:date
```

註：若使用 MySQL 必須先下 "rake db:create" 指令，讓 Ruby on Rails 建立相關的資料庫結構。若使用 SQLite3 則可跳過這步驟。

[![圖5：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud06.png "rake db:create 指令的輸出結果")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud06.png)  

▲ 圖5：rake db:create 指令的輸出結果

如圖5，產生若干檔案。其中較常更改到的檔案為 pbook.rb 以及 20110928100041_create_pbooks.rb。

pbook.rb 是 Rails 中與資料表 pbook 互動的程式，該資料夾的每個資料欄位都會由 pbook.rb 來定義程式與頁面間的邏輯運作。如果沒有關聯式的資料表，通常該檔會保持空白。

20110928100041_create_pbooks.rb 是資料表定義用的檔案，可以在目錄 db/migrate 找到，當配合團體開發時，與此類似的檔案就極為重要。

開啟 20110928100041_create_pbooks.rb 檔案內容如下。

[![圖6：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud07.png "20110928100041_create_pbooks.rb 的檔案內容")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud07.png)  

▲ 圖6：20110928100041_create_pbooks.rb 的檔案內容

以上確定產生 model 後，再輸入指令：

```
$ rake db:migrate
```

[![圖7：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud08.png "rake db:migrate 指令的輸出結果")](http://www.openfoundry.org/imges/111018/RailsIII/rorcrud08.png)  

▲ 圖7：rake db:migrate 指令的輸出結果

Ruby on Rails 就會針對 db/migrate 所產生出的 20110928100041_create_pbooks.rb 開始變更資料庫的資料結構。若想要了解 Ruby on Rails 在資料庫做了哪些異動，可以下指令 tail -f log/development.log 查看。

[![圖8：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud09.png "rake db:migrate 的底層操作記錄")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud09.png)  

▲ 圖8：rake db:migrate 的底層操作記錄

#### 用 rails console 來測試跟資料庫互動的狀況

使用 MySQL 的讀者大都習慣使用 phpmyadmin 或是 mysql 的 console 來檢視資料庫的異動情形。Ruby on Rails 也提供以 console 的方式直接異動資料庫。在建立網頁之前，剛建立好的資料庫沒有任何資料，因此先運用 console 的方式建立一些測試資料，以方便設計網頁的後續工作。

```
$ rails console
```

輸入方式有兩種，第一種是逐行輸入資料：

```
pbook.name = "Merry"pbook.phone = "0922123123"pbook.birthday = "1999/01/01"pbook.save        #資料新增完成之後會返回true。pbook.id         #新增完成會返回該資料的id值。
```

新增情形如下圖，

[![圖9：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud10.png "rails console 指令的輸出結果")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud10.png)  

▲ 圖9：rails console 指令的輸出結果

第二種使用類似 HASH 的格式將資料置入同一行：

```
pbook=Pbook.new(:name=>"John", :phone=>"0988138138", :birthday=>"1995/05/05")pbook.savepbook.id
```

新增情形如下圖，

[![圖10：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud11.png "Hash 格式輸出結果")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud11.png)  

▲ 圖10：Hash 格式的輸出結果

新增完之後，可以直接用 Pbook.find(id) 查詢目前資料新增的狀況，我們已完成網頁設計前的處理工作。

#### 開始設計網頁

開啟 app/controllers/phone_controller.rb 寫一些程式碼將剛剛輸入的資料呈現在網頁上。

[![圖11：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud12.png "app/controllers/phone_controller.rb 的程式碼")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud12.png)  

▲ 圖11：app/controllers/phone_controller.rb 的程式碼

然後再修改 app/views/phone/index.html.erb 檔案，並依照需要寫入來自 phone_controller.rb 的程式碼。

[![圖12：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud13.png "在 app/views/phone/index.html.erb 新增的程式碼")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud13.png)  

▲ 圖12：在 app/views/phone/index.html.erb 新增的程式碼

依照圖 12 的範例輸入完成後，重新整理網頁，呈現結果如下：

[![圖13：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud14.png "app/views/phone/index.html.erb 修改後的結果")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud14.png)  

▲ 圖13：app/views/phone/index.html.erb 修改後的結果

其中在 phone_controller.rb 的第一個 def 區段裡，我們定義了 index 的類別，而

```
@phones = Pbook.all
```

這一行程式碼，其用意是將資料表 Pbook 裡頭的資料用 all 的物件操作把資料全部倒出來，並指派給 @phones 這個實體變數。然後在 view 裡的 index.html.erb 就會接收到 @phones 的實體變數，用 code block 的方式逐一列出資料。

詳見：

```
    ＜% @phones.each do |p| %＞      ＜tr＞＜td＞＜%= p.name %＞＜/td＞＜td＞＜%=p.phone%＞＜/td＞＜td＞＜%=p.birthday%＞＜/td＞＜/tr＞    ＜% end %＞  
```

在上述程式碼中 @phone.each 的 each 會把 @phones 的資料傳給 p，之後 p 將得到各個資料陣列，然後用 p.name、p.phone、p.birthday 的欄位操作予以列印至網頁上。

於是，我們完成 CRUD 的 R。

#### 建立輸入表單

過去在設計表單時，除了要對應文字輸入框跟資料庫外，也要寫 SQL 語言從表單頁一一對應導入程式碼，而會耗去許多時間，且每一個動作都要配合不同的 SQL 語法，如 UPDATE、DELETE、INSERT 這三個動作。

在 Ruby on Rails 中，表單與 SQL 相對應的設計方式比較容易，過程如下：

先打開 config/routes.rb 並依照下圖設定「網址」

[![圖14：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud15.png "config/routes.rb 的設定畫面")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud15.png)  

▲ 圖14：config/routes.rb 的設定畫面

然後在 phone_controller.rb 建立一個 newphone 與 create 的類別，並照下圖輸入幾行程式碼。

[![圖15：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud16.png "修改 phone_controller.rb 程式碼")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud16.png)  

▲ 圖15：修改 phone_controller.rb 程式碼

利用在 views/index.html.erb 加入 link_to 的方法連結下一個頁面，結果如下圖：

```
    ＜%= link_to "edit" , :action =＞ "modify", :id =＞ p%＞ /    ＜%= link_to "del" , :action => "del", :id =＞ p%＞
```

並在最後一行加入 "add new phone number" 的連結，

```
　　＜p＞＜%= link_to "add new phone number" ,phone_newphone_path %＞＜/p＞
```

[![圖16：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud17.png "於 views/index.html.erb 中加入 link_to 的結果")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud17.png)  

▲ 圖16：於 views/index.html.erb 中加入 link_to 的結果

由於我們設定的 modify 與 del 頁面尚未建立，因此連過去時顯示為錯誤，不過，目前要先建立 add new phone 的表單頁面。

在目錄 views 中建立一個名叫 newphone.html.erb 的檔案，並輸入如下圖的程式碼：

[![圖17：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud18.png "newphone.html.erb 的程式碼")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud18.png)  

▲ 圖17：newphone.html.erb 的程式碼

輸入完成後重新整理網頁，並使用瀏覽器瀏覽 "http://localhost/phone/newphone" 就可得到如下圖的結果。

[![圖18：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud19.png "newphone 的瀏覽器呈現畫面")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud19.png)  

▲ 圖18：newphone 的瀏覽器呈現畫面

如此，我們就完成建立表單的步驟。記得在 phone_controller.rb 建立一個 create 的類別並加入處理送往資料庫的程序。至此我們完成 CRUD 的 C。

關於 routes.rb 與 URL Rewriting 的設計，可以回頭看 config/routes.rb 的程式碼。routes.rb 的作用是以 URL Rewriting（重寫網址）技術將上述網址列的各個頁面都導入 rails 的類別裡，而 get、 put、post 在瀏覽器中會發出指令動作向 web server 做出「要求」與「回應」。get 是向伺服器做一般的資料請求並輸出 HTML，而 put 與 post 是表單按下「送出」(submit) 時伺服器會接受表單內的資料，並導入資料庫中。

```
get "phone/newphone"get "phone/index"get "phone/modify/:id" => "phone#modify"post "phone/create"
```

上述程式碼都有相對應的 controller 跟 action ，如果後面還需 id 以檢視資料庫中個別的資料，則必須再自定如上的結構。

如在 index.html.erb 的

```
    ＜%= link_to "edit" , :action =＞ "modify", :id =＞ p%＞    ＜%= link_to "del" , :action => "del", :id =＞ p%＞
```

以及

```
   ＜%= link_to "add new phone number" ,phone_newphone_path %＞  
```

Ruby on Rails 會將 link_to 操作的方法，在 HTML 中轉換為如下的超連結：

[![圖19：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud20.png "link_to 實際對應的 HTML 輸出")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud20.png)  

▲ 圖19：link_to 實際對應的 HTML 輸出

在Ruby on Rails 的 MVC 概念下，controller 與 views 通常密不可分，幾乎 controller 中的每一個類別都會對應一個 views/phone/****.html.erb，如 def newphone 與 def index 等等。

因此，我們在建立表單時，進入 newsphone 後，先將 Pbook.new 的操作導至 @pbook 的實體變數，如：

```
    @pbook = Pbook.new
```

然後，在 newphone.html.erb 設計表單時，form_for 的程式碼會幫助處理表單的介面，只要把 f.text_field 的物件照資料表欄位名稱對應好即可。

```
   ＜%= form_for @pbook , :url =＞ {:controller =＞   "phone", :action => "create"} do |f| %>    
```

form_for 可提供的物件，在經過 code block 傳至變數 f 之後，除了 f.text_field 會輸出成文字輸入框之外，也有其它如 check_box、text_area 等等其它不同型別的表單元素。而 form_for 屬性設定除了接受 @pbook 的實體變數相關變數之外，url 的屬性也必須依照 routes.rb 的設定，傳送至資料庫的處理程序，因此轉變為 HTML 如下圖：

[![圖20：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud21.png "form_for 實際對應的 HTML 輸出")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud21.png)  

▲ 圖20：form_for 實際對應的 HTML 輸出

或許有讀者會問 SQL 語法在哪？由表單按下 Submit 送入後端資料庫的程式端，在圖20 HTML 的

屬性是指定至 /phone/create，也就是在 phone_controller.rb 中所定義的 def create 區段，我們輸入如下的程式碼：

```
def create@pbook = Pbook.new(params[:pbook])@pbook.saveredirect_to (:action => "index")end
```

第二行的程式，是將 Pbook.new 的操作用 params[:pbook] 引入 pbook 表單的各個輸入框，然後在第三行用 save 的方法儲存至 Pbook 的資料表中。完成之後 redirect_to 就會令該頁轉至 index。

因此不需輸入任何 SQL 語法， Ruby on Rails 已經為開發者妥善處理 SQL 與表單間的程序了。



#### 再完成其餘 CRUD 的 U 跟 D

在前幾段詳細說明了表單的設計、HTML 與 routes.rb 的原理，資料的異動還需要「更新」與「刪除」的動作，接著我們將完成本篇電話簿設計的範例。

接著在 phone_controller.rb 再新增 modify、updating、del 的 action，程式碼如下圖所示：

[![圖21：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud22.png "phone_controller.rb 再新增 modify、updating、del 的 action")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud22.png)  

▲ 圖21：phone_controller.rb 再新增 modify、updating、del 的 action

再從目錄 views/phone 新建一個名為 modify.html.erb 的表單，程式碼如同 newphone.html.erb 一樣，但有一個地方不同，如下圖：

[![圖22：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud23.png "從views/phone 新建一個名為 modify.html.erb 的表單")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud23.png)  

▲ 圖22：從views/phone 新建一個名為 modify.html.erb 的表單

記得更改為：action => "updating", :id => @pbook，如此一來點選 edit 即可在網頁上看到文字輸入框已經有相關的資料在裡頭了。如下圖：

[![圖23：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud24.png "瀏覽器的輸出結果")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud24.png)  

▲ 圖23：瀏覽器的輸出結果

記得在 phone_cpntroller.rb 中的 def updating 鍵入完成程式碼，至此我們就完成了 CRUD 的 U 了！

接著在 phone_controller.rb 加入 def del 的類別，但這個類別的刪除方式不必再用 del.html.erb 頁面顯示任何資料，所以重點在於 @pbook.destroy，再導回首頁。

最後，完成了 CRUD 的 D，本篇的線上電話簿範例就宣告完工了。

表單：另外值得一提的是更新資料與新建資料的差異性。在前段文章有提到 `form_for` 的操作方法，其中 `Create` 跟 `Update` 的動作略有不同。Create 資料是新建立過去不存在的資料，而 Update 資料是修改己存在的資料，因此 `form_for` 在 Create 中所顯示的程式碼如下：

```
   ＜%= form_for @pbook , :url =＞ {:controller =＞"phone", :action => "create"} do |f| %>
```

而 Update 的程式碼則為：

```
   ＜%= form_for @pbook , :url =＞{:controller =＞"phone", :action => "updating", :id => @pbook } do |f| %>
```

其中 :action => "updating" 要經過 Submit 後再導入 updating 的區段，而 :id => @pbook 會把 routes.rb 中的 get "phone/modify/:id" => "phone#modify" 之 id 值，幫忙將 Pbook 資料庫的資料導入 f.text_field 各個欄位中。

而 modify、updating 與 del 又是什麼？在 phone_controller.rb 裡，modify 的程式碼很簡單。由 index 裡的 edit 點選之後，會傳送這串網址 http://localhost:3001/phone/modify/13，而 modify 後的 13 就是 id 值。所以使用 Pbook 的 find 操作物件，就可以查詢該 id 值的相關資料。如下方的程式碼：

```
@pbook = Pbook.find(params[:id])
```

Pbook.find 的用法是 Pbook.find(13)，因此要接受從網址而來的參數就用 param[:id] 即可。

而 updating 的類別中，是由 modify.html.erb 按下表單中的送出鈕而觸發 updating 類別，經過 find id 之後，再透過 update_attributes(params[:pbook]) 將更改過的資料回送至資料庫。如下方的程式碼：

```
@pbook.update_attributes(params[:pbook])
```

刪除資料的 del 也是如類似的方式查詢該 id，然後照 id 內的參數，完成刪除資料，如下：

```
@pbook.destroy
```

[![圖24：](http://www.openfoundry.org/images/111018/RailsIII/rorcrud22.png "詳細的程式碼")](http://www.openfoundry.org/images/111018/RailsIII/rorcrud22.png)    

▲ 圖24：詳細的程式碼。

### 總結

最後為讀者整理手刻 CRUD 的一些重點步驟。

1.  建立資料庫相關的連結設定：`config/database.yml`，如要指定其它資料庫，就在建立新專案時加上 `-d SqlServerName`
2.  建立操作資料表相關的 model：`$rails g model table columnA:string columnB:integer birthday:date`
3.  設定相關路由：`config/routes.rb` 可先在建立 conroller 的過程中修改 `routes.rb`，注意改完之後要重啟 server。
4.  建立 `controller` 的 `def action` 跟 `views` 裡的 `_*_.html.erb` 相對應的網頁或表單。

完成以上的範例後您的 Ruby on Rails 的功力即提升五成以上，敬請期待下篇剖析 Rails MVC 概念的文章。

#### **作者簡介**

麥克阿忠，資深網站程式開發者，興趣是攝影。目前擔任 Ruby on Rails 網站開發員，主要使用 Ubuntu Server 進行 Web 應用程式開發。

作者部落格 [http://about.me/MichaelChen520](http://about.me/MichaelChen520)
 歡迎對 Ruby 有興趣的同好前來交流指教。