# mysql-utf8mb4-decode
ここは、MySQLデータベースに保存しているencodeされた絵文字と特殊コードの文字を表示させられる。
問題を解決するとき、資料いっぱい参照したので、整理する。

## utf8mb4は一体どういうことですか？

下記リンクを参照してください。

    http://tmtms.hatenablog.com/entry/2016/09/06/mysql-utf8

## 上記によって、mysqlに「urldecode」というメソッドを用意する。
下記リンクを参照して、「urldecode」というメソッドを追加して、入力と出力パラメタを【utf8mb4】に修正する

   　CREATE DEFINER=`root`@`localhost` FUNCTION `urldecode`(s VARCHAR(4096)) RETURNS varchar(4096) CHARSET  utf8
    
    ↓
    
    CREATE DEFINER=`root`@`localhost` FUNCTION `urldecode`(s VARCHAR(4096)  CHARSET utf8mb4 ) RETURNS varchar(4096) CHARSET   utf8mb4
    

## SQL Clientの注意点
    set names utf8mb4; /* change connection charset to utf8mb4 */
    SELECT URLDECODE( convert('%E5%AD%99%E6%BD%8D%E6%BB%A8%F0%9F%A4%95' using utf8mb4))

最終結果を下記オンラインツールによって、照合できる。

    http://www.urlencoder.urlencode.in/url/decoder/convert-string-to-date-java-mysql-url-decode
