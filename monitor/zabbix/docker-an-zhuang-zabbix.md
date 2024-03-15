# Docker 安装 Zabbix

{% embed url="https://www.zabbix.com/documentation/current/en/manual/installation/containers" %}



### 准备镜像

```
docker pull mysql:8.0

docker pull zabbix/zabbix-server-mysql:latest

docker pull zabbix/zabbix-web-nginx-mysql:latest

docker pull zabbix/zabbix-java-gateway:latest
```



### Docker 部署最新版 Zabbix

#### 1. 创建 Zabbix 专用 Docker 网络

```
docker network create --subnet 172.20.0.0/16 \
                      --ip-range 172.20.240.0/20 zabbix-net
```

#### 2. 启动空的 MySQL 服务器实例

```
docker run --name mysql-server_zabbix -t \
           -e MYSQL_DATABASE="zabbix" \
           -e MYSQL_USER="zabbix" \
           -e MYSQL_PASSWORD="zabbix_pwd" \
           -e MYSQL_ROOT_PASSWORD="root_pwd" \
           --network=zabbix-net \
           --restart unless-stopped \
           -d mysql:8.0 \
           --character-set-server=utf8 --collation-server=utf8_bin \
           --default-authentication-plugin=mysql_native_password
```

#### 3. 启动 Zabbix Java gateway 实例

```
docker run --name zabbix-java-gateway -t \
           --network=zabbix-net \
           --restart unless-stopped \
           -d zabbix/zabbix-java-gateway:latest
```

#### 4. 启动 Zabbix server 实例，并将其关联到已创建的 MySQL server 实例

```
docker run --name zabbix-server-mysql -t \
           -e DB_SERVER_HOST="mysql-server_zabbix" \
           -e MYSQL_DATABASE="zabbix" \
           -e MYSQL_USER="zabbix" \
           -e MYSQL_PASSWORD="zabbix_pwd" \
           -e MYSQL_ROOT_PASSWORD="root_pwd" \
           -e ZBX_JAVAGATEWAY="zabbix-java-gateway" \
           --network=zabbix-net \
           -p 10051:10051 \
           --restart unless-stopped \
           -d zabbix/zabbix-server-mysql:latest
```

#### 5. 启动 Zabbix Web 界面，并将其关联到已创建的 MySQL server 和 Zabbix server 实例

```
docker run --name zabbix-web-nginx-mysql -t \
           -e ZBX_SERVER_HOST="zabbix-server-mysql" \
           -e DB_SERVER_HOST="mysql-server_zabbix" \
           -e MYSQL_DATABASE="zabbix" \
           -e MYSQL_USER="zabbix" \
           -e MYSQL_PASSWORD="zabbix_pwd" \
           -e MYSQL_ROOT_PASSWORD="root_pwd" \
           --network=zabbix-net \
           -p 80:8080 \
           --restart unless-stopped \
           -d zabbix/zabbix-web-nginx-mysql:latest
```
