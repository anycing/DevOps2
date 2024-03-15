# 安装配置

### 安装 Ansible

```
curl -o /etc/yum.repos.d/epel.repo \
     http://mirrors.aliyun.com/repo/epel-7.repo
```

```
yum install ansible -y
```



### 配置 Ansible

#### 配置 SSH 连接新主机不检查 key

```
vim /etc/ansible/ansible.cfg

host_key_checking = False
# 取消新主机key文件检查（首次连接主机不用输yes）
```

使用 sed 一键修改

```
sed -i 's/#host_key_checking = False/host_key_checking = False/g' /etc/ansible/ansible.cfg
```

#### 主机列表配置（带密码验证信息的写法）

```
vim /etc/ansible/hosts

[k8s]
10.0.0.11 ansible_ssh_user=root ansible_ssh_pass=123456 ansible_ssh_port=60022
10.0.0.12 ansible_ssh_user=root ansible_ssh_pass=123456 ansible_ssh_port=60022
10.0.0.13 ansible_ssh_user=root ansible_ssh_pass=123456 ansible_ssh_port=60022

[extra]
10.0.0.1
10.0.0.2
10.0.0.3

[all:vars]
ansible_user=root
ansible_password=123456
```

#### 查看主机列表

```
ansible-inventory --graph
```



### 常用参数说明

| 参数            | 说明                               |
| ------------- | -------------------------------- |
| -m            | 模块名                              |
| -a            | 模块的参数                            |
| -f            | 并发数                              |
| -i            | 指定主机列表文件（默认为 /etc/ansible/hosts） |
| --private-key | 指定验证的私钥文件                        |

#### 使用示例

```
ansible web_server --private-key ~/.ssh/id_cluster -m command -a "free -m"

# web_server 为主机列表配置文件里的模块名称
```

