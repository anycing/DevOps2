# 安装配置

### 安装依赖

```
yum install curl policycoreutils-python openssh-server perl postfix wget -y
```

设置 ssh 服务与防火墙

```shell
# Enable OpenSSH server daemon if not enabled: sudo systemctl status sshd
sudo systemctl enable sshd
sudo systemctl start sshd

# Check if opening the firewall is needed with: sudo systemctl status firewalld
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo systemctl reload firewalld
```



### 安装 gitlab-ce

#### 官方 yum 源方式安装

```
# 安装官方 yum 源
curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash

# 安装 gitlab-ce
yum install gitlab-ce -y
```

#### rpm 包方式安装

```
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-14.8.2-ce.0.el7.x86_64.rpm

yum localinstall gitlab-ce-14.8.2-ce.0.el7.x86_64.rpm -y
```



### 配置 gitlab

#### 修改 /etc/gitlab/gitlab.rb

external\_url & GitLab email server settings & Email Settings

```
grep -Ev '^#|^$' /etc/gitlab/gitlab.rb

# external_url
external_url 'http://10.0.0.11'

# GitLab email server settings
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.163.com"
gitlab_rails['smtp_port'] = 465
gitlab_rails['smtp_user_name'] = "anycing@163.com"
gitlab_rails['smtp_password'] = "GJQFEVVKRXBWORWP"
gitlab_rails['smtp_domain'] = "smtp.163.com"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = true

# Email Settings
gitlab_rails['gitlab_email_enabled'] = true
gitlab_rails['gitlab_email_from'] = 'anycing@163.com'
gitlab_rails['gitlab_email_display_name'] = 'anycing163_gitlab'
```

{% hint style="warning" %}
配置信息里 GitLab email server settings 和 Email Settings 参数里的邮箱必须保持一致
{% endhint %}

#### 重新加载配置文件

```
gitlab-ctl reconfigure

# gitlab-ctl restart
```

#### 测试控制台

```
# 进入控制台
gitlab-rails console

# 测试邮件
Notify.test_email('anycing@qq.com','hello','这是测试邮件').deliver_now
```



### 常用命令

#### 启停

```
gitlab-ctl start | restart | stop

# 启停某一个服务
gitlab-ctl stop postgresql

# 查看状态
gitlab-ctl status
```

#### 查看日志

```
gitlab-ctl tail

# 查看某一个服务的日志
gitlab-ctl tail postgresql
```



### 软件组件目录

```shell
# 库默认存储目录
/var/opt/gitlab/git-data/repositories/

# 应用代码和相应的依赖程序
/opt/gitlab/

# gitlab-ctl reconfigure 生成的数据和配置
/var/opt/gitlab

# 配置文件目录
/etc/gitlab

# gitlab各个组件产生的日志
/var/log/gitlab

# 备份文件生成的目录
/var/opt/gitlab/backups
```



### 默认账号密码

```
用户名：root
密码：cat /etc/gitlab/initial_root_password

# 24小时内修改
```

