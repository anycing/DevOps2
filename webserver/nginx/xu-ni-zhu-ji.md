# 虚拟主机

### 虚拟主机种类

* 同一端口多IP
* 同一IP多端口
* 同一IP同一端口多域名

### 同一端口多IP虚拟主机



### 同一IP多端口虚拟主机



### 同一IP同一端口多域名虚拟主机

```shell
cd /usr/local/nginx/conf

# 备份配置
cp nginx.conf nginx.conf_ori_bak

# 修改主配置文件
grep -Ev "#|^$" nginx.conf_ori_bak > nginx.conf
sed -i '10,21d' nginx.conf
sed -i '9a \ \ \ \ include domains/*.conf;' nginx.conf

# 创建测试网站配置文件（配置文件具体内容见下方）
mkdir domains
touch test1.conf
touch test2.conf

/usr/local/nginx/sbin/nginx -t

# 创建测试网站网页文件
cd /usr/local/nginx/html
mkdir test1 test2
echo 'test1' > test1/index.html
echo 'test2' > test2/index.html

# 重启 nginx
/usr/local/nginx/sbin/nginx -s reload
```

#### test1.conf

```
    server {
        listen       80;
        server_name  test1.yunwei361.com;
        location / {
            root   html/test1;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```

#### test2.conf

```
    server {
        listen       80;
        server_name  test2.yunwei361.com;
        location / {
            root   html/test2;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
