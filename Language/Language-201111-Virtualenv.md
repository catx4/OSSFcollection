# Python 的虛擬環境及多版本開發利器─Virtualenv 與 Pythonbre
作者：Mosky，2011 年 11 月投稿。

Virtualenv 和 Pythonbrew 都是可以創造虛擬（獨立）Python 環境的工具，只是虛擬（獨立）標的不同。

Virtualenv 可以隔離函數庫需求不同的專案，讓它們不會互相影響。在建立並啟動虛擬環境後，透過 `pip` 安裝的套件會被放在虛擬環境中，專案就可以擁有一個獨立的環境。

簡而言之，Virtualenv 可以幫你做到：

*   在沒有權限的情況下安裝新套件
*   不同專案可以使用不同版本的相同套件
*   套件版本升級時不會影響其他專案

Pythonbrew 則可以在家目錄中安裝多個 Python，並迅速地切換版本；也可以在指定的 Python 版本下批次測試你的 Python 程式；另外更整合了 Virtualenv。

這篇文章會詳細介紹這兩個工具，讓你在多人開發及多版本開發的環境中更得心應手。

### 事前準備

Python 的 package 通常會上傳至 [PyPI](http://pypi.python.org/pypi)，有很多工具都可以從 [PyPI](http://pypi.python.org/pypi) 安裝 package。下面會使用 `easy_install` 這個工具（由 [setuptools](http://pypi.python.org/pypi/setuptools) 提供）來安裝 Virtualenv 和 Pythonbrew。

#### 01\. Linux

如果不知道 `easy_install` 或還沒安裝 setuptools，在 Debian/Ubuntu 可以用下列指令安裝：

```
$ sudo apt-get install python-setuptools

```

在 Fedora/CentOS/Redhat/openSUSE，則可以使用：

```
$ su -
# yum install python-setuptools

```

#### 02\. Windows

在 Windows 則可以從 [setuptools](http://pypi.python.org/pypi/setuptools) 的頁面找到 `*.exe` 格式的安裝檔案。安裝完後，可以在 `C:\PythonX.Y\Scripts\`（X.Y 是 Python 的版本）下找到 `easy_install.exe`。記得把這個路徑放進 Windows 環境變數中的 PATH。

接著就可以輕鬆安裝任何在 [PyPI](http://pypi.python.org/pypi) 的 Python Package 囉。

### Virtualenv - Virtual Python Environment builder

#### 01\. 安裝

Pythonbrew 已整合了 Virtualenv，如果不想額外安裝一個套件，也可以不要裝 Virtualenv。

如果需要安裝，請於命令列模式下輸入下列指令：

```
# easy_install virtualenv

```

#### 02\. 使用方法

##### I. 建立虛擬環境

請於命令列模式下輸入下列指令：

```
$ virtualenv [指定虛擬環境的名稱]

```

例如下列指令會建立名為 "ENV" 的虛擬環境：

```
$ virtualenv ENV

```

預設在建立虛擬環境時，會依賴系統環境中的 site packages，如果想_完全不依賴_系統的 packages，可以加上參數 `--no-site-packages` 來建立虛擬環境：

```
$ virtualenv --no-site-packages [指定虛擬環境的名稱]

```


##### II. 啟動虛擬環境

請先切換當前目錄至建立的虛擬環境中。前例中，建立名稱為 "ENV"，則：

```
$ cd ENV

```

接著，啟動虛擬環境：

```
$ source bin/activate

```

在 Windows 環境中則改用：

```
> \path\to\env\Scripts\activate.bat

```

然後就可以注意到，在 shell 提示字元的最前面多了虛擬環境的名稱提示：

```
(ENV) ...$

```

##### III. 退出虛擬環境

請於命令列模式下輸入下列指令：

```
$ deactivate

```

就可以回到系統原先的 Python 環境。

##### IV. 在虛擬環境安裝新的 Python 套件

Virtualenv 在安裝時會附帶 `pip` 這個 Python 的套件安裝工具，當虛擬環境被啟動時，由它安裝的 package 會出現在虛擬環境的資料夾中，用法是：

```
(ENV)...$ pip install [套件名稱]

```

如果系統也有安裝 `pip`，請特別注意是否已經_啟動_虛擬環境，否則套件會被安裝到系統中，而非虛擬環境裡。

如果想要避免 `pip` 在沒有進入虛擬環境時被使用，可以在 `~/.bashrc` 加上：

```
export PIP_REQUIRE_VIRTUALENV=true

```

要求 `pip` 一定要在虛擬環境中執行。

也可以用下面的設定，讓系統的 `pip` 自動使用啟動中的虛擬環境。

```
export PIP_RESPECT_VIRTUALENV=true

```

避免意外將套件安裝至系統環境。

##### V. 從程式中指定使用虛擬環境的函數庫

無法從 Shell 啟動虛擬環境的情況，像是使用 [mod_python](http://www.modpython.org/) 或 [mod_wsgi](http://code.google.com/p/modwsgi/)，這時可以在 Python 的程式中加上：

```
activate_this = '/path/to/env/bin/activate_this.py'
execfile(activate_this, dict(__file__=activate_this))

```

來使用安裝在虛擬環境中的 packages。

#### 03\. 延伸套件：Virtualenvwrapper

Virtualenvwrapper 是一個 Virtualenv 的 extension，可使虛擬環境的管理變得更容易。

詳細來說，Virtualenvwrapper 提供下述功能：

1.  將所有的虛擬環境整合在一個目錄下。
2.  管理（新增、移除、複製）所有的虛擬環境。
3.  可以使用一個命令切換虛擬環境。
4.  Tab 補全虛擬環境的名字。
5.  每個操作都提供允許使用者自訂的 hooks。
6.  可撰寫容易分享的 extension plugin 系統。

##### I. 安裝

請於命令列模式下輸入下列指令：

```
# easy_install virtualenvwrapper

```

##### II. 使用方法

於 `$WORKON_HOME`製作虛擬環境：

```
$ mkvirtualenv [-i package] [-r requirements_file] [virtualenv options] ENVNAME

```

列出所有的虛擬環境：

```
$ lsvirtualenv [-b] [-l] [-h]

```

`-b` 是簡短模式；`-l` 是詳細模式（預設）；`-h` 是印出 help 資訊。

移除虛擬環境：

```
$ rmvirtualenv ENVNAME

```

複製虛擬環境：

```
$ cpvirtualenv ENVNAME TARGETENVNAME

```

啟動虛擬環境：

```
$ workon [environment_name]

```

如果只輸入 `workon`，則會列出所有的虛擬環境。

離開虛擬環境一樣是使用 `deactivate`。

可以使用下面的設定來告訴 `pip` Virtualenv 的路徑。

```
export PIP_VIRTUALENV_BASE=$WORKON_HOME

```

Virtualenvwrapper 的功能當然不只如此，更多功能可以參考 [Virtualenvwrapper 的官方文件](http://www.doughellmann.com/docs/virtualenvwrapper/index.html)。

### Pythonbrew

#### 01\. 安裝

Pythonbrew 是個比較新的專案，雖然比較新，卻非常完整。它也有整合上面介紹的 Virtualenv。可以用類似 Virtualenvwrapper 的方式來操作 Virtualenv。

安裝方式與 Virtualenv 一樣，只要輸入下面的指令就可以了：

```
# easy_install $ pythonbrew

```

Pythonbrew 官方有推薦的安裝方式，但這篇教學為求一致性，就不額外介紹了，可以參考 [pythonbrew/README.rst](https://github.com/utahta/$%20pythonbrew/blob/master/README.rst)。

對於 Windows 的使用者，很可惜地，Pythonbrew 暫時沒有支援 Windows 的計畫 ([#6: Windows Support? - Issues - utahta/pythonbrew - GitHub](https://github.com/utahta/$%20pythonbrew/issues/6))。所以 Windows 暫時還沒辦法使用 Pythonbrew 囉。

經過 `easy_install` 的安裝後，還需要在 shell 執行：

```
$ pythonbrew_install

```

才會把初始的設定檔和資料夾配置進你的家目錄。接著要修改 `~/.bashrc` 的配置：

```
$ echo "source ~/.pythonbrew/etc/bashrc" >> ~/.bashrc

```

這樣就算安裝完全囉。

Pythonbrew 使用 `curl` 來抓取資料，如果你的系統沒有，請記得安裝。Ubuntu 上可以使用這行指令：

```
$ sudo apt-get install curl

```

#### 02\. 編譯前準備

因為 Pythonbrew 採取下載 tarball，並編譯、安裝的方法，所以我們要先為系統準備好編譯 Python 所需的套件。

也因為許多 Linux 發行版都已打包 Python，所以我們可以偷懶一點，用已經打包好的套件來解決編譯所需的相依性。在 Ubuntu/Debian 上，可以透過：

```
$ sudo apt-get build-dep python2.7

```

來安裝所有編譯 Python 2.7 所需的套件。雖然已經能夠安裝得相當完整，但還是缺少了 `gdbm` 這個 module，如果需要的話，可以透過：

```
$ sudo apt-get build-dep python-gdbm

```

來安裝編譯 `gdbm` 所需的套件。

註：`bsddb185`、`linuxaudiodev`、`ossaudiodev`、`sunaudiodev` 等是按以上方式安裝後，仍會缺少的 module。其中 `ossaudiodev`（Open Sound System）在隨 Ubuntu 發布的 Python 中有提供，列出來讓大家參考。

Fedora/CentOS/Redhat/openSUSE 則可以使用 `yum-builddep` 這個指令。

#### 03\. 使用方法

Pythonbrew 的操作不外乎安裝、移除、列出及使用新的 Python 版本，下面是依照初次使用時所需的指令順序來介紹。

##### I. 列出可安裝的版本

首先我們用 `list --know` 列出可以安裝的 Python 版本：

```
$ pythonbrew list --know

```


##### II. 安裝新的版本

接著利用 `install VERSION` 來下載並編譯 Python 到本機，除了接 Python 的版本編號以外，也可以接 Python 的 tarball 路徑或網址來安裝；也能調整編譯 Python 的選項。下面是一些例子：

```
$ pythonbrew install 2.7.2
$ pythonbrew install --verbose 2.7.2
$ pythonbrew install --force 2.7.2
$ pythonbrew install --no-test 2.7.2
$ pythonbrew install --configure="CC=gcc_4.1" 2.7.2
$ pythonbrew install --no-setuptools 2.7.2
$ pythonbrew install http://www.python.org/ftp/python/2.7/Python-2.7.2.tgz
$ pythonbrew install /path/to/Python-2.7.2.tgz
$ pythonbrew install /path/to/Python-2.7.2
$ pythonbrew install 2.7.2 3.2

```

##### III. 清理安裝時產生的檔案

下載的 Python tarball 會放在 `~/.pythonbrew/dists/` 下；而編譯則會在 `~/.pythonbrew/build/` 下進行。如果想清理這兩個目錄，可以使用：

```
$ pythonbrew cleanup

```

##### IV. 列出所有已安裝的版本

安裝好之後，可以使用 `list` 命令列出所有已安裝的 Python 版本：

```
$ pythonbrew list

```

後面有打星號的，就是現在正在使用的 Python 版本。

##### V. 切換已安裝的版本

可以使用 `switch` 來切換預設的 Python 版本：

```
$ pythonbrew switch VERSION

```

如果只想在當前的 shell 下切換，可以使用 `use`：

```
$ pythonbrew use VERSION

```

要切換回預設的環境時，使用 `off`：

```
$ pythonbrew off

```

就會返回系統環境的 Python 了。

##### VI. 批次在不同版本下測試

最重要的是，可以用系統內所有安裝過的 Python 版本，或指定的 Python 版本來測試自己的程式！

```
$ pythonbrew py test.py # 使用所有有安裝的版本
$ pythonbrew py -v test.py # 詳細輸出
$ pythonbrew py -p 2.7.2 -p 3.2 test.py # 指定特定的版本

```

##### VII. 移除已安裝的版本

若想移除已經安裝的 Python，則可以使用 `uninstall`：

```
$ pythonbrew uninstall 2.7.2
$ pythonbrew uninstall 2.7.2 3.2

```

##### VIII. 與 Virtualenv 的整合

要注意 Pythonbrew 中所提供的 Virtualenv，是基於 Pythonbrew 中所安裝的 Python（置於 `~/.pythonbrew/venvs/Python-VERSION/` 下）。在不使用 Pythonbrew 的情況下，無法使用附屬於 Pythonbrew 的 `venv` 功能。

Pythonbrew 提供了和 Virtualenvwrapper 類似的功能，只是沒有像 Virtualenvwrapper 那麼完整的 plugin 系統。所有在 Pythonbrew 中的 Virtualenv 指令都以 `venv` 作為第一個副命令。

```
$ pythonbrew venv create [指定虛擬環境的名稱]
$ pythonbrew venv list
$ pythonbrew venv use [指定虛擬環境的名稱]
$ pythonbrew venv delete [指定虛擬環境的名稱]

```

離開虛擬環境一樣是使用 `deactivate`。

##### IX. Buildout

如果有使用 [Buildout](http://www.buildout.org/) 這個工具，也可以透過 Pythonbrew 來執行：

```
$ pythonbrew buildout
$ pythonbrew buildout -p 2.6.6 # 指定版本

```



##### X. 自我更新

最後，Pythonbrew 有內建更新自己的指令：

```
$ pythonbrew update
$ pythonbrew update --master
$ pythonbrew update --develop

```

Virtualenv 和 Pythonbrew 就介紹到這邊，如果想獲得更多資訊，可以多多參考它們的官網哦。祝大家玩得愉快。

### 參考資料

*   [www.virtualenv.org](http://www.virtualenv.org/)
*   [utahta/pythonbrew - GitHub](https://github.com/utahta/pythonbrew)
*   [setuptools - PyPI](http://pypi.python.org/pypi/setuptools)
*   [pip - PyPI](http://pypi.python.org/pypi/pip)
*   [virtualenvwrapper documentaion](http://www.doughellmann.com/docs/virtualenvwrapper/)
*   [Building Python and more on missing modules - Stack Overflow](http://stackoverflow.com/questions/6171210/building-python-and-more-on-missing-modules)
