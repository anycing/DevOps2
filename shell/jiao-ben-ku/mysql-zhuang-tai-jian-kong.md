# MySQL 状态监控

### PHP 连接 MySQL

#### PHP 环境准备

```bash
yum remove php-mysql

yum install php-mysqlnd php -y
```

#### test\_connect\_mysql.php

```php
<?php
$mysql_id=mysql_connect("localhost","root","NewPass0.0") or mysql_error();
if ($mysql_id){
  echo "MySQL connect successfully! ";
}else{
  echo mysql_error();
  exit(1);
}
```

#### mysql\_monitor.sh

```bash
#!/bin/bash

/usr/bin/php test_connect_mysql.php

if [ "$?" -eq 0 ]; then
  echo "MySQL is running."
else
  echo "MySQL is stopped."
fi
```



### python3 连接 MySQL

#### python3 环境准备

```bash
# 安装 python3
yum install python3 python3-devel python3-pip -y

# 安装 pymysql 库
pip3 install pymysql
```

#### test\_connect\_mysql.py

```python
import pymysql

db = pymysql.connect(
    host="localhost",
    port=3306,
    user="root",
    password="NewPass0.0",
    db="mysql",
    charset="utf8"
)

cursor=db.cursor()

cursor.execute('select version()')

data=cursor.fetchone()

print("数据库连接正常，版本是：%s"%data)

db.close()
```

#### mysql\_monitor.sh

```bash
#!/bin/bash

/usr/bin/python3 test_connect_mysql.py

if [ "$?" -eq 0 ]; then
  echo "MySQL is running."
else
  echo "MySQL is stopped."
fi
```

