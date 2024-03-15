# Zabbix-get 工具

{% hint style="warning" %}
此处演示软件版本与服务端版本保持一致为 5.4 版，此工具只能安装在服务端上
{% endhint %}

### agent 连通性验证

#### 下载安装 zabbix-get 工具

```
curl -o zabbix-get-5.4.8-1.el7.x86_64.rpm \
     https://mirrors.aliyun.com/zabbix/zabbix/5.4/rhel/7/x86_64/zabbix-get-5.4.8-1.el7.x86_64.rpm

rpm -ivh zabbix-get-5.4.8-1.el7.x86_64.rpm

```

#### 执行命令

ping

```
zabbix_get -s '10.0.0.12' -p 10050 -k 'agent.ping'

# -s 指定 agent 的 host-name-or-IP
# -p 指定 agent 监听的端口
# -k 指定要执行的命令
```

获取主机名

```
zabbix_get -s '10.0.0.12' -p 10050 -k 'system.hostname'
```
