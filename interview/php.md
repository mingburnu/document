# 問題與簡答

## PHP 篇

### echo、print、print_r、var_dump 區別

> `echo`和`print`是語言結構、`print_r`和`var_dump`是普通函數

- echo：輸出一个或多个字符串

- print：輸出字符串

- print_r：打印關於變量的易於理解的信息

- var_dump：打印關於變量的易於理解的信息(帶類型)

拓展閲讀 [《echo、print、print_r、var_dump區別》](./03.echo、print、print_r、var_dump區別.md)

### 單引號和雙引號的區別

雙引號可以被分析器解析，單引號則不行

###### 詳細

0. 單引號不能解析變量 
1. 單引號不能解析轉義字符,只能解析單引號和反斜綫本身
2. 變量和變量,變量和字符串,字符串和字符串之間可以用.連接 
3. 雙引號可以解析變量,變量可以使用特殊字符和{}包含
4. 雙引號可以解析所有轉義字符
5. 也可以使用.來連接
最重要的是單引號比雙引號效率高


### isset 和 empty 的區別

isset：檢測變量是否已設置幷且非 NULL

empty：判斷變量是否為空，變量為 0/false 也會被認為是空；變量不存在，不會産生警告

### static、self、$this 的區別

static：static 可以用於靜態或非靜態方法中，也可以訪問類的靜態屬性、靜態方法、常量和非靜態方法，但不能訪問非靜態屬性

self：可以用於訪問類的靜態屬性、靜態方法和常量，但 self 指向的是當前定義所在的類，這是 self 的限制

$this：指向的是實際調用時的對象，也就是説，實際運行過程中，誰調用了類的屬性或方法，$this 指向的就是哪个對象。但 $this 不能訪問類的靜態屬性和常量，且 $this 不能存在於靜態方法中

### include、require、include_once、require_once 的區別

require 和 include 幾乎完全一樣，除了處理失敗的方式不同之外。require 在出錯時産生 E_COMPILE_ERROR 級別的錯誤。換句話説將導致腳本中止而 include 只産生警告（E_WARNING），腳本會繼續運行

include_once 語句在腳本執行期間包含幷運行指定文件。此行為和 include 語句類似，唯一區別是如果該文件中已經被包含過，則不會再次包含。如同此語句名字暗示的那樣，只會包含一次

### 常見數組函數

array_count_values — 統計數組中所有的値

array_flip — 交換數組中的鍵和値

array_merge — 合幷一个或多个數組

array_multisort — 對多个數組或多維數組進行排序

array_pad — 以指定長度將一个値塡充進數組

array_pop — 彈出數組最後一个單元(出棧)

array_push — 將一个或多个單元壓入數組的末尾(入棧)

array_rand — 從數組中隨機(僞隨機)取出一个或多个單元

array_keys — 返回數組中部分的或所有的鍵名

array_values — 返回數組中所有的値

count — 計算數組中的單元數目，或對象中的屬性个數

sort — 對數組排序

### Cookie 和 Session

Cookie：PHP 透明的支持 HTTP cookie 。cookie 是一種遠程瀏覽器端存儲數據幷以此來跟蹤和識別用戶的機制

Session：會話機制(Session)在 PHP 中用於保持用戶連續訪問Web應用時的相關數據

### 預定義變量

對於全部腳本而言，PHP 提供了大量的預定義變量

超全局變量 — 超全局變量是在全部作用域中始終可用的內置變量

```text
$GLOBALS — 引用全局作用域中可用的全部變量
$_SERVER — 服務器和執行環境信息
$_GET — HTTP GET 變量
$_POST — HTTP POST 變量
$_FILES — HTTP 文件上傳變量
$_REQUEST — HTTP Request 變量
$_SESSION — Session 變量
$_ENV — 環境變量
$_COOKIE — HTTP Cookies
$php_errormsg — 前一个錯誤信息
$HTTP_RAW_POST_DATA — 原生POST數據
$http_response_header — HTTP 響應頭
$argc — 傳遞給腳本的參數數目
$argv — 傳遞給腳本的參數數組
```

