# 脚本不同执行方式的影响

### 脚本的4种执行方式

* bash ./filename.sh
* ./filename.sh
* source ./filename.sh
* . ./filename.sh



### 脚本不同执行方式的影响

#### 产生子进程

```bash
bash ./filename.sh
```

```bash
./filename.sh
```

#### 不产生子进程

```bash
source ./filename.sh
```

```bash
. ./filename.sh
```



### 示例

#### demo.sh

```bash
cat > demo.sh << EOF
cd /tmp
pwd
EOF

```

#### 产生子进程的运行方式的结果

```bash
[root@localhost ~]# bash demo.sh 
/tmp
[root@localhost ~]# pwd
/root
```

```bash
[root@localhost ~]# chmod u+x demo.sh 
[root@localhost ~]# ./demo.sh 
/tmp
[root@localhost ~]# pwd
/root
```

#### 不产生子进程的运行方式的结果

```bash
[root@localhost ~]# source ./demo.sh 
/tmp
[root@localhost tmp]# pwd
/tmp
```

```bash
[root@localhost ~]# . ./demo.sh 
/tmp
[root@localhost tmp]# pwd
/tmp
```



### 内建命令和外部命令的区别

* 内建命令不需要创建子进程
* 内建命令对当前 Shell 生效
