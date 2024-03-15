# test 比较

### 测试命令 test

* test 命令用于检查文件或者比较值
* test 可以做以下测试：
  * 文件测试
  * 整数比较测试
  * 字符串测试
* test 测试语句可以简化为 \[ ] 符号
* \[ ] 符号还有拓展写法 \[\[ ]] 支持 &&、||、<、>



### Demo

```bash
[root@localhost ~]# test -f /etc/passwd
[root@localhost ~]# echo $?
0
[root@localhost ~]# test -f /etc/passwd2
[root@localhost ~]# echo $?
1
[root@localhost ~]# [-d /etc]
bash: [-d: command not found...
[root@localhost ~]# [ -d /etc ]
[root@localhost ~]# echo $?
0
[root@localhost ~]# [ -e /etc ]
[root@localhost ~]# echo $?
0
[root@localhost ~]# [ 5 -gt 4 ]
[root@localhost ~]# echo $?
0
[root@localhost ~]# [ 5 -gt 6 ]
[root@localhost ~]# echo $?
1
```

```bash
[root@localhost ~]# [ 5 > 4 ]
[root@localhost ~]# echo $?
0
[root@localhost ~]# [ 5 < 4 ]
[root@localhost ~]# echo $?
0
[root@localhost ~]# [[ 5 > 4 ]]
[root@localhost ~]# echo $?
0
[root@localhost ~]# [[ 5 < 4 ]]
[root@localhost ~]# echo $?
1
```