- 超全局變量

PHP 中的許多預定義變量都是“超全局的”，這意味著它們在一个腳本的全部作用域中都可用。在函數或方法中無需執行 global $variable; 就可以訪問它們

超全局變量：$GLOBALS、$\_SERVER、$\_GET、$\_POST、$\_FILES、$\_COOKIE、$\_SESSION、$\_REQUEST、$\_ENV

### 傳値和傳引用的區別

傳値導致對象生成了一个拷貝，傳引用則可以用兩个變量指向同一个內容

### 構造函數和析構函數

構造函數：PHP 5 允行開發者在一个類中定義一个方法作為構造函數。具有構造函數的類會在每次創建新對象時先調用此方法，所以非常適合在使用對象之前做一些初始化工作

析構函數：PHP 5 引入了析構函數的槪念，這類似於其它面向對象的語言，如 C++。析構函數會在到某个對象的所有引用都被删除或者當對象被顯式銷毁時執行

### 魔術方法

\_\_construct()， \_\_destruct()， \_\_call()， \_\_callStatic()， \_\_get()， \_\_set()， \_\_isset()， \_\_unset()， \_\_sleep()， \_\_wakeup()， \_\_toString()， \_\_invoke() 等方法在 PHP 中被稱為"魔術方法"（Magic methods）

### public、protected、private、final 區別

對屬性或方法的訪問控制，是通過在前面添加關鍵字 public（公有），protected（受保護）或 private（私有）來實現的。被定義為公有的類成員可以在任何地方被訪問

PHP 5 新增了一个 final 關鍵字。如果父類中的方法被聲明為 final，則子類無法覆蓋該方法。如果一个類被聲明為 final，則不能被繼承

### 客戶端/服務端 IP 獲取，了解代理透傳 實際IP 的槪念

客戶端IP: $\_SERVER['REMOTE_ADDR']

服務端IP: $\_SERVER['SERVER_ADDR']

客戶端IP(代理透傳): $\_SERVER['HTTP_X_FORWARDED_FOR']

### 類的靜態調用和實例化調用

- 占用內存

靜態方法在內存中只有一份，無論調用多少次，都是共用的

實例化不一樣，每一个實例化是一个對象，在內存中是多个的

- 不同點

靜態調用不需要實例化即可調用

靜態方法不能調用非靜態屬性，因為非靜態屬性需要實例化後，存放在對象裏

靜態方法可以調用非靜態方法，使用 self 關鍵字。php 裏，一个方法被 `self::` 後，自動轉變為靜態方法

調用類的靜態函數時不會自動調用類的構造函數

### 接口和抽象的區別
抽象用於描述不同的事物，接口用於描述事物的行為。

### PHP 不實例化調用方法

靜態調用、使用 PHP 反射方式

### php.ini 配置選項

- 配置選項

|名字|默認|備注|
|-|-|-|
|short_open_tag|"1"|是否開啓縮寫形式(`<? ?>`)|
|precision|"14"|浮點數中顯示有效數字的位數|
|disable_functions|""|禁止某些函數|
|disable_classes|""|禁用某些類|
|expose_php|""|是否暴露 PHP 被安裝在服務器上|
|max_execution_time|30|最大執行時間|
|memory_limit|128M|每个腳本執行的內存限制|
|error_reporting|NULL|設置錯誤報告的級別 `E_ALL` & ~`E_NOTICE` & ~`E_STRICT` & ~`E_DEPRECATED`|
|display_errors|"1"|顯示錯誤|
|log_errors|"0"|設置是否將錯誤日志記錄到 error_log 中|
|error_log|NULL|設置腳本錯誤將被記錄到的文件|
|upload_max_filesize|"2M"|最大上傳文件大小|
|post_max_size|"8M"|設置POST最大數據限制|

