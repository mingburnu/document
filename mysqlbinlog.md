> find  / -name mysqlbinlog -print

> /www/server/mysql/bin/mysqlbinlog  --start-datetime="2010-04-01 10:40:00"   --stop-datetime="2017-12-24 00:00:00" /www/server/data/mysql-bin.000040 > /tmp/mysql_restore.sql

> /www/server/mysql/bin/mysqlbinlog /www/server/data/mysql-bin.000012 > /tmp/statement.sql

> show binlog events limit 0,20
