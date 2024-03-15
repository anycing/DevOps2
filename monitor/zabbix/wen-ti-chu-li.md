# 问题处理

### 解决图形中的中文乱码

```
docker cp /usr/share/fonts/wqy-microhei/wqy-microhei.ttc \
          zabbix-web-nginx-mysql:/usr/share/zabbix/assets/fonts/DejaVuSans.ttf
```

{% hint style="success" %}
此处使用系统自带的 wqy-microhei 字体替换 zabbix-web 容器里的默认字体
{% endhint %}



### 解决 agent 与 server 同时部署时主机可用性一直处于不可用的状态

#### 配置 agent 的 conf 文件时公网私网IP同时配上

```
vim /etc/zabbix/zabbix_agent2.conf

Server=10.0.0.11,172.20.240.1
ServerActive=10.0.0.11,172.20.240.1
```

{% hint style="danger" %}
如果 server 端是 docker 部署的，则使用 docker inspect 命令查看 zabbix server 容器的私网IP
{% endhint %}
