# 系统设置

### 文件描述符

#### 查看系统级别能够打开的文件描述符数量

```shell
cat /proc/sys/fs/file-max
```

#### 修改系统级别文件描述符

```shell
vim /etc/sysctl.conf
fs.file-max = 6553500

sysctl -p
```

#### 查看用户级别能够打开的文件描述符数量

```shell
ulimit -n
```

#### 修改用户级

```
cat /etc/security/limits.conf

*                soft    nofile          65535
*                hard    nofile          65535
```

#### 用户级修改马上生效

```
ulimit -n 65535
```
