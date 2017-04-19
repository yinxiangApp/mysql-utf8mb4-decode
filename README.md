# mysql-utf8mb4-decode
ここは、MySQLデータベースに保存しているencodeされた絵文字と特殊コードの文字を表示させられる。

## utf8mb4は一体どういうことですか？

下記リンクを参照してください。

    http://tmtms.hatenablog.com/entry/2016/09/06/mysql-utf8

## 上記によって、mysqlに「urldecode」というメソッドを用意してください。
    CREATE DEFINER=`root`@`localhost` FUNCTION `urldecode`(s VARCHAR(4096)  CHARSET utf8mb4 ) RETURNS varchar(4096) CHARSET   utf8mb4
    DETERMINISTIC
    BEGIN
       DECLARE c VARCHAR(4096) DEFAULT '';
       DECLARE pointer INT DEFAULT 1;
       DECLARE s2 VARCHAR(4096) DEFAULT '';
       DECLARE h3 VARCHAR(4096) DEFAULT '';
       declare exit handler for sqlexception return UNHEX('');

       IF ISNULL(s) THEN
          RETURN NULL;
       ELSE
       SET s2 = '';
       WHILE pointer <= LENGTH(s) DO
          SET c = SUBSTR(s,pointer,1);
          IF c = '+' THEN
             SET h3 = '20';
          ELSEIF c = '%' AND pointer + 2 <= LENGTH(s) THEN
                   SET h3 = UPPER(SUBSTR(s,pointer+1,2));
                   SET pointer = pointer + 2;
          ELSE
                   SET h3 = HEX(c);
          END IF;
          SET s2 = CONCAT(s2,h3);
          SET pointer = pointer + 1;
       END while;
       END IF;
       RETURN UNHEX(s2);
    END

## SQL Client
    set names utf8mb4; /* change connection charset to utf8mb4 */
    SELECT URLDECODE( convert('%E5%AD%99%E6%BD%8D%E6%BB%A8%F0%9F%A4%95' using utf8mb4))

最終結果を下記オンラインツールによって、照合できる。

    http://www.urlencoder.urlencode.in/url/decoder/convert-string-to-date-java-mysql-url-decode
