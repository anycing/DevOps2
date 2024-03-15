# 自定义四则运算

### 需求

开发一个计算脚步：

要求接受用户输入的数字和运算符，对用户输入信息（数字和运算符）进行判断，最终得出结果



### 脚步实现

```sh
#!/bin/bash

print_useage(){
    printf "Please enter an integer!!!\n"
    exit 1
}

read -p "Please input your number: " firstnum

if [ -n "`echo ${firstnum} | sed 's/[0-9]//g'`" ]
    then
        print_useage
fi

read -p "Please input your operator: " operator

if [ "${operator}" != "+" ] && [ "${operator}" != "-" ] && [ "${operator}" != "*" ] && [ "${operator}" != "/" ]
    then
        echo "只允许输入+|-|*|/"
        exit 2
fi

read -p "Please input second number: " secondnum

if [ -n "`echo ${secondnum} | sed 's/[0-9]//g'`" ]
    then
        print_useage
fi

echo "${firstnum}${operator}${secondnum}结果是：$((${firstnum}${operator}${secondnum}))"
#echo "${firstnum}${operator}${secondnum}结果是：$((firstnum${operator}secondnum))" #也可以
```



### 知识点解析

双小括号的数学运算
