# 变量 & 环境变量

## 变量

### 变量的定义

#### 变量的命名规则

* 字母、数字、下划线
* 不以数字开头



### 变量的赋值

变量赋值的过程，成为变量替换

* 变量名=变量值（等号两边不能有空格）
  * a=123
* 使用let为变量赋值（可以让变量进行计算）
  * let a=10+20
* 将命令赋值给变量（很少用，无意义）
  * l=ls
* 将命令结果赋值给变量，使用 $( ) 或者 \` \` （当命令会多次执行时，可以赋值给变量，通过调用变量以减少系统开销）
  * letc=$(ls -l /etc)
* 变量值有空格等特殊字符可以包含在 " " 或 ' ' 中



### 变量的引用

* ${变量名} 称作对变量的引用
* echo ${变量名} 查看变量的值
* ${变量名} 在部分情况下可以省略为 $变量名

#### Demo

```bash
string1="hello bash"

echo $string1

echo ${string1}

echo $string123

echo ${string1}23

```



### 变量的作用范围

* 变量的默认作用范围
  * 只针对当前的终端或当前的 Shell 生效（子、父、平行均不生效）
* 变量的导出
  * export（导出变量，让子进程得到父进程的变量）
* 变量的删除
  * unset

#### Demo1

```bash
[root@localhost ~]# a=1
[root@localhost ~]# bash
[root@localhost ~]# echo $a

[root@localhost ~]# a=2
[root@localhost ~]# exit
exit
[root@localhost ~]# echo $a
1
```

#### Demo2

```bash
demo_var1="hello subshell"

vim demo2.sh
#!/bin/bash
echo $demo_var1

chmod u+x demo2.sh
```

```bash
[root@localhost ~]# bash demo1.sh 

[root@localhost ~]# ./demo1.sh 

[root@localhost ~]# source demo1.sh 
hello subshell
[root@localhost ~]# . demo1.sh 
hello subshell
```

导出变量，让子进程得到父进程的变量

```bash
[root@localhost ~]# export demo_var1
[root@localhost ~]# bash demo1.sh 
hello subshell
[root@localhost ~]# ./demo1.sh 
hello subshell
```

删除变量（用于某一变量在脚本中反复使用时取消变量）

```bash
[root@localhost ~]# unset demo_var1 
[root@localhost ~]# echo $demo_var1

```





## 环境变量

### 系统环境变量

* 环境变量：每个 Shell 打开都可以获得到的变量
  * set 和 env 命令
  * $PATH
  * $PS1
* 预定义变量
  * $?
  * \$$ （进程PID）
  * $0 （进程名称）
  * $! （上一个在后台工作的进程的进程号）
  * $\_ （上一个执行的命令或脚本的最后一个参数）
  * $\* （所有位置参数视为一个整体字符串）
  * $@ （所有位置参数视为单独的字符串）
  * $# （位置参数的数量）
* 位置变量
  * $1    $2    ...    $n  从${10}开始，参数号需要用大括号括起来，如${10}、${11}、${100}等。

内部参数分两类，一类是命令行参数相关的

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

另一类内部参数是与进程状态相关的

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

#### Demo1 - 预定义变量

```bash
cat demo1.sh
#!/bin/bash
echo $$
echo $0
```

```bash
[root@localhost ~]# bash demo1.sh
21330
demo.sh
[root@localhost ~]# source demo1.sh
19781
bash
```

#### Demo2 - 位置变量

```bash
cat demo2.sh
#!/bin/bash
echo $1
echo $2

chmod u+x demo2.sh
```

```bash
[root@localhost ~]# ./demo2.sh -a -l
-a
-l
[root@localhost ~]# ./demo2.sh -a
-a

```

避免位置参数为空值的小技巧1（**不完美**，若位置变量不为空则会导致增加\_）

```bash
cat demo2.sh
#!/bin/bash
echo $1
echo $2_


# 增加执行权限
chmod u+x demo2.sh
```

```bash
[root@localhost ~]# ./demo2.sh -a
-a
_
[root@localhost ~]# ./demo2.sh -a -l
-a
-l_
```

避免位置参数为空值的小技巧2（**完美**，此方式称为**参数替换**）

```bash
cat demo2.sh
#!/bin/bash

echo $1
echo ${2-_}


# 增加执行权限
chmod u+x demo2.sh
```

```bash
[root@localhost ~]# ./demo3.sh -a
-a
_
[root@localhost ~]# ./demo3.sh -a -l
-a
-l
```



### 环境变量配置文件

#### 配置文件

* /etc/profile
* /etc/profile.d/
* \~/.bash\_profile
* \~/.bashrc
* /etc/bashrc

/etc 下的配置文件是给所有用户公用的

#### login shell : su - userName

所有配置文件均会执行，顺序如下：（可以在每个文件开头增加 echo 命令进行验证）

1. &#x20;/etc/profile
2. &#x20;.bash\_profile
3. &#x20;.bashrc
4. &#x20;/etc/bashrc

#### nologin shell : su userName

只有bashrc执行，顺序如下：

1. &#x20;.bashrc
2. &#x20;/etc/bashrc

{% hint style="info" %}
登录建议使用 " - " 进行登录；

在配置文件中添加了环境变量后不会立即生效，需要重新打开终端或者用 source 导入
{% endhint %}

