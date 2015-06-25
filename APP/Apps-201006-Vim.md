# Vim - 編輯緩衝區以及編輯視窗 (Buffers and Windows)

作者：林佑安，2010 年 6 月投稿 。

Vim 內建視窗分割功能，可於編輯器內分割多重視窗及分頁來並行編輯檔案。

各編輯視窗 (Window) 可載入不同編輯緩衝區 (Buffer) 來進行編輯，不限於當前的編輯緩衝區。 編輯視窗也可載入共同的編輯緩衝區來編輯。 事實上編輯視窗基本上就是一個編輯緩衝區的觀察窗口 (viewport)。

由於可開啟多重編輯視窗來並行編輯，因此若能善用編輯視窗及編輯緩衝區等功能，便可提高編輯效率。

### 編輯緩衝區 (Buffers)

在討論編輯視窗之前，需要先來討論編輯緩衝區。

編輯緩衝區有三種狀態，分別為: 啟用(active), 隱藏(hidden), 非啟用(inactive)。

編輯視窗在關閉時，只是將觀察窗口關閉，所以並不會影響到編輯緩衝區，實際上編輯緩衝區還是會在 執行中的 Vim 內，需要時，仍可使用編輯緩衝區命令將該緩衝區載回。

以下簡單示範緩衝區應用之作法:

於命令列可同時開啟多檔案載入緩衝區:

      $ vim file1 file2 file3

此時 file1 會出現在第一個編輯視窗，而 file2 file3 會在背景，可透過 `:buffers` 命令來查看緩衝區清單:

      :buffers  1 %a   "file1"                        line 1  2      "file2"                        line 0  3      "file3"                        line 0

而 `:buffers` 有其他兩個別名，分別為: `:ls` , `:files`。

欲切換至 file2 及 file3 的 編輯緩衝區需要 `:bnext` (載入下一緩衝區) , `:bprev` (載入前一緩衝區) 或 `:[N]buffer [N]` (載入第 [N] 個緩衝區)。

不過每次翻閱緩衝區需鍵入指令 :bnext , :bprev 仍是麻煩，所以筆者建議可加入以下快捷鍵對應，請於 ~/.vimrc 內加入:

      nmap <C-b>n  :bnext<CR>  nmap <C-b>p  :bprev<CR>

如此可以 Ctrl-b n 以及 Ctrl-b p 來翻閱編輯緩衝區，若不習慣 Ctrl-b 也沒關係 可自行代換成其他快捷鍵組合。

要跳至特定編輯緩衝區可使用 :[N]buffer 命令，如:

      :2buffer

或

      :buffer 2

如此便可跳至 file2 之編輯緩衝區。

而:

      :2buffer!

則可強制跳至 file2 之編輯緩衝區。

若直接輸入緩衝區名稱也行，如:

      :buffer file3

若要新增其他檔案進編輯緩衝區，可使用 `:badd` 命令:

      :badd path/to/file4

若要於目前編輯緩衝區編輯檔案:

      :edit path/to/file5

離開編輯視窗或是關閉編輯緩衝區時，該編輯緩衝區其實還留在記憶體中， 使用命令 `:ls!` 可以將隱藏的編輯緩衝區列舉出來。

若要將編輯緩衝區完全卸載，則可使用 bwipeout 命令:

      :[N]bw[ipeout][!]  :bw[ipeout][!] {bufname}  :N,Mbw[ipeout][!]  :bw[ipeout][!] N1 N2 ...

舉例來說:

      :1bw  :bw path/to/file  :1,5bw!  :bw 1 2 3 4

該參數可以是編輯緩衝區的編號或是完整路徑。

#### 相關選項

      buftype bufhidden buflisted swapfile modifiable

### 編輯視窗

編輯視窗內可以切換不同的編輯緩衝區，常用編輯視窗命令如下:

      :split     - 將目前編輯視窗水平分割為二，新的視窗為原有的編輯緩衝區  :vsplit    - 將目前編輯視窗垂直分割為二，新的視窗為原有的編輯緩衝區  :split  path/to/file   - 開啟新的水平分割視窗來編輯檔案  :vsplit path/to/file   - 開啟新的垂直分割視窗來編輯檔案  :new                   - 開啟新的水平分割視窗，並且開啟新的編輯緩衝區  :vnew                  - 開啟新的垂直分割視窗，並且開啟新的編輯緩衝區

