# 常用命令

### 查看 K8s 支持的所有资源

```
kubectl api-resources
```



### 查看 K8s 支持的所有 api 版本

```
kubectl api-versions
```

```
kubectl explain pod
kubectl explain pod.apiVersion
```



### 查看资源排序

```
kubectl get po --sort-by=.status.startTime
```



### 备份 etcd

#### 直接使用容器里的客户端工具进行备份

查看

```
kubectl -n kube-system exec -it etcd-k8scp -- sh -c \
"ETCDCTL_API=3 \
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt \
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt \
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key  \
etcdctl --endpoints=https://127.0.0.1:2379 \
member list -w table"

```

备份

```
kubectl -n kube-system exec -it etcd-k8scp -- sh -c \
"ETCDCTL_API=3 \
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt \
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt \
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key \
etcdctl --endpoints=https://127.0.0.1:2379 \
snapshot save /var/lib/etcd/snapshot_$(date "+%F_%T").db"

```

#### 拷贝etcdctl客户端工具至宿主机，在宿主机备份

拷贝客户端工具

```
docker ps | grep etcd

docker cp bff304f0b31d:/usr/local/bin/etcdctl /usr/local/bin/
```

设置 ETCDCTL\_API 版本

```
export ETCDCTL_API=3
```

查看

```
etcdctl --endpoints=https://127.0.0.1:2379 \
        --cacert=/etc/kubernetes/pki/etcd/ca.crt \
        --cert=/etc/kubernetes/pki/etcd/server.crt \
        --key=/etc/kubernetes/pki/etcd/server.key \
        member list -w table

```

备份

```
etcdctl --endpoints=https://127.0.0.1:2379 \
        --cacert=/etc/kubernetes/pki/etcd/ca.crt \
        --cert=/etc/kubernetes/pki/etcd/server.crt \
        --key=/etc/kubernetes/pki/etcd/server.key \
        snapshot save /var/lib/etcd/snapshot_$(date "+%F_%T").db

```

