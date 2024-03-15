# 配置信息

### 用户配置信息

```
/var/lib/jenkins/users/admin_xxx/config.xml
```



### 主配置文件解析

```
grep -Ev '^#|^$' /etc/sysconfig/jenkins 
JENKINS_HOME="/var/lib/jenkins"    # 主数据目录，数据备份只需到打包该文件夹即可
JENKINS_JAVA_CMD=""
JENKINS_USER="jenkins"    # 启动用户
JENKINS_JAVA_OPTIONS="-Djava.awt.headless=true"
JENKINS_PORT="8080"    # 启动端口
JENKINS_LISTEN_ADDRESS=""    # 监听地址，若是公网访问可以修改为0.0.0.0
JENKINS_HTTPS_PORT=""
JENKINS_HTTPS_KEYSTORE=""
JENKINS_HTTPS_KEYSTORE_PASSWORD=""
JENKINS_HTTPS_LISTEN_ADDRESS=""
JENKINS_HTTP2_PORT=""
JENKINS_HTTP2_LISTEN_ADDRESS=""
JENKINS_DEBUG_LEVEL="5"
JENKINS_ENABLE_ACCESS_LOG="no"
JENKINS_HANDLER_MAX="100"
JENKINS_HANDLER_IDLE="20"
JENKINS_EXTRA_LIB_FOLDER=""
JENKINS_ARGS=""
```



### 修改Jenkins时区

![](<../../.gitbook/assets/image (3).png>)

![](<../../.gitbook/assets/image (98).png>)

![](<../../.gitbook/assets/image (64).png>)

```
System.setProperty('org.apache.commons.jelly.tags.fmt.timeZone', 'Asia/Shanghai')
```
