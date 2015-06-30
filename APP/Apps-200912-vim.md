# Vim - 自動補齊

作者：Cornelius，2009 年 12 月投稿。

自動補齊 (Auto-Completion) 一直是每個整合環境編輯器 (IDE) 都必備的一項功能，因此 Vim 也不例外的支援了自動補齊 (Auto-Completion) 的功能。

其中最大的不同就是使用者可以撰寫或設定自己的補齊函式 (Completion Function)，其中還包含了使用者定義補齊 (User-defined completion)、全補齊 (Omni-completion)、關鍵字補齊 (Keyword completion)、檔案名稱補齊 (Filename completion)、字典補齊 (Dictionary completion)、詞庫補齊 (Thesaurus completion) 以及 Vim 指令補齊 (Vim command completion ) 等等各種的自動補完方式。

其中，使用者定義補齊 (User-defined completion) 以及全補齊 (Omni-completion)是可以由使用者定義函式 (Function)來延展補齊的精確性以及廣度。而字典檔補齊 (Dictionary completion) 以及詞庫補齊 (Thesaurus)則採用讀取外部文字檔案來做字典來源的補齊。

我們接下來將一一介紹基本的補齊操作以及各種補齊方式的使用，再進階進入撰寫自己的自動補齊函式。自動補齊主要在插入模式 (Insert Mode) 以 Ctrl-x 鍵作為補齊觸發鍵，再接上一個補齊類型的觸發鍵。

以下是內建的補齊列表:

C-x C-l 行補齊
 C-x C-n 以目前檔案做關鍵字補齊
 C-x C-k 由字典補齊
 C-x C-t 由分類詞庫補齊
 C-x C-i 從目前以及被引入的檔案為來源做關鍵字補齊
 C-x C-] 標籤 (tags) 補齊
 C-x C-f 檔案名稱補齊
 C-x C-d 定義或巨集補齊
 C-x C-v Vim 指令補齊
 C-x C-u 使用者定義函式補齊
 C-x C-o 全補齊函式做補齊
 C-x s 拼字校正補齊
 C-n 關鍵字補齊，不過依據 `complete` 選項來決定關鍵字來源

接下來對每種補齊方式一一做說明。

####※ Ctrl-x Ctrl-l 行補齊 (Line completion):
 
 以往前搜尋行，找出與目前行相同開始的行做補齊項目 (Completion Item)。並且以 `complete`選項來決定來源的項目。行補齊將忽略縮排。使用行補齊 (Ctrl-x Ctrl-l)之後，可以再次按下 Ctrl-x Ctrl-l，這樣就會將當前行的補齊結果作為下一行的補齊。
 
####**※ Ctrl-x Ctrl-n 以目前檔案內容做關鍵字補齊 (Keyword completion):**
 
 可使用 Ctrl-x Ctrl-n 往後搜尋關鍵字做補齊，Ctrl-x Ctrl-p 往前搜尋關鍵字補齊。可參考 `iskeyword` 選項來定義關鍵字的字元。

####**※ Ctrl-x Ctrl-k 字典補齊 (Dictionary completion):**

 使用 `dictionary`選項所設定的字典檔案來做自動補齊。將使用字典檔案內的關鍵字來做自動補齊。舉例來說，新增字典檔案到目前的 `dictionary` 選項可用 set dictionary+=/path/to/you_dictionary 那麼 Ctrl-x Ctrl-k 時，便可以以這個字典檔作為補齊。
 
#### **※ Ctrl-x Ctrl-t 分類詞庫補齊 (Thesaurus completion):**
 
 其實和字典補齊 (Dictionary completion) 很類似，差異在 Thesaurus completion 所作的是在分類詞庫裡頭找到符合的關鍵字之後，相同一行的其他關鍵字會成為補齊的選項。
 
#### **※ Ctrl-x Ctrl-I 由引入檔做關鍵字補齊**

 常在撰寫程式碼時用到，舉例來說，在 C/C++ 程式碼我們寫:
 
``` #include “stdio.h”**```

 那麼在 Ctrl-x Ctrl-i 做補齊的時候，Vim 會自動去 stdio.h 裡面搜尋符合的關鍵字。但是要注意的是 `path` 選項，記得將 C 語言或任何其他語言的引入檔路徑加入 `path`。以 C 語言為例:
 
 ```set path+=/usr/share/include/glib/```

 如此 Vim 才會在相關路徑內找到你的標頭檔。
 
