# 子串

### 字串说明

<table><thead><tr><th width="229">名称</th><th>描述</th></tr></thead><tbody><tr><td>${变量}</td><td>返回变量值</td></tr><tr><td>${#变量}</td><td>返回变量长度，字符串长度</td></tr><tr><td>${变量:start}</td><td>返回索引start及之后的值</td></tr><tr><td>${变量:start:length}</td><td>返回索引start及之后长度为length的值</td></tr><tr><td>${变量#word}</td><td>返回从变量开头删除最短匹配word后的值</td></tr><tr><td>${变量##word}</td><td>返回从变量开头删除最长匹配word后的值</td></tr><tr><td>${变量%word}</td><td>返回从变量结尾删除最短匹配word后的值</td></tr><tr><td>${变量%%word}</td><td>返回从变量结尾删除最长匹配word后的值</td></tr><tr><td>${变量/pattern/word}</td><td>返回用word替换第一个匹配后的值</td></tr><tr><td>${变量//pattern/word}</td><td>返回用word替换所有匹配后的值</td></tr></tbody></table>

### 案例

#### 输出字符串长度

```bash
str1="Hello, world"
echo ${#str1}
```

![](<../../.gitbook/assets/image (1).png>)

#### 截取字符串

```bash
str1="Hello, world"
echo ${str1:3}
echo ${str1:3:6}
```

![](<../../.gitbook/assets/image (49).png>)

#### 字符串删除

```bash
str1="Hello,hello,Hello,hello"
# 从头删最短匹配
echo ${str1#l}
echo ${str1#H}
echo ${str1#h*l}
echo ${str1#H*l}
```

![](<../../.gitbook/assets/image (48).png>)

```bash
str1="Hello,hello,Hello,hello"
# 从头删最长匹配
echo ${str1##l}
echo ${str1##H}
echo ${str1##h*l}
echo ${str1##H*l}
```

![](<../../.gitbook/assets/image (17).png>)

```bash
str1="Hello,hello,Hello,hello"
# 从尾删最短匹配
echo ${str1%h}
echo ${str1%o}
echo ${str1%h*o}
echo ${str1%H*o}
```

![](<../../.gitbook/assets/image (106).png>)

```bash
str1="Hello,hello,Hello,hello"
# 从尾删最长匹配
echo ${str1%%h}
echo ${str1%%o}
echo ${str1%%h*o}
echo ${str1%%H*o}
```

![](<../../.gitbook/assets/image (52).png>)

#### 字符串替换

```bash
str2="Hello, world, hello, world"
# 替换第一个匹配到的
echo ${str2/world/friend}
# 替换所有匹配到的
echo ${str2//world/friend}
```

![](<../../.gitbook/assets/image (40).png>)
