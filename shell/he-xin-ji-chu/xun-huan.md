# 循环

## 循环

* 使用 for 循环遍历命令的执行结果
* 使用 for 循环遍历变量和文件的内容
* C 语言风格的 for 命令
* while 循环
* 死循环
* until 循环
* break 和 continue 语句
* 使用循环对命令行参数的处理



### for 循环的语法

```bash
for 参数 in 列表
do 执行的命令
done 封闭一个循环
```

使用反引号或 $() 方式执行命令，命令的结果当作列表进行处理



### 使用 for 循环遍历变量和文本

* 列表中包含多个变量，变量用空格分隔
* 对文本处理，要使用文本查看命令去除文本内容
  * 默认逐行处理，如果文本出现空格会当作多行处理

#### Demo1

```bash
[root@localhost ~]# for i in {1..9}
> do
>     echo $i
> done
1
2
3
4
5
6
7
8
9
```

#### Demo2

```bash
[root@localhost ~]# touch {1..3}.mp3
[root@localhost ~]# ls *.mp3
1.mp3  2.mp3  3.mp3
[root@localhost ~]# for filename in `ls *.mp3`
> do
>     mv $filename `basename $filename .mp3`.mp4
> done
[root@localhost ~]# ls *.mp3
ls: cannot access *.mp3: No such file or directory
[root@localhost ~]# ls *.mp4
1.mp4  2.mp4  3.mp4
```



### C 语言风格的 for

```bash
for ((变量初始化；循环判断条件；变量变化))
do
    循环执行的命令
done
```

#### Demo

```bash
[root@localhost ~]# for(( i=1; i<=10; i++ ))
> do
>     echo $i
> done
1
2
3
4
5
6
7
8
9
10
```





### while 循环

```bash
while test测试是否成立
do
    命令
done
```

#### Demo1

```bash
[root@localhost ~]# a=1
[root@localhost ~]# while [ $a -lt 10 ]
> do
>     echo $a
>     (( a++ ))
> done
1
2
3
4
5
6
7
8
9
```

#### Demo2 - 死循环

```bash
while :
do
    echo 'hello world !'
done
```



### until 循环

until 循环和 while 循环相反，循环测试为假时，执行循环，为真时循环停止

```bash
until [ 5 -lt 4 ]:
do
    echo 'hello world !'
done
```



### 循环嵌套语句

#### Demo - 找出目录 /etc/profile.d/ 下具有可执行权限的 .sh 脚本

```bash
[root@localhost ~]# for sc_name in /etc/profile.d/*.sh
> do
>     if [ -x $sc_name ] ; then
>         echo $sc_name
>     fi
> done
```



### break 中断循环（永久跳出循环）

#### Demo

```bash
[root@localhost ~]# for num in {1..9}
> do
>     if [ $num -eq 5 ] ; then
>         break
>     fi
>     echo $num
> done
1
2
3
4
```



### continue 跳过循环（临时跳出循环）

#### Demo

```bash
[root@localhost ~]# for num in {1..9}
> do
>     if [ $num -eq 5 ] ; then
>         continue
>     fi
>     echo $num
> done
1
2
3
4
6
7
8
9
```



{% hint style="info" %}
break、continue 适合所有类型的循环跳出（for、while、until）
{% endhint %}



### 使用循环处理命令行参数

#### Demo1 -  使用 shift 命令处理位置参数（shift的作用为每次删除最左边的一个位置参数）

```bash
#!/bin/bash

while [ $# -ge 1 ]
do
    echo $#
    echo "do something"
    shift
done
```

```bash
[root@localhost ~]# bash 1.sh a b c d e
5
do something
4
do something
3
do something
2
do something
1
do something
```

#### Demo2

```bash
#!/bin/bash

while [ $# -ge 1 ]
do
    if [ "$1" = "hello" ] ; then
        echo "There is $1"
    fi
    shift
done
```

```bash
[root@localhost ~]# sh 2.sh a b c d
[root@localhost ~]# sh 2.sh a b c d hello
There is hello
```