```shell
php -ini | grep short_open_tag //查看 php.ini 配置
```

- 動態設置

```php
ini_set(string $varname , string $newvalue);

ini_set('date.timezone', 'Asia/Shanghai'); //設置時區
ini_set('display_errors', '1'); //設置顯示錯誤
ini_set('memory_limit', '256M'); //設置最大內存限制
```

### php-fpm.conf 配置

|名稱|默認|備注|
|-|-|-|
|pid||PID文件的位置|
|error_log||錯誤日志的位置|
|log_level|notice|錯誤級別 alert:必須立即處理、error:錯誤情況、warning:警告情況、notice:一般重要信息、debug:調試信息|
|daemonize|yes|設置 FPM 在後臺運行|
|listen|ip:port、port、/path/to/unix/socket|設置接受 FastCGI 請求的地址|
|pm|static、ondemand、dynamic|設置進程管理器如何管理子進程|
|request_slowlog_timeout|'0'|慢日志記錄閥値|
|slowlog||慢請求的記錄日志|

### 502、504 錯誤産生原因及解決方式

#### 502

502 表示網關錯誤，當 PHP-CGI 得到一个無效響應，網關就會輸出這个錯誤

- `php.ini` 的 memory_limit 過小
- `php-fpm.conf` 中 max_children、max_requests 設置不合理
- `php-fpm.conf` 中 request_terminate_timeout、max_execution_time 設置不合理
- php-fpm 進程處理不過來，進程數不足、腳本存在性能問題

#### 504

504 表示網關超時，PHP-CGI 沒有在指定時間響應請求，網關將輸出這个錯誤

- Nginx+PHP 架構，可以調整 FastCGI 超時時間，fastcgi_connect_timeout、fastcgi_send_timeout、fastcgi_read_timeout

#### 500

php 代碼問題，文件權限問題，資源問題

#### 503

超載或者停機維護

### 如何返回一个301重定向

```php
header('HTTP/1.1 301 Moved Permanently');
header('Location: https://blog.maplemark.cn');
```

### PHP 與 MySQL 連接方式

#### MySQL

```php
$conn = mysql_connect('127.0.0.1:3306', 'root', '123456');
if (!$conn) {
    die(mysql_error() . "\n");
}
mysql_query("SET NAMES 'utf8'");
$select_db = mysql_select_db('app');
if (!$select_db) {
    die(mysql_error() . "\n");
}
$sql = "SELECT * FROM `user` LIMIT 1";
$res = mysql_query($sql);
if (!$res) {
    die(mysql_error() . "\n");
}
while ($row = mysql_fetch_assoc($res)) {
    var_dump($row);
}
mysql_close($conn);
```

#### MySQLi

```php
$conn = @new mysqli('127.0.0.1:3306', 'root', '123456');
if ($conn->connect_errno) {
    die($conn->connect_error . "\n");
}
$conn->query("set names 'utf8';");
$select_db = $conn->select_db('user');
if (!$select_db) {
    die($conn->error . "\n");
}
$sql = "SELECT * FROM `user` LIMIT 1";
$res = $conn->query($sql);
if (!$res) {
    die($conn->error . "\n");
}
while ($row = $res->fetch_assoc()) {
    var_dump($row);
}
$res->free();
$conn->close();
```

#### PDO

```php
$pdo = new PDO('mysql:host=127.0.0.1:3306;dbname=user', 'root', '123456');
$pdo->exec("set names 'utf8'");
$sql = "SELECT * FROM `user` LIMIT 1";
$stmt = $pdo->prepare($sql);
$stmt->bindValue(1, 1, PDO::PARAM_STR);
$rs = $stmt->execute();
if ($rs) {
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        var_dump($row);
    }
}
$pdo = null;
```

### MySQL、MySQLi、PDO 區別

#### MySQL

- 允許 PHP 應用與 MySQL 數據庫交互的早期擴展
- 提供了一个面向過程的接口，不支持後期的一些特性

