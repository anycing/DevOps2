# 客户端 agent 安装

{% hint style="success" %}
鉴于服务端安装的版本为5.4，此处客户端也安装对应的版本，此演示选择最简单的 **rpm** 包方式安装
{% endhint %}

#### 下载安装

```
curl -o zabbix-agent2-5.4.8-1.el7.x86_64.rpm \
        https://mirrors.aliyun.com/zabbix/zabbix/5.4/rhel/7/x86_64/zabbix-agent2-5.4.8-1.el7.x86_64.rpm

rpm -ivh zabbix-agent2-5.4.8-1.el7.x86_64.rpm
```

#### 编辑配置文件

```
grep -Ev '^#|^$' /etc/zabbix/zabbix_agent2.conf

# Zabbix 服务端 IP
Server=10.0.0.11
ServerActive=10.0.0.11

# 本机主机名
Hostname=web-10.0.0.11
```

#### 启动

```
systemctl enable --now zabbix-agent2.service
```
