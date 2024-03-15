# 内存监控

### 脚本内容

#### memory\_monitor.sh

```bash
#!/bin/bash

Mem_Ava=$(free -m | awk 'NR==2 {print $NF}')
IP=$(/usr/sbin/ifconfig | grep "inet " | grep -v 127.0.0.1 | awk '{print $2}')
subject="Memory warning: ${IP}"

if [ ${Mem_Ava} -lt 100 ] ; then
    echo -e "Time: $(date +"%F %T")\n" > /tmp/memory_content.txt
    echo -e "IP: ${IP}\n" >> /tmp/memory_content.txt
    echo -e "Content: 内存不足 Current available memory is ${Mem_Ava}M\n" | tee -a /tmp/memory_content.txt
    # mail -s "主题" 收件人 < 来自文件的内容
    mail -s "${subject}" anycing@qq.com < /tmp/memory_content.txt
fi
```

#### config\_email.sh （配置邮件服务）

```bash
#!/bin/bash

# 配置邮件服务发件人相关内容
echo "set from=anycing@sina.com smtp=smtp.sina.com smtp-auth-user=anycing smtp-auth-password=a7c7b1c1e2c87571dz smtp-auth=login" >> /etc/mail.rc

# start mail service
systemctl restart postfix
systemctl enable postfix
```

#### 写入定时任务

```
echo "*/3 * * * * /bin/bash /root/scripts/memory_monitor.sh &> /dev/null" >> /var/spool/cron/root

chmod 600 /var/spool/cron/root
```



### 坑

#### 现象：手动执行脚本时一切正常，但是定时任务调度时却获取不到IP

#### 解决方案：系统命令 ifconfig 在脚本中需要使用绝对路径 /usr/sbin/ifconfig
