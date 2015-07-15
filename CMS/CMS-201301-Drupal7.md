# 行事曆 (Calendar) on Drupal 7
作者：凍仁翔，2013 年 1 月投稿。

先前在建置某網站時曾提過要有行事曆，也想過直接嵌入 [Google Calendar](http://www.google.com/calendar/) 的方案，但對某些使用者而言並不友善，所幸該網站凍仁是建置在 Drupal 上的，只需將 [Calendar](http://drupal.org/project/calendar) 模組設定好即可，毋需再重新造輪子了。

![min-calendar with corolla](http://www.openfoundry.org/images/130129/drupal7/min-calendar_with_corolla.jpg)

套用 Corolla 主題的迷你行事曆 (min-calendar)。

# 若要使用 Calendar 模組記得要連相依的 [Date](http://drupal.org/project/date) 及 [Views](http://drupal.org/project/views) 兩模組也一同補上。

### 1\. 安裝相關模組

1\. 安裝 Calendar 模組。

<pre>[ <span style="color: #ffdb00;">jonny</span><span style="color: red;">@squeeze</span> <span style="color: #ad7fa8;">~</span> ] 
$ sudo drush dl calendar [Enter] 
Project calendar (7.x-3.4) downloaded to /var/www/drupal/sites/all/modules/calendar.
</pre>

2\. 安裝 Date 模組。

<pre>[ <span style="color: #ffdb00;">jonny</span><span style="color: red;">@squeeze</span> <span style="color: #ad7fa8;">~</span> ] 
$ sudo drush dl date [Enter]
Project date (7.x-2.6) downloaded to /var/www/drupal/sites/all/modules/date. [success]
Project date contains 11 modules: date_migrate_example, date_migrate, date_views, date_repeat_field, date_repeat, date_popup, date_api,
date_tools, date_context, date_all_day, date.
</pre>

3\. 安裝 Date Popup Authored 模組。

<pre>[ <span style="color: #ffdb00;">jonny</span><span style="color: red;">@squeeze</span> <span style="color: #ad7fa8;">~</span> ] $ sudo drush dl date_popup_authored [Enter]
Project date_popup_authored (7.x-1.1) downloaded to /var/www/drupal/sites/all/modules/date_popup_authored.</pre>

4\. 啟用 Calendar 模組。

<pre>[ <span style="color: #ffdb00;">jonny</span><span style="color: red;">@squeeze</span> <span style="color: #ad7fa8;">~</span> ] $ sudo drush en calendar [Enter]</pre>

5\. 啟用 Date Popup Authored 模組。

<pre>[ <span style="color: #ffdb00;">jonny</span><span style="color: red;">@squeeze</span> <span style="color: #ad7fa8;">~</span> ] $ sudo drush en date_popup_authored [Enter]
The following extensions will be enabled: date_popup_authored, date_popup
Do you really want to continue? (y/n): y
date_popup was enabled successfully.
date_popup_authored was enabled successfully.</pre>

### 2\. Calendar 應用

#### <span style="color: #ff6600;">2.1\. 新增「以建立日期為基準」的 Calendar View</span>

若今天是想讓所有的文章 (node) 依造建立的時間點顯示在行事曆上，可以使用「A calendar view of the 'created' field in the 'node' base table.」，完成後會如 2.1.6 一般，此方案適合只需紀錄單一時間點的網站，例如：部落格、新聞、雜誌... 等等。

##### <span style="color: #666699;">2.1.1\. 新增 Calendar</span>

1\. 進入 **Views** 介面：首頁 » 管理 » 架構 » Views (http://example.tw/admin/structure/views)。

![2012-12-07-Calendar-01 view](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-01_view.jpg)

2\. 點選 Add view from template (http://example.tw/admin/structure/views/add-template) 。

![2012-12-07-Calendar-02 add view from template](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-02_add_view_from_template.jpg)

3\. 點選「A calendar view of the 'created' field in the 'node' base table.」一項的 <span class="Ctrl">add</span>。

![2012-12-07-Calendar-03 view name](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-03_view_name.jpg)

4\. 設定 Calendar 名稱。

![2012-12-07-Calendar-04 need save](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-04_need_save.jpg)

5\. 點選 儲存 寫入後，再點選 view month 檢視成果。

![2012-12-04 Calendar origin 01](http://www.openfoundry.org/images/130129/drupal7/2012-12-04_Calendar_origin_01.jpg)

6\. **View: Calendar** 新增完成。

##### <span style="color: #666699;">2.1.2\. 新增迷你行事曆 (mini-Calendar) 區塊 (block)</span>

1\. 進入**區塊**介面：首頁 » 管理 » 架構 (http://example.tw/admin/structure/block)。

![2012-12-07-Calendar-05 block](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-05_block.jpg)

2\. 將「View Calendar: Block」設定至首側欄。

![2012-12-04 Calendar origin 02 block](http://www.openfoundry.org/images/130129/drupal7/2012-12-04_Calendar_origin_02_block.jpg)

3\. **View: Calendar: Block** 設定完成。

##### <span style="color: #666699;">2.1.3\. 修改星期翻譯</span>

相信不少伙伴都發現上方的範例都有些小缺陷，那就是週日、一、五及六的日期怪怪的，這時只需自行改翻譯就可解決此問題。

*   週曰 <span style="color: #ff0000;">→</span> 日
*   週一 <span style="color: #ff0000;">→</span> 一
*   週五 <span style="color: #ff0000;">→</span> 五
*   週六 <span style="color: #ff0000;">→</span> 六

1\. 進入**翻譯**介面：首頁 » 管理 » 設定 » 地區與語言 » 介面翻譯 (http://example.tw/admin/config/regional/translate/translate)。

![2012-12-07-Calendar-06 translate](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-06_translate.jpg)

2\. 搜尋**週五**並編輯 Fri。

![2012-12-04 Calendar origin 03 translate](http://www.openfoundry.org/images/130129/drupal7/2012-12-04_Calendar_origin_03_translate.jpg)

3\. 手動更換為**五**。

![2012-12-04 Calendar modified 01](http://www.openfoundry.org/images/130129/drupal7/2012-12-04_Calendar_modified_01.jpg)

4\. 修改後的 **View: Calendar**。

![2012-12-04 Calendar modified 02 block](http://www.openfoundry.org/images/130129/drupal7/2012-12-04_Calendar_modified_02_block.jpg)

5\. 修改後的 **View: Calendar: Block**。

#### <span style="color: #ff6600;">2.2\. 新增「有事件起迄為基準」的 Calendar View</span>

若今天是想表示某個事件 (Event) 且擁有某範圍的時間點，則可以使用「A calendar view of the 'field_event_date' field in the 'node' base table.」 ...

##### <span style="color: #666699;">2.2.1\. 新增內容類型 Event</span>

1\. 進入**內容類型**介面：首頁 » 管理 » 架構 » 內容類型 (http://example.tw/admin/structure/types)。

![2012-12-07-Calendar-07 add types](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-07_add_types.jpg)

2\. 點選 新增內容類型。

![2012-12-07-Calendar-08 add types title](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-08_add_types_title.jpg)

3\. 設定 Event 內容類型的名稱 (Title)。

![2012-12-07-Calendar-09 event types field](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-09_event_types_field.jpg)

4\. 管理 Event 欄位。

![2012-12-07-Calendar-10 event types add field](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-10_event_types_add_field.jpg)

5\. 新增 Event Date 欄位，切忌 **機器可讀名稱 (Machine Name)** 不可重復。

![2012-12-07-Calendar-11 event types collect end date](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-11_event_types_collect_end_date.jpg)

6\. 進入 Event Date 欄位設定，並將「Collect an end date」一項打勾 。

![2012-12-07-Calendar-11 field sort](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-11_field_sort.jpg)

7\. 更改欄位順序並點選 儲存。

##### <span style="color: #666699;">2.2.2\. 新增 Event 內容類型</span>

1\. 進入**內容**介面：首頁 » 管理 (http://example.tw/admin/content)。

![2012-12-07-Calendar-18 types](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-18_types.jpg)

2\. 新增內容。

![2012-12-07-Calendar-15 add event](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-15_add_event.jpg)

3\. 內容類型選擇 Event。

![2012-12-07-Calendar-16 testing rang](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-16_testing_rang.jpg)

4\. 新增有範圍的 Event。

##### <span style="color: #666699;">2.2.3\. 新增 Calendar Event</span>

1\. 進入 Views 介面：首頁 » 管理 » 架構 » Views (http://example.tw/admin/structure/views)。

![2012-12-07-Calendar-01 view](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-01_view.jpg)

2\. 點選 <span class="Ctrl">Add view from</span> template (http://example.tw/admin/structure/views/add-template)。

![2012-12-07-Calendar-12 field event date](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-12_field_event_date.jpg)

3\. 點選「A calendar view of the 'field_event_date_test' field in the 'node' base table.」一項的 add 。

![2012-12-07-Calendar-13 view call name](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-13_view_call_name.jpg)

4\. 設定 Calendar Event 名稱。

![2012-12-07-Calendar-14 need save](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-14_need_save.jpg)

5\. 點選 儲存 寫入後，再點選 view month 檢視成果。

![2012-12-07-Calendar-17 calendar rang](http://www.openfoundry.org/images/130129/drupal7/2012-12-07-Calendar-17_calendar_rang.jpg)

6\. **View: Calendar Event** 新增完成。

### 相關連結：

<div style="margin-left: 2em">★[Calendar Block | drupal.org](http://drupal.org/project/calendar_block)
 ★[Pretty Calendar | drupal.org](http://drupal.org/project/pretty_calendar)
 ★[Drupal Calendar Setup on Vimeo](http://vimeo.com/6544779)
 ★[How to Create a Mini Calendar in Drupal | eHow.com](http://www.ehow.com/how_8677207_create-mini-calendar-drupal.html)

</div>

### 資料來源：

<div style="margin-left: 2em">★[Drupal + Calendar 讓網站擁有行事曆功能 - 玩物尚誌 by lyhcode](http://blog.lyhdev.com/2011/11/drupal-calendar.html)
 ★[Build an Event Calendar in Drupal - slideshare](http://www.slideshare.net/AcquiaInc/build-an-event-calendar-in-drupal)</div>