# 自定义监控项

{% hint style="info" %}
&#x20;用户自定义的监控项仅在agent端配置目录下存在时才有效，目录为：

<mark style="color:green;">**/etc/zabbix/zabbix\_agent2.d**</mark>
{% endhint %}

### 创建一个统计登录用户数的自定义监控项

#### 创建 KEY

```
cat /etc/zabbix/zabbix_agent2.d/num_login.conf
UserParameter=user.login,who | wc -l

# 此处创建的 KEY 为：user.login
# Zabbix server 调用后会返回执行 who | wc -l 命令的结果
```

```
systemctl restart zabbix-agent2.service
```

#### 验证有效性（在 Server 端操作）

```
zabbix_get -s '10.0.0.12' -p 10050 -k 'user.login'
```



### 在 Web 页面添加自定义监控项

#### 流程

1. 创建模板
2. 创建监控项，自定义 item （具体监控的内容）
3. 创建触发器（当监控项获取到值时与触发器进行比较，决定是否报警）
4. 创建图形
5. 将具体的主机和该模板进行链接关联

#### 1. 创建模板

![](<../../.gitbook/assets/image (119).png>)

![](<../../.gitbook/assets/image (115).png>)

#### 2. 创建监控项

![](<../../.gitbook/assets/image (34).png>)

![](<../../.gitbook/assets/image (32).png>)

![](<../../.gitbook/assets/image (33).png>)

![](<../../.gitbook/assets/image (93).png>)

#### 3. 创建触发器

![](<../../.gitbook/assets/image (116).png>)

![](<../../.gitbook/assets/image (24).png>)

![](<../../.gitbook/assets/image (104).png>)

![](<../../.gitbook/assets/image (47).png>)

#### 4. 创建图形

![](<../../.gitbook/assets/image (21).png>)

![](<../../.gitbook/assets/image (81).png>)

![](<../../.gitbook/assets/image (109).png>)

#### 5. 将模板关联到主机

![](<../../.gitbook/assets/image (113).png>)