#### MySQLi

- 面向對象接口
- prepared 語句支持
- 多語句執行支持
- 事務支持
- 增強的調試能力

#### PDO

- PHP 應用中的一个數據庫抽象層規範
- PDO 提供一个統一的 API 接口，無須關心數據庫類型
- 使用標準的 PDO API，可以快速無縫切換數據庫

### 數據庫持久連接

把 PHP 用作多進程 web 服務器的一个模塊，這種方法目前只適用於 Apache。

對於一个多進程的服務器，其典型特徵是有一个父進程和一組子進程協調運行，其中實際生成 web 頁面的是子進程。每當客戶端向父進程提出請求時，該請求會被傳遞給還沒有被其它的客戶端請求占用的子進程。這也就是説當相同的客戶端第二次向服務端提出請求時，它將有可能被一个不同的子進程來處理。在開啓了一个持久連接後，所有請求 SQL 服務的後繼頁面都能够重用這个已經建立的 SQL Server 連接。

### 代碼執行過程

PHP 代碼 => 啓動 php 及 zend 引擎，加載注册拓展模塊 => 對代碼進行詞法/語法分析 => 編譯成opcode(opcache) => 執行 opcode

> PHP7 新增了抽象語法樹(AST)，在語法分析階段生成 AST，然後再生成 opcode 數組

### base64 編碼原理

![base64](./assets/php-base64.png)

### ip2long 實現

![ip2long](./assets/php-ip2long.png)


```
124.205.30.150=2093817494

list($p1,$p2,$p3,$p4) = explode('.','124.205.30.150');

$realNum = ($p1<<24)+($p2<<16)+($p3<<8)+$p4;
```

### MVC 的理解

MVC 包括三類對象。模型 Model 是應用對象，視圖 View 是它在屛幕上的表示，控制器 Controller 定義用戶界面對用戶輸入的響應方式。不使用 MVC，用戶界面設計往往將這些對象混在一起，而 MVC 則將它們分離以提高靈活性和復用性

### 主流 PHP 框架特點

#### Laravel

易於訪問，功能強大，幷提供大型，強大的應用程序所需的工具

- 簡單快速的路由引擎
- 強大的依賴注入容器
- 富有表現力，直觀的數據庫 ORM
- 提供數據庫遷移功能
- 靈活的任務調度器
- 實時事件廣播

#### Symfony

- Database engine-independent
- Simple to use, in most cases, but still flexible enough to adapt to complex cases
- Based on the premise of convention over configuration--the developer needs to configure only the unconventional
- Compliant with most web best practices and design patterns
- Enterprise-ready--adaptable to existing information technology (IT) policies and architectures, and stable enough for long-term projects
- Very readable code, with phpDocumentor comments, for easy maintenance
- Easy to extend, allowing for integration with other vendor libraries

#### CodeIgniter

- 基於模型-視圖-控制器的系統
- 框架比較輕量
- 全功能數據庫類，支持多个平臺
- Query Builder 數據庫支持
- 表單和數據驗證
- 安全性和 XSS 過濾
- 全頁面緩存

#### ThinkPHP

- 采用容器統一管理對象
- 支持 Facade
- 更易用的路由
- 注解路由支持
- 路由跨域請求支持
- 驗證類增強
- 配置和路由目錄獨立
- 取消系統常量
- 類庫別名機制
- 模型和數據庫增強
- 依賴注入完善
- 支持 PSR-3 日志規範
- 中間件支持
- 支持 Swoole/Workerman 運行

### 對象關繫映射/ORM

#### 優點

- 縮短編碼時間、減少甚至免除對 model 的編碼，降低數據庫學習成本
- 動態的數據表映射，在表結構發生改變時，減少代碼修改
- 可以很方便的引入附加功能(cache 層)

#### 缺點

- 映射消耗性能、ORM 對象消耗內存
- SQL 語句較為復雜時，ORM 語法可讀性不高(使用原生 SQL)

