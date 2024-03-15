# 安装 LAMP LNMP

### 脚本

#### lamp\_or\_lnmp.sh

```bash
#!/bin/bash

path=~/scripts

[ ! -d "${path}" ] && mkdir -p ~/scripts

cat << END
    1. [ install lamp ]
    2. [ install lnmp ]
    3. [ exit ]
END

read -p "Please input the number you want: " num

# 判断用户输入是否是数字
expr $num + 1 &> /dev/null

[ $? -ne 0 ] && {
    echo "The number must be {1|2|3}"
    exit 1
}

# 当用户输入1时
[ $num -eq 1 ] && {
    echo "LAMP is installing..."
    sleep 2

    [ -f "${path}"/lamp.sh ] || {
        echo "file: lamp.sh does not exist"
        exit 1
    }

    source ${path}/lamp.sh
    exit $?
}

# 当用户输入2时
[ $num -eq 2 ] && {
    echo "LNMP is installing..."
    sleep 2

    [ -f "${path}"/lnmp.sh ] || {
        echo "file: lnmp.sh does not exist"
        exit 1
    }

    source ${path}/lnmp.sh
    exit $?
}

# 当用户输入3时
[ $num -eq 3 ] && {
    echo byebye.
    exit 3
}

# 输入1｜2｜3之外的数字时（使用正则处理）
[[ ! $num =~ [1-3] ]] && {
    echo "The number must be {1|2|3}"
    echo "shit"
}
```

#### lamp.sh

```bash
#!/bin/bash

echo "LAMP is installed"
```

#### lnmp.sh

```bash
#!/bin/bash

echo "LNMP is installed"
```



### 文件结构

```bash
[root@localhost scripts]# tree
.
├── lamp_or_lnmp.sh
├── lamp.sh
└── lnmp.sh
```
