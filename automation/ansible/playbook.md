# playbook

### 剧本简介

> YAML文件开头需要先写三个减号---，多个分组信息需要间隔一致才能执行，上下也要对齐，后缀名一般为.yml
>
> Playbook剧本文件的结构由四部分组成——target、variable、task、handler。target部分用于定义要执行剧本的主机节点范围、variable部分用于定义剧本执行时要用的变量、task部分用于定义将在远程主机上执行的任务列表、handler部分用于定义执行完成后需要调用的后续任务。
>
> 其中name字段代表此项Play（动作）的名字，可自行命名，没有限制，用于在执行过程中提示用户执行到了那一步，以及帮助管理员日后阅读时回忆起这段代码的作用。
>
> hosts字段代表要在哪些主机节点上执行该剧本，多个主机组之间用逗号间隔，若需要对全部主机进行操作则用all参数。
>
> tasks字段用于定义要执行的任务，每个任务都要有一个独立的name字段进行命名，并且每个任务的name字段和模块名称都要严格上下对齐，参数要单独缩进。

#### 剧本示例

```
cat > /opt/packages.yml << EOF
---
- name: 安装软件包
  hosts: balancers
  tasks:
    - name: install mariadb
      yum:
        name: mariadb
        state: latest
EOF
```

#### 执行剧本

```
ansible-playbook /opt/packages.yml
```



### playbook 安装 Zabbix agent

#### 编写剧本

```shell
vim install_zabbix_agent2.yml

- name: install zabbix_agent2
  hosts: all
  vars:
  # 定义一个变量存储客户端文件
    agent_file: zabbix-agent2-5.4.8-1.el7.x86_64.rpm
  tasks:
  
  # 拷贝rpm包到远程，rpm包名为上方定义的变量
    - name: copy file to remote
      copy:
        src: "/opt/{{ agent_file }}"
        dest: /tmp

    - name: install rpm
      command: rpm -i "/tmp/{{ agent_file }}"

# 停止老服务
    - name: stop old agent
      command: systemctl disable --now zabbix-agent

# 修改配置文件中的 Server=127.0.0.1 为 Server=10.0.0.11
    - name: set config file Server
      lineinfile:
        path: /etc/zabbix/zabbix_agent2.conf
        regexp: '^Server=127.0.0.1'
        line: "Server=10.0.0.11"

    - name: set config file ServerActive
      lineinfile:
        path: /etc/zabbix/zabbix_agent2.conf
        regexp: '^ServerActive=127.0.0.1'
        line: "ServerActive=10.0.0.11"

    - name: set config file Hostname
      lineinfile:
        path: /etc/zabbix/zabbix_agent2.conf
        regexp: '^Hostname=Zabbix server'
        line: "Hostname={{ ansible_default_ipv4.address }}"

# 启动服务
    - name: start agent2
      command: systemctl enable --now zabbix-agent2
```

#### 检查剧本语法正确性

```
ansible-playbook --syntax-check install_zabbix_agent2.yml
```

#### 执行剧本

```
ansible-playbook install_zabbix_agent2.yml
```