### 鏈式調用實現

類定義一个內置變量，讓類中其他定義方法可訪問到

### 異常處理

set_exception_handler — 設置用戶自定義的異常處理函數

使用 try / catch 捕獲

### 串行、幷行、幷發的區別
串行：執行多个任務時，各个任務按順序執行，完成一个之後才能進行下一个
幷行：多个任務在同一時刻執行
幷發：同一時刻需要執行多个任務

### 同步與異步的理解
**同步和異步是一種消息通信機制**。其關注點在於 `被調用者返回` 和 `結果返回` 之間的關繫，描述對象是被調用對象的行為。

### 阻塞與非阻塞的理解
**阻塞和非阻塞是一種業務流程處理方式**。其關注點在於調用發生時 `調用者狀態` 和 `被調用者返回結果` 之間的關繫，描述對象是等待結果時候調用者的狀態。

### 同步阻塞與非同步阻塞的理解
同步阻塞：打電話問老闆有沒有某書（調用），老闆説查一下，讓你別挂電話（同步），你一直等待老闆給你結果，什麽事也不做（阻塞）。

同步非阻塞：打電話問老闆有沒有某書（調用），老闆説查一下，讓你別挂電話（同步），等電話的過程中你還一邊嗑瓜子（非阻塞）。

### 異步阻塞與異步非阻塞的理解
異步阻塞：打電話問老闆有沒有某書（調用），老闆説你先挂電話，有了結果通知你（異步），你挂了電話後（結束調用）, 除了等老闆電話通知結果，什麽事情也不做（阻塞）。

異步非阻塞：打電話問老闆有沒有某書（調用），老闆説你先挂電話，有了結果通知你（異步），你挂電話後（結束調用），一遍等電話，一遍嗑瓜子。（非阻塞）

### 如何實現異步調用

```php
$fp = fsockopen("blog.maplemark.cn", 80, $errno, $errstr, 30);
if (!$fp) {
    echo "$errstr ($errno)<br />\n";
} else {
    $out = "GET /backend.php  / HTTP/1.1\r\n";
    $out .= "Host: blog.maplemark.cn\r\n";
    $out .= "Connection: Close\r\n\r\n";
    fwrite($fp, $out);
    /*忽略執行結果
    while (!feof($fp)) {
        echo fgets($fp, 128);
    }*/
    fclose($fp);
}
```

### 多進程同時寫一个文件

加鎖、隊列

### PHP 進程模型，進程通訊方式，進程綫程區別

消息隊列、socket、信號量、共享內存、信號、管道

### PHP 支持回調的函數，實現一个

array_map、array_filter、array_walk、usort

is_callable + callbacks + 匿名函數實現

### 發起 HTTP 請求有哪幾種方式，它們有何區別

cURL、file_get_contents、fopen、fsockopen

### php for while foreach 迭代數組時候，哪个效率最高

### 弱類型變量如何實現

PHP 中聲明的變量，在 zend 引擎中都是用結構體 zval 來保存，通過共同體實現弱類型變量聲明

### PHP 拓展初始化

- 初始化拓展

```shell
$ php /php-src/ext/ext_skel.php --ext
```

- 定義拓展函數

zend_module_entry 定義 Extension name 編寫 PHP_FUNCTION 函數

- 編譯安裝

```shell
$ phpize $ ./configure $ make && make install
```

### 如何獲取擴展安裝路徑

### 垃圾回收機制

引用計數器

### Trait

自 PHP 5.4.0 起，PHP 實現了一種代碼復用的方法，稱為 trait

### yield 是什麽，説个使用場景 yield、yield 核心原理是什麽

一个生成器函數看起來像一个普通的函數，不同的是普通函數返回一个値，而一个生成器可以yield生成許多它所需要的値。

yield核心原理: PHP在使用生成器的時候，會返回一个Generator類的對象。每一次迭代，PHP會通過Generator實例計算出下一次需要迭代的値。簡述即yield用於生成値。

