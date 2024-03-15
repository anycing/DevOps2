# 运算符

### 运算符种类

* 赋值运算符
* 算数运算符

**拓展**

* 数字常量
* 双圆括号

{% hint style="danger" %}
**bash 原生语法不支持算数运算，需使用 (())、 let、expr 等**
{% endhint %}

###

### 赋值运算符

* \= 赋值运算符，用于算数赋值和字符串赋值
* 使用 unset 取消为变量赋的值
* \= 除了作为赋值运算符还可以作为测试操作符

####

### 算数运算符

* 基本运算符
  * \+   &#x20;
  * \-   &#x20;
  * \*   &#x20;
  * /   &#x20;
  * \*\*   &#x20;
  * %
* 使用 expr 进行运算（<mark style="color:yellow;">**只支持传参的形式，参数用空格分隔**</mark>）
  * expr 4 + 5



### 数字常量

数字常量的使用方法：

* let "变量名 = 变量值"
* 变量值使用 0 开头为八进制
* 变量值使用 0x 开头为十六进制



### 双圆括号

双圆括号是 let 命令的简化

* (( a = 10 ))
* (( a++ ))
* echo $(( 10+20 ))



### Demo - 双圆括号

```bash
[root@localhost ~]# (( a=3+2 ))
[root@localhost ~]# echo $a
5
[root@localhost ~]# b=2+5
[root@localhost ~]# echo $b
2+5
[root@localhost ~]# (( a++ ))
[root@localhost ~]# echo $a
6
[root@localhost ~]# (( a++ ))
[root@localhost ~]# echo $a
7
```

### Demo - let

```bash
[root@localhost ~]# num=5+1
[root@localhost ~]# echo $num
5+1
[root@localhost ~]# let num=5+1
[root@localhost ~]# echo $num
6
[root@localhost ~]# let num=num+1
[root@localhost ~]# echo $num
7
[root@localhost ~]# let num+=1
[root@localhost ~]# echo $num
8
```

### Demo - expr

```bash
# expr只支持传参的形式
[root@localhost ~]# expr 4 + 5
9
[root@localhost ~]# expr 4+5
4+5
[root@localhost ~]# expr 4 +5
expr: syntax error

# expr 只支持整数运算
[root@localhost ~]# expr 4 + 5.2
expr: non-integer argument
[root@localhost ~]# num1=`expr 4 + 5`
[root@localhost ~]# echo $num1
9
```

```bash
[root@localhost ~]# expr 5 + 2
7
[root@localhost ~]# expr 5 - 2
3
[root@localhost ~]# expr 5 * 2
expr: syntax error
#一些特殊字符需要转义
[root@localhost ~]# expr 5 \* 2
10
[root@localhost ~]# expr 5 / 2
2
[root@localhost ~]# expr 13 % 5
3
```

```bash
[root@localhost ~]# expr length 123456
6
[root@localhost ~]# expr length 123456*()
9
```

```bash
# 逻辑判断，1是对，0是错

[root@localhost ~]# expr 6 \> 5
1
[root@localhost ~]# expr 6 \< 5
0
```

```bash
# 模式匹配
# 两个特殊符号
# : 计算字符串的字符数量
# .* 任意的字符串重复0次或多次

[root@localhost ~]# expr hello.jpg ":" ".*"
9
[root@localhost ~]# expr hello.jpg ":" ".*j"
7
[root@localhost ~]# expr hello.jpg ":" ".*\.jpg"
9
```



### 拓展 \[] 计算：

```bash
[root@localhost ~]# num=5
[root@localhost ~]# 
[root@localhost ~]# res=$[num+6]
[root@localhost ~]# echo $res
11
[root@localhost ~]# res=$[num*6]
[root@localhost ~]# echo $res
30
[root@localhost ~]# res=$[num%6]
[root@localhost ~]# echo $res
5
```
