# 安装

### 下载

```shell
wget -c https://nginx.org/download/nginx-1.22.0.tar.gz

tar xvf nginx-1.22.0.tar.gz

cd ./nginx-1.22.0
```

### 安装编译器及依赖

```shell
yum install gcc pcre-devel zlib-devel openssl-devel -y
```

### 编译

```shell
./configure --prefix=/usr/local/nginx/ --user=www --group=www --with-http_stub_status_module --with-http_ssl_module

make -j4

make -j4 install
```

### 添加用户

```shell
useradd -s /sbin/nologin www -M
```

### 启动

```shell
/usr/local/nginx/sbin/nginx
```