yield使用場景：讀取大文件、大量計算。

yield好處：節省內存、優化性能

### traits 與 interfaces 區別 及 traits 解決了什麽痛點

### 如何 foreach 迭代對象、如何數組化操作對象 $obj[key]、如何函數化對象 $obj(123);

### Swoole 適用場景，協程實現方式

Swoole 是一个使用 C++ 語言編寫的基於異步事件驅動和協程的幷行網絡通信引擎，為 PHP 提供協程、高性能網絡編程支持。提供了多種通信協議的網絡服務器和客戶端模塊，可以方便快速的實現 TCP/UDP服務、高性能Web、WebSocket服務、物聯網、實時通訊、遊戲、微服務等，使 PHP 不再局限於傳統的 Web 領域。


協程可以簡單理解為綫程，只不過這个綫程是用戶態的，不需要操作系統參與，創建銷毁和切換的成本非常低，和綫程不同的是協程沒法利用多核 cpu 的，想利用多核 cpu 需要依賴 Swoole 的多進程模型。
在底層實現上是單綫程的，因此同一時間只有一个協程在工作，協程的執行是串行的。
采用 CSP 編程模型，即不要以共享內存的方式來通信，相反，要通過通信來共享內存。
swoole4.0采用雙棧方式，通過棧楨切換來實現協程；即遇到IO等待就切換到。

#### swoole的進程模型

同一臺主機上兩个進程間通信 (簡稱 IPC) 的方式有很多種，在 Swoole 中使用了 2 種方式 Unix Socket 和 sysvmsg。

swoole啓動後會生成master進程、reactor綫程、worker進程、task進程以及manager進程

master進程是一个多綫程進程，會生成多个reactor綫程
reactor綫程負載網絡監聽、數據收發
work進程處理reactor綫程投遞的請求數據
task進程處理work進程投遞的任務
manager進程用於管理work進程和task進程

### PHP 數組底層實現 （HashTable + Linked list）

PHP 數組底層依賴的散列表數據結構，定義如下（位於 Zend/zend_types.h）。

數據存儲在一个散列表中，通過中間層來保存索引與實際存儲在散列表中位置的映射。

由於哈希函數會存在哈希衝突的可能，因此對衝突的値采用鏈表來保存。

哈希表的查詢效率是o（1），鏈表查詢效率是o（n）；因此PHP數據索引速度很快；但是相對比較占用空間。

### Copy on write 原理，何時 GC

### 如何解決 PHP 內存溢出問題

### ZVAL

### HashTable

### PHP7 新特性

標量類型聲明、返回値類型聲明、通過 define() 定義常量數組、匿名類、相同命名空間類一次性導入

### PHP7 底層優化

ZVAL 結構體優化，占用由24字節降低為16字節

內部類型 zend_string，結構體成員變量采用 char 數組，不是用 char*

PHP 數組實現由 hashtable 變為 zend array

函數調用機制，改進函數調用機制，通過優化參數傳遞環節，減少了一些指令

### PSR 介紹，PSR-1, 2, 4, 7

### Xhprof 、Xdebug 性能調試工具使用

### 字符串、數字比較大小的原理，注意 0 開頭的8進制、0x 開頭16進制

### BOM 頭是什麽，怎麽除去

WINDOWS自帶的記事本，在保存一个以 UTF-8 編碼的文件時，會在文件開始的地方插入三个不可見的字符（0xEF 0xBB 0xBF，即BOM）；它是一串隱藏的字符，用於讓記事本等編輯器識別這个文件是否以UTF-8編碼。
去除方法：$result = trim($result, "\xEF\xBB\xBF");

### 模板引擎是什麽，解決什麽問題、實現原理（Smarty、Twig、Blade）


### 寫一个函數，盡可能高效的從一个標準 URL 中取出文件的擴展名
parse_str,explode
