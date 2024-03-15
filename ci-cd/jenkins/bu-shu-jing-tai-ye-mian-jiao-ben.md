# 部署静态页面脚本

```
cat deploy_static_nginx.sh
```

```shell
DATE=$(date +%Y-%m-%d_%H-%M-%S)

web_server="10.0.0.11"

get_code(){
  cd $WORKSPACE && \
  tar czf /opt/web-${DATE}.tar.gz ./*
}

scp_web_server(){
  for host in $web_server
  do
    scp -i /root/.ssh/id_cluster /opt/web-${DATE}.tar.gz root@$host:/opt/
    ssh -i /root/.ssh/id_cluster root@$host "mkdir -p /nginx_web_html/web-${DATE} && \
                    tar xzf /opt/web-${DATE}.tar.gz -C /nginx_web_html/web-${DATE} && \
                    rm -rf /nginx_web_html/web && \
                    ln -s /nginx_web_html/web-${DATE} /nginx_web_html/web && \
                    /usr/sbin/nginx -s reload"
  done
}

deploy(){
  get_code
  scp_web_server
}

deploy
```

{% hint style="warning" %}
Jenkins服务器需要与部署服务器之间做ssh免密认证
{% endhint %}

{% hint style="warning" %}
若job执行过程中提示权限不够，则需要修改jenkins配置文件里的运行用户为root
{% endhint %}

```shell
vim /etc/sysconfig/jenkins

# JENKINS_USER="jenkins"
JENKINS_USER="root"
```

#### 在Jenkins执行Shell命令里写入如下命令，如下图

```
sh -x /opt/deploy_static.sh
```

![](<../../.gitbook/assets/image (7).png>)