有時在分割視窗時，可以利用 topleft 以及 botright 命令來指定分割視窗位置，例如:

從上方左側開啟分割視窗:

      :topleft split 

從下方右側開啟分割視窗:

      :botright split

常用的編輯視窗快捷鍵如下:

垂直分割:

      Ctrl-w v

水平分割:

      Ctrl-w s

切換視窗 Focus:

      Ctrl-w [hjkl]

交換視窗內的編輯緩衝區:

      Ctrl-w x

移動目前視窗至 (H: 最左側, L: 最右側, J: 最下方, K: 最上方):

      Ctrl-w [HJKL]

將目前的編輯視窗移動至新的分頁:

      Ctrl-w T

將目前的水平分割編輯視窗最大化:

      Ctrl-w _

將目前的垂直分割編輯視窗最大化:

      Ctrl-w |

將目前分割的視窗重新平均分配大小:

      Ctrl-w =

在瀏覽程式碼時，可使用之視窗相關命令如下:

`Ctrl-w f`

> 於目前游標底下的檔案名稱或檔案路徑，開啟新的視窗來讀取該檔案。 要設定 Ctrl-w f 所搜尋檔案的路徑，可以設定 'path' 選項，如: set path+=/usr/include/

`Ctrl-w F`

> 於目前游標底下的檔案名稱或檔案路徑，開啟新的視窗來讀取該檔案， 並且跳至檔名或檔案路徑之後所跟隨的行號。例如:
> 
>         ~/.vimrc:300
> 
> 於按下 ~/.vimrc 上按下 Ctrl-w F 之後，便會以水平分割視窗開啟 ~/.vimrc 檔案，並且跳至第 300 行。

`Ctrl-w gf`

> 同 `Ctrl-w f`。不過開啟的檔案會在新的分頁開啟。同樣的功能可透過 `tab split` 以及 `gf` 來達到。

`Ctrl-w gF`

> 同 `Ctrl-w F`。 不過開啟的檔案會在新的分頁開啟。同樣的功能可透過 `tab split` 以及 `gF` 來達到。

### 相關腳本片段

#### 設定 gVim 一開始的視窗大小

將以下設定放入 .vimrc 內:

      set lines=50 columns=100

或是:

      if has("gui_running")      " GUI is running or is about to start.      " Maximize gvim window.      set lines=999 columns=999  else      " This is console Vim.      if exists("+lines")          set lines=50      endif      if exists("+columns")          set columns=100      endif  endif

#### 簡易調整編輯視窗大小

要調整水平分割視窗大小，通常使用 Ctrl-w + , Ctrl-w - ，若要方便調整，可使用下列設定:

      nmap + <C-W>+  nmap - <C-W>-

直接使用 + 或 - 來調整視窗大小。

垂直分割視窗大小，則可嘗試下列設定:

      nmap <Right>  <C-w>>  nmap <Left>  <C-w><

#### 快速切換垂直分割視窗 focus

      map <C-j> <C-w>j<C-w>_  map <C-k> <C-w>k<C-w>_

如此會切換至下方或上方視窗，並且調整至最大化。

#### 設置最小、預設視窗大小

      set winminheight=0  set winheight=999

#### 以循環方式切換編輯緩衝區

      :nnoremap <C-n> :bnext<CR>  :nnoremap <C-p> :bprevious<CR>

#### 以正規表示式搜尋編輯緩衝區

要快速搜尋編輯緩衝區，可以使用下方的 Vim Script Function:

      "   buffer sel by pattern {{{  fu! BufSel(pattern)      let buf_count = bufnr("$")      let cur_bufnr = 1      let nummatches = 0      let firstmatchingbufnr = 0      while cur_bufnr <= buf_count          if(bufexists(cur_bufnr))          let currbufname = bufname(cur_bufnr)          if(match(currbufname, a:pattern) > -1)              echo cur_bufnr . ": ". bufname(cur_bufnr)              let nummatches += 1              let firstmatchingbufnr = cur_bufnr          endif          endif          let cur_bufnr = cur_bufnr + 1      endwhile      if(nummatches == 1)          execute ":buffer ". firstmatchingbufnr      elseif(nummatches > 1)          let desiredbufnr = input("Enter buffer number: ")          if(strlen(desiredbufnr) != 0)          execute ":buffer ". desiredbufnr          endif      else          echo "No matching buffers"      endif  endf  fu! BufSelInput()      let pattern = input( "pattern: " )      call BufSel( pattern )  endf  "Bind the BufSel() function to a user-command  com! -nargs=1 Bs    :call BufSel("<args>")  nmap <leader>bf      :call BufSelInput()<CR>

