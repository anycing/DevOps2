# 函数

## 函数

* 自定义函数
* 系统函数库



### 自定义函数

* 函数用于 “包含” 重复使用的命令集合
* 自定义函数
  * function fname(){命令}
* 函数的执行
  * fname

#### Demo - 检查 PID 是否存在，存在返回0

```bash
[root@localhost ~]# cat checkpid.sh 
#!/bin/bash

function checkpid() {
    local i

    for i in $@
    do
        [ -d "/proc/$i" ] && return 0
    done

    return 1
}
[root@localhost ~]# . checkpid.sh 
[root@localhost ~]# checkpid 3000 6000
[root@localhost ~]# echo $?
1
[root@localhost ~]# checkpid 1 6000
[root@localhost ~]# echo $?
0
```

{% hint style="info" %}
local 定义局部变量，作用范围仅在函数体内部有效
{% endhint %}



### 系统函数库

*   系统自建了函数库，可以在脚本中引用

    * /etc/init.d/functions


* 自建函数库
  * 使用 source 函数脚本文件 “导入” 函数



