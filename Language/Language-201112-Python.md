# Python 套件管理程式簡介
作者：林錦賜 (pct)，2011 年 12 月投稿。 

### 前言

對任何作業系統以及程式語言而言，管理「擴充套件」是非常重要的一環。有了擴充套件，可以更容易地操作電腦，程式設計師寫程式也變得更輕鬆。

您也許聽過「不要重造輪子」這句話，或是 DRY (Don't Repeat Yourself)，講得就是「別人已經寫好的東西，就拿去用吧，不用自己再重新寫一套」。

本次介紹的 `easy_install` 以及 `pip`，正是 Python 程式語言的套件管理程式，讓您可以直接使用前人的心血結晶。

以下敘述的指令，以 # 開頭的，請用 root 權限；以 $ 開頭的，使用一般使用者權限即可。

### Python 的擴充套件格式──蛇蛋

Python 的代表動物是「蛇」。有趣的是，Python 的擴充套件以 `.egg` 結尾。`.egg` 可以是個目錄或 zip 檔。


#### 01\. 安裝 easy_install

目前許多作業系統都已配置好相關的 Python 程式語言執行環境，同時也內建了 `esay_install`。若發現系統中沒有 `easy_install`，可以依以下方式安裝：

##### I. 利用作業系統套件管理程式安裝

使用作業系統的套件管理程式尋找，在 Linux 相關的發行版本中，名稱通常為 `setuptools`。

##### II. 至官方網站上安裝

請至以下網址中尋找 `ez_setup.py` 的連結。

```
http://pypi.python.org/pypi/setuptools

```

下載後，請於命令列模式下輸入下列指令：

```
# python ez_setup.py

```

如果作業系統為 Windows，亦可使用 Windows 的安裝檔，檔名為 `setuptools-0.6c11.win32-py2.x.exe`。

#### 02\. 安裝 pip

與 `easy_install` 類似，有些作業系統已經內建了。如果沒有，請於命令列模式下輸入下列指令：

```
# curl http://python-distribute.org/distribute_setup.py | python
# curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python

```

若是在有 `easy_install` 的環境中，請於命令列模式下輸入下列指令：

```
# easy_install pip

```

依據 pip 官方網站的說明，setuptools 目前還不支援 python3，所以如果您的 python 版本是 3.x，請記得使用第一種安裝方法。

### 使用範例

#### 01\. easy_install

##### I. 利用 easy_install 安裝套件

請於命令列模式下輸入下列指令：

```
# easy_install [套件名稱]

```

預設會安裝目前最新的版本。

亦可以指定安裝的特定版本，如下：

```
# easy_install '[套件名稱]==[版本]'

```

以安裝 `virtualenv` 並指定 1.6.3 版本為例：

```
# easy_install 'virtualenv==1.6.3'

```

可以指定一個範例的版本：

```
# easy_install 'virtualenv>=1.6.3'
# easy_install 'virtualenv<1.6.3'

```

也可以指定一個網路連結來安裝：

```
# easy_install http://example.com/virtualenv.egg

```

若在安裝前想了解一下安裝的執行，可以使用 `--dry-run` 參數，如：

```
# easy_install virtualenv --dry-run

```

##### II. 利用 easy_install 移除套件

若使用如下範例，`easy_install` 會將自己 easy-install.pth的 virtualenv 移除；此時可以去 python 的 packages 目錄手動「移除」所有 virtualenv 的檔案：

```
# easy_install -m virtualenv

```

##### III. 利用 easy_install 列出所有已安裝的套件

`easy_install` 本身不支援此功能，但可以利用 `yolk` 來達到。

首先，利用 `easy_install` 安裝 `yolk`。請於命令列模式下輸入下列指令：

```
# easy_install yolk

```

再使用 `yolk` 及其參數 `l`。請於命令列模式下輸入下列指令：

```
$ yolk -l
PIL             - 1.1.6        - active
Python          - 2.5.4        - active
html5lib        - 0.11.1       - active
...

```

若僅要列出 "active" 的套件，可使用參數 `a`。請於命令列模式下輸入下列指令：

```
$ yolk -a

```

若僅要列出可升級的套件，可使用參數 `U`。請於命令列模式下輸入下列指令：

```
$ yolk -U

```

##### IV. 利用 easy_install 升級套件

請於命令列模式下輸入下列指令：

```
# easy_install -U virtualenv

```

若要一次升級所有可升級的套件，則需配合先前介紹過的 `yolk`。請於命令列模式下輸入下列指令：

```
# yolk -U | cut -d ' ' -f 2 | xargs easy_install

```

##### V. 列出使用範例

想要了解更多 `easy_install` 的使用範例，可於命令列模式下輸入下列指令：

```
# easy_install --help

```

#### 02\. pip

##### I. 利用 pip 安裝套件

請於命令列模式下輸入下列指令：

```
# pip [套件名稱]

```

預設會安裝目前最新的版本。

亦可以安裝指定版本，如下：

```
# pip '[套件名稱]==[版本]'

```

以安裝 `virtualenv` 並指定 1.6.3 版本為例：

```
# pip 'virtualenv==1.6.3'

```

或可以指定一個範例的版本：

```
# pip 'virtualenv>=1.6.3'
# pip 'virtualenv<1.6.3'

```

也可以指定一個網路連結來安裝：

```
# pip install install http://example.com/virtualenv-1.6.4.zip
# pip install git+https://github.com/simplejson/simplejson.git
# pip install svn+ssh://svn.zope.org/repos/main/zope.interface/trunk/

```

##### II. 利用 pip 移除套件

`pip` 相較於 `easy_install` 來說，支援較多自動化清理的工作，後續不需再用人工清理殘留檔案。

請於命令列模式下輸入下列指令：

```
# pip uninstall [套件名稱]

```

##### III. 利用 pip 列出所有已安裝的套件

請於命令列模式下輸入下列指令：

```
# pip freeze
BeautifulSoup==3.1.0.1
CDDB==1.4
CherryPy==3.1.2
...

```

##### IV. 利用 pip 升級套件

請於命令列模式下輸入下列指令：

```
# pip install -U [套件名稱]

```

##### V. 利用 pip 搜尋可安裝或管理的套件

請於命令列模式下輸入下列指令：

```
# pip search [關鍵字]

```

##### VI. 列出使用範例

欲列出`pip` 的使用範例，可於命令列模式下輸入下列指令：

```
# pip help

```

##### VII. 利用 pip 封裝相關套件

`pip` 支援將相關套件封裝在一個檔案中，未來這個檔案可以方便地複製到別的環境中執行。

以套件 `vimpyre` 為例，其所需的套件約有 3 至 4 個。此時可於命令列模式下輸入下列指令：

```
# pip bundle vimpyre.pybundle vimpyre

```

如此會將相關套件封裝在 vimpyre.pybundle 檔案中。

事後在另一個環境中，可於命令列模式下輸入下列指令來安裝：

```
# pip install vimpyre.pybundle

```

### 在虛擬環境練習安裝 python 套件

若想測試 python 的套件，卻又不想直接裝到系統預設 python packages 裡；或者不是系統管理者，只能在自己目錄下安裝自己的 python 套件時，可以使用 `virtualenv` 來達到目的。

#### 01\. 安裝 virtualenv

```
# pip install virtualenv

```

#### 02\. 建立一個虛擬環境

```
$ virtualenv my_python_env

```

#### 03\. 初始化虛擬環境

```
$ source ./my_python_env/bin/activate

```

需要注意的是，之後在不同的 shell 環境下，或者在新的視窗操作時，都要重新執行上述指令。

#### 04\. 安裝套件於 virtualenv 中

```
$ pip install vimpyre

```

接著可以在 ./my_python_env/lib/python(版號)/site-packages 發現剛剛安裝的 vimpyre 及其相關套件。

### 結語

建議使用者在管理套件時，以 `pip` 為主，`easy_install` 為輔，因為 `pip` 仍持續開發中，有時候會發現某些套件無法使用 `pip` 安裝，但是卻可以用 `easy_install` 安裝。

如果使用 `pip` 及 `easy_install` 都無法安裝成功時。使用者不妨直接進入套件目錄，看該套件是否有提供 `setup.py` 檔案。請於命令列模式下輸入下列指令：

```
$ python setup.py

```