將以上程式碼加入至 `~/.vimrc` 後，即可使用 `:Bs` 命令以及 `\\bf` 快捷鍵來呼叫該函式來使用正規表示式來搜尋符合的編輯緩衝區，如:

      :Bs vim.*

或是按下 `\\bf` ，則會出現要求輸入 Pattern 的提示符。

#### 顯示編輯緩衝區資訊

.vimrc 加入此一片段:

      fun! BufInfo()      echo "[bufnr ] ".bufnr("%")      echo "[bufname ] ". expand("%:p")      echo "[cwd ] " . getcwd()      if filereadable(expand("%"))          echo "[mtime ] " . strftime("%Y-%m-%d %H:%M %a",getftime(expand("%")))      endif      echo "[size ] " . Bufsize() . " bytes"      echo "[comment ] " . (exists('b:commentSymbol') ? b:commentSymbol : "undefined")      echo "[filetype ] " . &ft      echo "[tab ] " . &ts . " (" . (&et ? "" : "no") . "expandtab)"      echo "[keywordprg] " . &keywordprg      echo "[makeprg ] " . &makeprg      echo "[Buffer local mappings]"      nmap <buffer>  endf  com! BufInfo :cal BufInfo()<CR>

最後呼叫 `:BufInfo` 命令即可顯示編輯緩衝區資訊。

### 相關 Plugin

#### FuzzyFinder

若有使用 FuzzyFinder Plugin ，則可加上此段設定來搜尋編輯緩衝區 (Buffer)。

      :nmap <silent> <leader>fb :FufBuffer<CR>

FuzzyFinder: [http://www.vim.org/scripts/script.php?script_id=1984](http://www.vim.org/scripts/script.php?script_id=1984)

#### Bufexplorer

使用 Bufexplorer 可顯示出緩衝區清單，並且以選單的方式切換編輯緩衝區。

Bufexplorer: [http://www.vim.org/scripts/script.php?script_id=42](http://www.vim.org/scripts/script.php?script_id=42)

#### filefind

filefind.vim 是一依據 find 命令的結果的 Vim 延伸插件。

平常需求為:

      $ find path/to -type f -iname "*pattern*"

使用 find 命令加上 -type f 參數搜尋檔案，但是要對該檔案清單另外處理就麻煩了，這時候最直接想到的是:

      $ find path/to -type f -iname "*pattern*" | vim -

將該結果導向給 vim ，對 vim 命令而言，加上 "-" 則為從 stdout 讀取結果導至 Buffer (編輯緩衝區)。

此時若要開啟、重新命名、編輯檔案，便可利用 filefind.vim 插件來執行這些操作。

安裝：

      可使用 git 將 repository 抓取下來:  $ git clone 
     git@github.com :c9s/filefind.vim.git  $ cd filefind.vim  $ make install

只需要呼叫 make 命令即可安裝。 :-) 用法可參考 github 上的 README file.

filefind.vim: [http://github.com/c9s/filefind.vim](http://github.com/c9s/filefind.vim)

#### tselectbuffer

類似 bufexplorer。

tselectbuffer: [http://vim.sourceforge.net/scripts/script.php?script_id=1866](http://vim.sourceforge.net/scripts/script.php?script_id=1866)

#### 作者

[林佑安 (c9s)](mailto:cornelius.howl_DELETE_ME_@gmail.com) ，

熟 Vim Script、 Perl 程式設計及網頁相關等技術。開發及維護多項 Vim Plugin 及 Perl 相關模組。多數 Vim Plugin 可於 Github.com 上找到。

[Blog](http://c9s.blogspot.com)

[Github](http://github.com/c9s)