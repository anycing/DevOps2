# 模块

### 查看模块

```
ansible-doc -l
```

###

### 查看指定模块的帮助信息

```
ansible-doc -s MODULE

# 查看 MODULE 模块的帮助信息
```



### command 模块

```
ansible web_server -m command -a "hostname -I"
```

{% hint style="danger" %}
command 模块 不支持 > < | & 等 符号，如需要使用则可以自写Shell替代
{% endhint %}



### shell 模块

```
ansible web_server -m shell -a "/server/scripts/time.sh"

# Shell脚本需要存在于远程主机上
```



### copy 模块

```
ansible web_server -m copy -a "src=/etc/hosts dest=/tmp mode=0644 backup=yes"

# src 本地文件或目录
# src 若为 /etc 则代表把 /etc 目录复制到远端
# src 若为 /etc/ 则代表把 /etc/ 目录下的文件复制到远端
# dest 远端文件或目录
# mode 指定复制过去文件或目录的权限，可使用所有的权限格式
# backup 指定为yes时目标主机上已存在文件时在覆盖前对源文件进行备份
# 默认不备份直接覆盖
```



### script 模块

```
ansible web_server -m script -a "/server/scripts/test.sh"

# 在远端服务器上执行本地的Shell脚本
# 若脚本指定的参数为文件，则文件需要存在于远端服务器上
```



### cron 模块

```
ansible web_server --private-key ~/.ssh/id_cluster -m cron -a "name='backup data' minute=5 hour=3 job='/server/scripts/backup.sh > /dev/null 2>&1'"

# name 指定任务的名称（注释信息）
# minute    0-59    *    */2    etc
# hour      0-23    *    */2    etc
# day       1-31    *    */2    etc
# month     1-12    *    */2    etc
# weekday   0-6     *    */2    etc
# job       The command to execute

# state=absent 删除定时任务
# backup=yes 备份

# 删除示例
ansible web_server --private-key ~/.ssh/id_cluster -m cron -a "name='backup data' state=absent backup=yes"

# name 不变，再次执行命令则会覆盖原name命令，可以理解为修改
```