#### **※ Ctrl-x Ctrl-] 標籤補齊 (Tag completion)**

 由標籤檔 (tags file) 來取得關鍵字來做自動補齊。標籤檔通常由 `ctags` 所產生。
 
#### **※ Ctrl-x Ctrl-f 檔案名稱補齊 (Filename completion)**

 此種補齊用以補齊檔案名稱或者路徑。（可使用 `isfname` 選項來決定檔案名稱的樣式 (Pattern)）
 
#### **※ Ctrl-x Ctrl-d 定義或巨集補齊 (Definitions and Macros completion)**

 在目前檔案以及引入檔案的內容搜尋定義以及巨集名稱做補齊。
 
#### **※ Ctrl-x Ctrl-v Vim 命令補齊 (Vim command completion)**

 Vim 命令補齊，撰寫 Vim script 時非常有用。

#### **※ Ctrl-x Ctrl-u 使用者定義補齊 (User-defined completion)**

 由使用者定義的函式來補齊，使用者可撰寫自己的函式來客製化補齊，可透過 `completefunc` 選項來設置補齊函式。
 
#### **※ Ctrl-x Ctrl-o 全補齊 (Omni completion)**
 
 全補齊類似使用者定義函式補齊，不過通常為特定檔案類型做補齊。可透過 `omnifunc` 選項定義函式名稱。
 
#### **※ Ctrl-x s 拼字校正補齊 (Spelling suggestions completion)**
 
 批字校正補齊會在游標前或是游標上的字上提供建議的單字作為取代。
####  由於 Ctrl-s 會使得某些 Terminal 造成 scroll lock，所以建議直接以 Ctrl-x s 來呼叫拼字補齊。

#### **※ Ctrl-n 關鍵字補齊 (由 `complete` 選項決定關鍵字來源)**

 這種補齊由 `complete` 選項決定關鍵字補齊。

###**◎ 簡單介紹 `complete` 選項：**

`complete` 為一字串，選項由逗號分別，預設的 `complete` 值為 “.,w,b,u,t,i”
 各個選項的說明如下:

. 掃描目前 Buffer。

w 掃描其他視窗的 Buffer

b 掃描其他在清單中的 Buffer

u 掃描已經卸載的 Buffer（但仍然在 Buffer 清單中）

U 掃描不在 Buffer 清單內的 Buffer。也就是 Unlisted Buffer ，被隱藏起來的 Buffer。

k 掃描 `dictionary` 指定的字典做補齊。

kspell 使用目前的拼字檢查做補齊。

k{dict} 掃描檔案 {dict} ，可以同時有多個 k 旗標設置，來掃描不同的字典檔。

