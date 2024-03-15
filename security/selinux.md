# SELinux

### 查看 SELinux 状态

```shell
getenforce
```

###

### 修改 SELinux

#### 临时关闭

```
setenforce 0
```

#### 临时启用

```
setenforce 1
```

#### 永久生效（重启后生效，要立即生效需要重启系统或临时启用或临时关闭）

```shell
vim /etc/selinux/config
# enforcing
# permissive
# disabled
```

{% hint style="danger" %}
若系统启动时 SELinux 的状态就是 disabled ，在此状态下修改 SELinux 状态后需要重启系统，让系统重新打标签
{% endhint %}



### 修改文件 SELinux context 标签

#### chcon 命令修改文件 context 标签

```
chcon -t httpd_sys_content_t test.html
```

```
ls -Z test.html
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 test.html
```

{% hint style="info" %}
此方式修改的上下文标签可以被 restorecon 命令还原，命令如下
{% endhint %}

```
restorecon test.html
```

```
ls -Z test.html
-rw-r--r--. root root unconfined_u:object_r:admin_home_t:s0 test.html
```

{% hint style="info" %}
可以将策略写入 SELinux 规则，这样还原的话就可以将上下文还原成想要的策略，如下
{% endhint %}

#### semanage fcontext 修改 SELinux 数据库的规则，并还原生效

```
# 修改 /myapp/ 目录下所有文件的 SELinux context 默认规则
# -a添加，-t上下文标签
semanage fcontext -a -t httpd_sys_content_t '/myapp(/.*)?'
```

```
# 规则还原，-v可视化，-R递归
restorecon -vR /myapp/
```

```
# 查看
ls -Zd /myapp/
drwxr-xr-x. root root unconfined_u:object_r:httpd_sys_content_t:s0 /myapp/

ls -Z /myapp/
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 test.html
```



### 系统自带 SELinux boolean value

#### 查看 SELinux 布尔值

```
getsebool -a

getsebool -a | grep httpd_use_nfs
# httpd_use_nfs --> off
```

#### 修改 SELinux 布尔值

```
setsebool httpd_use_nfs=on
```

```
# 永久生效加 -P 选项即可
setsebool -P httpd_use_nfs=on
```

```
getsebool -a | grep httpd_use_nfs
# httpd_use_nfs --> on
```

{% hint style="info" %}
getsebool -a | wc -l

系统自带的 SELinux 布尔值有几百个，建议遇到哪个服务研究哪个
{% endhint %}



### SELinux 常用故障处理方法

#### sealert 命令

```
sealert -a /var/log/audit/audit.log
```

根据上方命令执行后生成的报告中给出的建议执行相关命令，此案例为 httpd 服务，报告给出的建议如下

```
Do allow this access for now by executing:
# ausearch -c 'httpd' --raw | audit2allow -M my-httpd
# semodule -i my-httpd.pp
```

{% hint style="success" %}
此方法可以解决绝大多数问题，挨个执行报告中给出的建议命令即可
{% endhint %}



### SELinux 端口安全

{% hint style="info" %}
SELinux 开启后默认会限制服务的端口，若要为相关服务使用新端口需要配置相关策略
{% endhint %}

此处用修改 ssh 服务默认端口为例做演示

#### 修改  ssh 默认的端口

```
vim /etc/ssh/sshd_config
# Port 22222
```

重启服务

```
systemctl restart sshd

# 重启服务报错
Job for sshd.service failed because the control process exited with error code.
See "systemctl status sshd.service" and "journalctl -xe" for details.
```

#### 增加 ssh 服务的 SELinux 策略端口

```
semanage port -l | grep ssh
# ssh_port_t                     tcp      22
```

```
semanage port -a -t ssh_port_t -p tcp 22222
```

```
semanage port -l | grep ssh
# ssh_port_t                     tcp      22222, 22
```

```
systemctl restart sshd
```



### 拓展：常用 SELinux 命令

#### 查询

```
getsebool -a

semanage boolean -l
```

```
semanage port -l
```

```
semanage fcontext -l
```

#### 修改

```
chcon -t <context> <file>
```

```
semanage fcontext -a -t <context> <file>
```

```
restorecon <file>
```

```
setsebool -P <sebool_name>=on

setsebool -P <sebool_name>=off
```

```
semanage port -a -t <name_port_t> -p tcp <port>

semanage port -d -p tcp <port>
```

#### 万能解决方案

```
sealert -a /var/log/audit/audit.log
```
