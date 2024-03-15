# MySQL 分库分表备份脚本

```shell
#!/bin/bash

BASEDIR=/tmp
MYUSER=root
MYPWD=123456
MYSOCK=/usr/mysql/3306/mysql.sock
MYCMD="mysql -u${MYUSER} -p${MYPWD} -S ${MYSOCK}"
MYDUMP="mysqldump -u${MYUSER} -p${MYPWD} -S ${MYSOCK}"

for dbname in $($MYCMD -e "show databases;" | sed "1d" | egrep -v "schema|mysql|sys")
do
  # 创建对应的数据库目录
  mkdir -p ${BASEDIR}/${dbname}_$(date "+%F")

  # 找到数据库中的每一张表进行数据备份
  for table in $($MYCMD -e "show tables from $dbname" | sed "1d")
  do
    $($MYDUMP $dbname $table | gzip > ${BASEDIR}/${dbname}_$(date "+%F")/${table}.sql.gz)
  done

done
```
