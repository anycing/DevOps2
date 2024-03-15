# kubeadm 安装 K8s

### 一键系统设置

```
# 关闭 SELinux
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config


# 关闭 firewalld 防火墙
systemctl stop firewalld
systemctl disable firewalld


# 加载 br_netfilter 模块
lsmod | grep br_netfilter
sudo modprobe br_netfilter


# 设置 net.bridge.bridge-nf-call-iptables 为 1
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system

```

#### 设置主机名与 hosts 解析（视本地环境而定）

```
hostnamectl set-hostname k8scp
exec bash

echo "10.0.0.11   k8scp" >> /etc/hosts
echo "10.0.0.12   worker01" >> /etc/hosts
echo "10.0.0.13   worker02" >> /etc/hosts

```



### 一键安装 Docker 脚本

```shell
#!/bin/bash

# 安装必要的一些系统工具
yum install yum-utils device-mapper-persistent-data lvm2 -y

# 添加软件源信息
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 更新yum
yum makecache fast


# 安装docker-ce软件包
yum install docker-ce -y


# 启动服务端
systemctl start docker
systemctl enable docker


# 查看信息
docker version


# 配置镜像加速
cat > /etc/docker/daemon.json << EOF
{
    "registry-mirrors": ["https://2cky5149.mirror.aliyuncs.com"]
}
EOF

# 重启服务端
systemctl restart docker

```



### 一键安装 K8s 脚本（CP）

```shell
#!/bin/bash

# 添加 aliyun K8s YUM 源
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF


# 查看历史版本
# yum list --showduplicates kubeadm


# 安装 1.21.7 版本的 K8s
yum install kubeadm-1.21.7-0 kubectl-1.21.7-0 kubelet-1.21.7-0 -y


# 初始化 K8s 集群
kubeadm init --image-repository=registry.aliyuncs.com/google_containers --kubernetes-version=v1.21.7
export KUBECONFIG=/etc/kubernetes/admin.conf
echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> /etc/profile
source /etc/profile
systemctl enable kubelet.service

# 初始化 Calico 网络配置
# wget https://docs.projectcalico.org/manifests/calico.yaml
wget https://willsgitbook.oss-cn-chengdu.aliyuncs.com/tools/calico/calico.yaml
kubectl apply -f calico.yaml


# 查看集群配置（创建时使用的kubeadm-config.yaml）
sudo kubeadm config print init-defaults

```



### 一键安装 K8s 脚本（Worker）

```shell
#!/bin/bash

# 添加 aliyun K8s YUM 源
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF


# 查看历史版本
# yum list --showduplicates kubeadm


# 安装 1.21.7 版本的 K8s
yum install kubeadm-1.21.7-0 kubectl-1.21.7-0 kubelet-1.21.7-0 -y
systemctl enable kubelet.service

# 节点加入集群，替换成本地环境
kubeadm join 10.0.0.11:6443 --token zpwegx.xt7zt1qcf7ohgdek \
         --discovery-token-ca-cert-hash sha256:901c09d866d67fd8adf50d162fd435b33d82b4d66b1cb21f17071f3e78136e6b

```



### 拓展

{% file src="../.gitbook/assets/calico.yml" %}
