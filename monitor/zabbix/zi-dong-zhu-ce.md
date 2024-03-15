# 自动注册

### 配置 agent 配置文件

```
vim /etc/zabbix/zabbix_agent2.conf
ServerActive=10.0.0.11

# HostnameItem=system.hostname
# 开启此项配置后将会使用系统的主机名作为自动注册主机的显示名称
```

```
systemctl restart zabbix-agent2.service
```



### 添加自动注册动作

![](<../../.gitbook/assets/image (85).png>)

![](<../../.gitbook/assets/image (59).png>)

![](<../../.gitbook/assets/image (58).png>)
