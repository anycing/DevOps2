# frp

### Server

#### frps.ini

```
[common]
bind_port = 7000
bind_udp_port = 7001
kcp_bind_port = 7000
vhost_http_port = 9999
vhost_https_port = 10000

dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = 520Frps._.

log_file = /usr/local/frp/logs/frps.log
log_level = info
log_max_days = 14
disable_log_color = false

authentication_method = token
token = 381ecd469c0cc767eb773d34c30405a132a4a33be263a06c55d8d829b28c3521
```

#### Service configuration

```
# service PATH
cp ./systemd/frps.service /usr/lib/systemd/system/frps.service


# Modify
ExecStart=/usr/local/frp/frps -c /usr/local/frp/frps.ini
```



### Client

#### frpc.ini

```
// Some code
```

#### Service configuration

```
# service PATH
cp ./systemd/frpc.service /usr/lib/systemd/system/frpc.service


# Modify
ExecStart=/usr/local/frp/frpc -c /usr/local/frp/frpc.ini
```