譬如說
 :set cpt=k/usr/dict/*,k~/spanish

s 使用 `thesaurus` 選項做分類詞庫補齊。

s{tsr} 掃描 {tsr} 檔案做分類詞庫補齊。

i 掃描目前的檔案以及引入的檔案。

d 掃描目前的檔案以及引入的檔案，並且做定義 (defined name) 或巨集(macro) 的補齊。

] 使用標籤補齊 (tag completion)

t 同 “]”

所以預設的選項 ".,w,b,u,t,i" 意思就是掃描:

1\. 目前的 Buffer
 2\. 其他視窗的 Buffer
 3\. 其他載入的 Buffer
 4\. 其他卸載的 Buffer
 5\. 標籤 (tags)
 6\. 引入檔


###◎ 使用者定義補齊 (User-defined Completion) 以及全補齊 (Omni Completion)：


介紹如何定義補齊函式 (Completion Function)

Completion Function 的運作方式:

第一次被呼叫必須先找到補齊的定位點，函式必須回傳補齊定位點的位置。（為整數）
 第二次被呼叫時，函式會被給予已輸入字串作為參數，Completion function 再
 利用此字串搜尋需補齊的項目，並且回傳。

基本上補齊函式的原型為:

function! CompletionFunc(findstart,base)

endfunction

其中 CompletionFunc 為你的補齊函式名稱，這個函式必須取得兩個參數
 a:findstart 以及 a:base。

a:findstart 參數第一次被呼叫時，會被設定為 1 ，此時我們必須找到補齊的定位點，
 並且回傳補齊定位點的位置。

第二次被呼叫時，a:findstart 會為 0 ，a:base 會被設定為游標位置至補齊定位點的字串

假設我們輸入字串至:

how comple
 ^
 cursor

那麼按下 C-x C-o 或 C-x C-u 呼叫函式補齊時，我們第一次會接到 a:findstart 為 1
 我們回傳 5 (c 的位置) ，第二次時，我們會接到 a:base 回 "comple"
 ，接著我們可回傳 一串列作為可補齊的項目，如:

[ "complete" , "completion" , "completefunc" ]

假設我們只回傳

["completion"]

那麼就會自動補齊至 "completion"。

所以 Completion Function 的範本基本上可以寫成:

function! Func(findstart,base)
 if a:findstart
 " 尋找補齊定位點

return
 else
 " 使用 a:base 尋找補齊項目

return
 endif
 endfunction

那麼一個簡單的月份補齊函式可以這樣寫：（取自 vim help 文件 complete-functions 條目）

fun! CompleteMonths(findstart, base)
 if a:findstart
 " locate the start of the word
 let line = getline('.')
 let start = col('.') - 1
 while start > 0 && line[start - 1] =~ '\a'
 let start -= 1
 endwhile
 return start
 else
 " find months matching with "a:base"
 let res = []
 for m in split("Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec")
 if m =~ '^' . a:base
 call add(res, m)
 endif
 endfor
 return res
 endif
 endfun
 set completefunc=CompleteMonths

在回傳補齊項目的串列時，我們也可以是回傳 Dictionary
 的串列，換而言之就是由雜湊 (hash) 組成的串列，這個雜湊可以有以下索引值:

word 補齊字串

abbr 補齊字串的縮寫，當不為空值時，abbr 會成為補齊選單
 用以顯示補齊字串的字串。

menu 標注的文字
 用以顯示在補齊項目後面。

info 額外資訊，當不為空時，額外資訊會被顯示在預覽視窗中。

kind 該項目的補齊類型。（為單個字元）

icase 忽略大小寫，為 1 時將忽略大小寫。
 同樣的補齊項目會被視為同一個項目。

dup 決定重複的字是否加入補齊選單

譬如說:

let list = []
 for item in items
 if item.text =~ '^'.a:base
 cal add(list, { 'word': item.text , 'abbr' : item.abbr , 'menu' : item.file })
 endif
 endfor
 return list

上述寫法是將所有結果處完之後才回傳
 有時補齊項目需要花比較久的時間搜尋，那麼我們可以利用 complete_add() 以及
 complete_check() 函式來即時插入補齊項目，而不需要等待所有搜尋完成:

for item in items
 if item.text =~ '^'.a:base
 cal complete_add({ 'word': item.text , 'abbr' : item.abbr , 'menu' : item.file })
 endif
 if complete_check()
 break
 endif
 endfor

###**◎ 特定檔案類型的全補齊 (Omni completion)**

可參考 *compl-omni-filetypes* 條目，
 此章節針對各種檔案類型的補齊做了說明。

Vim 已經內建了大部份主要程式語言的全補齊函式 (Omni completion
 function)，相關程式碼可以參考 $VIMRUNTIME/autoload/*complete.vim

###**◎ 相關 Plugin**

[**AutoComplPop**](http://www.vim.org/scripts/script.php?script_id=1879)

Takeshi Nishida 寫的 AutoComplPop (ACP)
 可以自動呼叫補齊函式，不需另外按快捷鍵呼叫，ACP 會在已定義的 樣式 (Pattern)
 呼叫相對應的補齊模式。

使用 [Vimana](http://search.cpan.org/dist/Vimana/) 可直接安裝

$ vimana install autocomplpop

[**NeoComplCache**](http://www.vim.org/scripts/script.php?script_id=2620)

由於多數補齊函式運作繁複且花費許多時間剖析字串，所以
 日本的 Shougo Matsushita 寫了 NeoComplCache 來做 Completion 的快取。

使用 [Vimana](http://search.cpan.org/dist/Vimana/) 可直接安裝

$ vimana install neocomplcache

####**◎作者簡介**

Cornelius，目前在 AIINK（愛印網），以 Perl 語言開發的 Jifty web framework 從事網站開發相關工作。於 CPAN - Perl 模組典藏網維護多個 Perl 模組，參與 Jifty, SD 等 Perl 相關開放原始碼專案 。主要以 Vim 做為開發工具，著有 cpan.vim , perl-completion.vim , perldoc.vim 等多個 vim 相關 Plugin。
 [Github](http://www.github.com/c9s)
 [Twitter](http://twitter.com/c9s)
 [Plurk
](http://www.plurk.com/c9s) [Blog](http://c9s.blogspot.com/)
 [Google group](http://groups.google.com/group/vim-taiwan)