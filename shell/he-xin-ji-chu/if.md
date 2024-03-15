# if

### if-then

if-then 语句的基本用法

```bash
if [ 测试条件成立 ] 或 命令返回值是否为0
then 执行相应命令
fi 结束
```



### if-then-else

if-then-else 语句可以在条件不成立时也可以运行相应的命令

```bash
if [ 测试条件成立 ]
then 执行相应命令
else 测试条件不成立，执行相应命令
fi 结束
```

#### Demo

```bash
#!/bin/bash

if [ $USER = root ] ; then
    echo "user root"
    echo $UID
else
    echo "oher user"
    echo $UID
fi
```



### if-elif-else

if-then-else 语句可以在条件不成立时也运行相应的命令

```bash
if [ 测试条件成立 ]
then 执行相应命令
elif [ 测试条件成立 ]
then 执行相应命令
else 测试条件不成立，执行相应命令
fi 结束
```

#### Demo

```bash
#!/bin/bash

if [ $USER = root ] ; then
    echo "user root"
elif [ $USER = will ] ; then
    echo "user will"
else
    echo "oher user"
fi
```



### 嵌套 if 的使用

if 条件测试中可以再嵌套 if 条件测试

```bash
if [ 测试条件成立 ]
then 执行相应命令
    if [ 测试条件成立 ]
    then 执行相应命令
    fi
fi 结束
```
