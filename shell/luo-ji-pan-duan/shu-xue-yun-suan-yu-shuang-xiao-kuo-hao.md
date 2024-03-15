# 数学运算与双小括号

### 双小括号 (()) 的操作方法

<table><thead><tr><th width="228">运算操作符与运算命令</th><th>含义</th></tr></thead><tbody><tr><td>((i=i+1))</td><td>运算后赋值法，将 i+1 的运算结果赋值给变量 i ，输出其值时要使用 echo $((i=i+1))</td></tr><tr><td>i=$((i+1))</td><td>可以在(())前加$符，表示将表达式运算后赋值给i</td></tr><tr><td>((8>7&#x26;&#x26;5==5))</td><td>可以进行比较操作，还可以加入逻辑与和逻辑或，用于条件判断</td></tr><tr><td>echo $((2+1))</td><td>需要直接输出运算表达式的运算结果时，可以在(())前加$符</td></tr></tbody></table>

###

### 案例

#### 逻辑运算真假的区别，真为1，假为0

```bash
[root@localhost ~]# echo $((8>7))
1
[root@localhost ~]# echo $((6>7))
0
```

#### 逻辑与的用法 &&

```bash
[root@localhost ~]# echo $((8>7&&6<7))
1
[root@localhost ~]# echo $((8>7&&6>7))
0
[root@localhost ~]# echo $((8<7&&6<7))
0
[root@localhost ~]# echo $((8<7&&6>7))
0
```

#### 加减乘除

```bash
[root@localhost ~]# echo $((2+3))
5
[root@localhost ~]# echo $((2-3))
-1
[root@localhost ~]# echo $((2*3))
6
[root@localhost ~]# echo $((2/3))
0
[root@localhost ~]# echo $((7/2))
3
[root@localhost ~]# echo $((2**3))
8
[root@localhost ~]# echo $((7%4))
3
```

#### 结合变量计算

```bash
[root@localhost ~]# num=5
[root@localhost ~]# 
[root@localhost ~]# ((num*3))
[root@localhost ~]# 
[root@localhost ~]# echo $num
5
[root@localhost ~]# 
[root@localhost ~]# ((num=num*3))
[root@localhost ~]# 
[root@localhost ~]# echo $num
15
[root@localhost ~]# echo ((num=num*3))
-bash: syntax error near unexpected token `('
[root@localhost ~]# echo $((num=num*3))
45
```

#### 复杂的数学运算

```bash
[root@localhost ~]# ((a=2+2**3-4%3))
[root@localhost ~]# 
[root@localhost ~]# echo $a
9
[root@localhost ~]# 
[root@localhost ~]# b=((2+2**3-4%3))
-bash: syntax error near unexpected token `('
[root@localhost ~]# 
[root@localhost ~]# b=$((2+2**3-4%3))
[root@localhost ~]# 
[root@localhost ~]# echo $b
9
```

#### 不使用变量赋值的计算

```bash
[root@localhost ~]# ((2+2**3-4%3))
[root@localhost ~]# 
[root@localhost ~]# echo ((2+2**3-4%3))
-bash: syntax error near unexpected token `('
[root@localhost ~]# 
[root@localhost ~]# echo $((2+2**3-4%3))
9
```



### 拓展坑

#### ++ -- 操作符

```bash
[root@localhost ~]# a=5
[root@localhost ~]# b=5
[root@localhost ~]# 
[root@localhost ~]# echo $((a++))
5
[root@localhost ~]# echo $a
6
[root@localhost ~]# 
[root@localhost ~]# echo $((++b))
6
[root@localhost ~]# echo $b
6
```

{% hint style="info" %}
\++ -- 在前，先操作命令，再赋值给变量

\++ -- 在后，先赋值给变量，再操作命令
{% endhint %}



