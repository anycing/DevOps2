# \[] 条件判断

### \[] 概述

\[] 与 test 作用一样；\[]里面的内容前后需要有空格；test和\[]条件判断时使用变量，必须添加双引号



### 案例

```bash
# 示例
[  "$HOME"  ==  "$MAIL"  ]
[□"$HOME"□==□"$MAIL"□]
 ↑       ↑  ↑       ↑
#----------------------------------------

[root@localhost ~]# [ -z "${HOME}" ] ; echo $?
[root@localhost ~]#
[root@localhost ~]# name="Will D"
[root@localhost ~]# [ $name = "Will" ]
-bash: [: too many arguments
# 原因是表达式被翻译为：[ Will D = "Will" ]
```

```sh
#/bin/bash

read -p "Please input Y/N : " yn

[ "$yn" = "Y" -o "$yn" = "y" ] && echo "OK, continue"
[ "$yn" = "N" -o "$yn" = "n" ] && echo "Oh, interrupt"
echo "I don't know your what your choice is"
```
