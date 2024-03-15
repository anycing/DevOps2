# 安装Jenkins

### 安装 JDK （Oracle JDK）

```shell
# Oracle 官方下载
rpm -ivh jdk-8u311-linux-x64.rpm

# 删除之前java的软链接（可以记录或备份下之前的软链接）
which java
ls -lh /usr/bin/java
# java -> /etc/alternatives/java
# /etc/alternatives/java -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-1.el7.x86_64/jre/bin/java
rm -f /usr/bin/java

# 创建新的java软链接（此处以1.8.0311版本演示）
ln -s /usr/java/jdk1.8.0_311-amd64/jre/bin/java /usr/bin/java

```



### 安装 Jenkins

```shell
# 下载
wget --no-check-certificate https://mirrors.tuna.tsinghua.edu.cn/jenkins/redhat/jenkins-2.299-1.1.noarch.rpm

# 安装
rpm -ivh jenkins-2.299-1.1.noarch.rpm

# 检查版本
java -jar /usr/lib/jenkins/jenkins.war --version

# 启动
systemctl start jenkins
systemctl status jenkins
```



### Jenkins 配置文件说明

```
rpm -ql jenkins 
/etc/init.d/jenkins    # 启动目录
/etc/logrotate.d/jenkins    # 日志切割
/etc/sysconfig/jenkins    # Jenkins配置文件
/usr/lib/jenkins    # 程序文件，直接替换新版的Jenkins.war包，重启即升级
/usr/lib/jenkins/jenkins.war    # war包
/usr/sbin/rcjenkins
/var/cache/jenkins
/var/lib/jenkins    # Jenkins主目录，如果要备份Jenkins所有数据，直接拷贝这个目录即可
/var/log/jenkins    # 日志文件
```



### 访问 Jenkins 页面

```
# Jenkins默认端口为8080，使用浏览器访问
10.0.0.12:8080
```

#### 查看默认密码

```
cat /var/lib/jenkins/secrets/initialAdminPassword
```

#### 修改密码

![](<../../.gitbook/assets/image (53).png>)

![](<../../.gitbook/assets/image (101).png>)
