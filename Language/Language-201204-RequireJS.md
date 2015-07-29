# 初探 RequireJS
作者：Jace Ju，2012 年 4 月投稿

一直以來，我們都習慣使用 `script` 這個 HTML 標籤來載入 JavaScript 檔案。這種方式有兩種缺點：

*   無法在 JavaScript 程式中直接管理相依性，必須在 HTML 中處理。

*   雖然目前新式瀏覽器已經能夠以非同步的方式來載入 js 檔案，但是舊型瀏覽器還是會有阻塞 (blocking) 問題。

終於 [CommonJS](http://www.commonjs.org/) 提出了 [AMD](http://wiki.commonjs.org/wiki/Modules/AsynchronousDefinition) 這個 API 規範，用以讓我們的 JavaScript 程式可以模組化，並同時解決 js 檔案載入時的阻塞問題。

目前已經有許多實作 AMD 規範的 JavaScript Library 了，而 [RequireJS](http://requirejs.org/) 則是目前討論最多，應用最廣的其中一個實作。

以下是我在研究 RequireJS 時的筆記，若有謬誤還請大家指正。

### 起手式

先來看看我們的程式目錄架構：

```

├── index.html
├── js
│   ├── app.js
│   └── main.js
└── lib
    ├── backbone
    │   ├── backbone-min.js
    │   └── wrapper.js
    ├── jquery
    │   ├── jquery-min.js
    │   └── wrapper.js
    ├── underscore
    │   ├── underscore-min.js
    │   └── wrapper.js
    └── requirejs
        ├── order.js
        └── require.js

```

其中 index.html 的內容如下：

index.html

```
1 ＜!doctype html＞  
2 ＜html lang="en"＞  
3 ＜head＞  
4 ＜meta charset="utf-8" /＞  
5 ＜title＞Beginning Require.JS＜/title＞  
6 ＜script data-main="js/main"   src="/lib/requirejs/require.js"＞＜/script＞  
7 ＜/head＞  
8 ＜body＞  
9 ＜/body＞  
10 ＜/html＞ 
```

你會發現，我們只需要用一個 script 標籤來載入 `lib/requirejs/require.js` 即可，剩下的 js 檔案都可以讓 RequireJS 來幫我們載入。

可是 RequireJS 怎麼知道要載入哪些檔案呢？注意 script 標籤上的 `data-main` 屬性，它指向了 `js/main.js` (可以將 js 副檔名省略) 。在 js/main.js 中，我們就可以指定我們要載入的模組：

js/main.js

```
1 require([
2  '../lib/jquery/jquery-min'
3 ], function () {
4   console.log($);
5 });
```

**所以一切都是從 `js/main.js` 開始執行。**

在 `js/main.js` 裡，我們用到了 `require` 這個 RequireJS 中最主要的 API ，它的基本用法如下：

```
1 require(dependencies, callback); 
```

其中 `dependencies` 的格式必須為陣列， `callback` 則為函式。

`dependencies` 表示我們要載入的 js ，而其路徑則是相對於 `js/main.js` ，而且一樣不需要寫副檔名。

因此在 `dependencies` 中，我們就可以將所有會用到的 js 載入，然後在 `callback` 中撰寫我們真正要處理的程式邏輯。

### 模組化

當然把所有的程式邏輯都寫在 `js/main.js` 的 `callback` 裡面是沒問題的，但那就沒辦法達到我們想要的模組化了。

而 RequireJS 也實作 AMD 所定義的 `define` API 方法，所以我們就可以用它來實現程式的模組化。 define 的 API 如下：


```1 define(id?, dependencies?, factory);```

其中 `id` 格式為字串，代表模組的名稱，可以不寫。如果要寫的話，就必須是相對於 `js/main.js` 的檔案路徑，但不用加上 `js` 副檔名，例如 `../lib/foo` 或 `./js/bar` 。

而 `dependencies` 格式為陣列，作用與 `require` 中的 `dependencies` 相同。一般來說如果我們在 `js/main.js` 中定義好相依性後，這裡可以不需要特別指定。

最後的 `factory` 則為一個工廠方法，它必須回傳一個物件，也就是我們的模組。

接著我們把原來 `require` API 中的 `callback` 改成模組，並將它放到 `js/app.js` 中：

js/app.js

| 

<pre>1
2
3
4
5
6
7
</pre>

 | 

<pre>define(function () {
  return {
    initialize: function () {
      console.log($);
    }
  }
});
</pre>

 |

`js/app.js` 會回傳一個包含 `initialize` 方法的物件模組，而這個方法就是我們前面的 `callback` 。注意這個例子裡並沒有設定模組的 `id` 。

接下來我們把 `js/app.js` 加到 `require` 的第一個參數中，特別注意這裡的 `app` 是指 `js/app.js` ，而不是模組名稱。

js/main.js

| 

<pre>1
2
3
4
5
6
</pre>

 | 

<pre>require([
  'app',
  '../lib/jquery/jquery-min'
], function (App) {
  App.initialize();
});
</pre>

 |

在 `callback` 的第一個參數 `App` 會對應到 `js/app.js` 中所回傳的物件，這意謂著我們可以為該物件指定新的 namespace 。

到這裡其實可以應付很多基本的應用了，不過如果當 library 間有相依性問題時，這樣的寫法就可能會出錯了。

## 順序問題

因為使用非同步的載入方式，所以用 `require` 載入套件時，是有可能會造成相依性上的問題。 所幸 RequireJS 提供了一個 `order` plugin ，讓我們可以依序載入正確的套件。

以 Backbone 為例，我們需要依序載入 jQuery 、 underscore 及 Backbone 等三個套件，方法如下：

js/main.js

| 

<pre>1
2
3
4
5
6
7
8
</pre>

 | 

<pre>require([
  'app',
  '../lib/requirejs/order!../lib/jquery/jquery-min',
  '../lib/requirejs/order!../lib/underscore/underscore-min',
  '../lib/requirejs/order!../lib/backbone/backbone-min'
], function (App) {
  App.initialize();
});
</pre>

 |

如上面的範例所示，在每一行載入 js 的字串中，我們先載入 plugin ，然後利用 `!` 符號來將 library 的位置傳給 plugin 。

其他有用的 plugin ，可以在官方網站的 [Plugins](http://requirejs.org/docs/download.html#plugins) 頁找到。

## 路徑別名

不過每次都要輸入這麼長的路徑實在是很麻煩，還好 RequireJS 也提供了 `paths` 讓我們設定路徑的別名，就不需要輸入這麼多字：

js/main.js

| 

<pre>1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
</pre>

 | 

<pre>require({
  paths: {
    "order": "../lib/requirejs/order",
    "lib": "../lib"
  }
});

require([
  'app',
  'order!lib/jquery/jquery-min',
  'order!lib/underscore/underscore-min',
  'order!lib/backbone/backbone-min'
], function (App) {
  App.initialize();
});
</pre>

 |

要特別注意的是，這裡設定的別名，也會影響到其他模組裡所使用的 `define` API 。

`require` 還有其他設定，請參考官方文件的 [Configuration Options](http://requirejs.org/docs/api.html#config) 。

## Namespace

前面提到 `require` API 可以讓我們對載入的 js 檔案所回傳的模組物件做 namespace 對應，也就是上述例子的 `App` 。事實上我們可以針對每個模組都設定一個 namespace ，例如：

js/main.js

| 

<pre>1
2
3
4
5
6
7
8
9
10
11
</pre>

 | 

<pre>require([
  '../lib/a',
  '../lib/b',
  '../lib/c',
  '../lib/d'
], function (moduleA, moduleB, moduleC) {
  moduleA.doSomething();
  moduleB.doSomething();
  moduleC.doSomething();
  namespaceD.doSomething();
});
</pre>

 |

可以看到 `'../lib/a'` 這個模組對應到 `moduleA` 這個 namespace ，`'../lib/b'` 則對應到 `moduleB` ，以此類推。

但是 `namespaceD` 並沒有在 `require` 方法的 `callback` 參數中，那為什麼我們可以取用呢？

回到一開始我們用 `require` 載入第三方套件的方式，其實可以看到我們是直接利用該套件定義好的 namespace ，例如 jQuery 的 `$` 符號，或是 underscore.js 的 `_` 符號。

而我們並沒有再為這些套件指定新的 namespace ，是因為這些 namespace 已經被綁在 global 變數裡了 (在瀏覽器環境下是指 window 變數) ，所以我們可以直接取用。

所以 `namespaceD` 其實就是 `lib/d.js` 裡已經定義好的，例如：

lib/d.js

| 

<pre>1
2
3
4
5
6
7
8
</pre>

 | 

<pre>define(function () {
  var namespaceD = window.namespaceD = {
    doSomething: function () {
      console.log('namespaceD.doSomething()');
    }
  };
  return namespaceD;
});
</pre>

 |

瞭解這個回到我們前面所提到的 Backbone.js 範例，有些文章的例子會教大家這麼用：

js/main.js

| 

<pre>1
2
3
4
5
6
7
8
9
10
11
</pre>

 | 

<pre>require([
  'order!lib/jquery/jquery-min',
  'order!lib/underscore/underscore-min',
  'order!lib/backbone/backbone-min',
  'order!app'
], function ($, _, Backbone, App) {
  console.log($);
  console.log(_);
  console.log(Backbone);
  console.log(App);
});
</pre>

 |

如果各位是使用 Underscore.js 及 Backbone.js 的官方版本時，這樣做是錯誤的，你會發現 `callback` 裡的 `_`, `Backbone` 都會是 null 值。為什麼呢？主要是因為這兩個套件目前不支援 AMD 架構，所以無法正確回傳對應的 Underscore.js 及 Backbone.js 物件回來。所以很多人在透過 RequireJS 使用這兩個套件時，就會在這裡卡關。

最簡單的方式就是不要再為這些第三方套件設定一個 namespace ，也就是一開始為大家介紹的用法。

另一種方式就是直接使用 RequireJS 所 fork 出來的 [Underscore.js](https://github.com/amdjs/underscore) 及 [Backbone.js](https://github.com/amdjs/backbone) 的 AMD 版本。

還有一種方法是為這些套件的官方版本定義一個 `wrapper` ，以 Underscore.js 為例：

lib/underscore/wrapper.js

| 

<pre>1
2
3
4
5
</pre>

 | 

<pre>define([
  'lib/underscore/underscore-min'
], function(){
  return _.noConflict();
});
</pre>

 |

這樣在 `js/main.js` 裡就可以重新使用 Underscore.js 的 namespace 了，例如：

js/main.js

| 

<pre>1
2
3
4
5
</pre>

 | 

<pre>require([
  'order!lib/underscore/wrapper'
], function (_) {
  console.log(_);
})
</pre>

 |

不過因為非同步載入的關係，要使用 `wrapper` 方法處理套件相依性時，其流程就稍微複雜些了，大家可以參考 [Organizing your application using Modules (require.js)](http://backbonetutorials.com/organizing-backbone-using-modules/) 一文的介紹。

## 編譯

當我們把 JavaScript 拆成這麼多模組檔案後，那麼不就會讓 HTTP Request 變多了嗎？有沒有什麼方法可以幫我們把這些檔案再組合成為一支檔案呢？

RequireJS 就提供了一個好用的工具，叫做 `r.js` 。它必須透過 node.js 的套件管理系統來安裝，也就是 `npm` ；安裝方法如下：

```
npm -g install requirejs

```

若無錯誤的話，應該會出現以下畫面：

```
npm http GET https://registry.npmjs.org/requirejs
npm http 200 https://registry.npmjs.org/requirejs
npm http GET https://registry.npmjs.org/requirejs/-/requirejs-1.0.7.tgz
npm http 200 https://registry.npmjs.org/requirejs/-/requirejs-1.0.7.tgz
/usr/local/bin/r.js -> /usr/local/lib/node_modules/requirejs/bin/r.js
requirejs@1.0.7 /usr/local/lib/node_modules/requirejs

```

`r.js` 會幫我們處理以 `require` 或 `define` 所定義的模組，再參照其相依性把所有檔案合併為單一的 JavaScript 檔案。用法如下：

```
r.js -o name=js/main out=js/main-built.js baseUrl=. paths.order="lib/requirejs/order"

```

其中 `-o` 為最佳化； `name` 則為要處理的 JavaScript 檔案； `out` 則是輸出的檔案名稱； `baseUrl` 為指定 `r.js` 在處理相依性時所要參考的相對路徑； `paths.order` 是路徑別名，但不相對於 `js/main.js` ，而是相對於 `baseUrl` 。

處理完成後，我們就可以直接改用以下方式載入：

index.html

| 

<pre>1
</pre>

 | 

```
＜script data-main="js/main-built" src="lib/requirejs/require.js"＞＜/script＞
```

 |

當然聰明如你，應該想到該怎麼讓開發和上線環境使用不同的 JavaScript 檔案了吧？

## 心得

以往在寫 JavaScript 時，雖然都會儘可能模組化，但變數的管理還有程式拆解不易的狀況，都是自己在維護 JavaScript 程式時很大的痛處。

在瞭解 RequireJS 的強大後，我相信以這樣的模組化方式再搭配 Backbone.js 的架構，一定可以讓系統在開發與維護上更為有組織性。

## 參考

*   [Organizing your application using Modules (require.js)](http://backbonetutorials.com/organizing-backbone-using-modules/)
*   [AMD 規範：簡單而優雅的動態載入 JavaScript 代碼](http://blog.csdn.net/dojotoolkit/article/details/6076668)
*   [Javascript 文件加載： LABjs 和 RequireJS](http://www.ruanyifeng.com/blog/2011/10/javascript_loading.html)