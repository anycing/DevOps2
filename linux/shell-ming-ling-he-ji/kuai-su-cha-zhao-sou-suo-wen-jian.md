# 快速查找、搜索文件

### find

find命令用来在指定的路径下查找指定的文件。其格式如下：

```shell
find path-name [-options] [-print -exec -ok 命令 {} \;]
```

* path-name：find命令查找的目录路径，例如，可以用.表示当前目录，用/表示系统根目录。
* \-options：find命令用来控制查找的方式。这里列出-options选项常见的几种格式，见下表

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption><p>find命令选项及含义</p></figcaption></figure>

* \-print：将查找结果输出到标准输出。
* \-exec：对查找出的符合条件的文件执行所给出的Linux命令，而不询问用户是否需要执行该命令。{}表示shell命令的选项即为所查找到的文件。命令的末尾必须以；结束。

**注意：**格式要正确，-exec命令{};，在}和\之间一定要有空格才行。

* \-ok：对查找出的符合条件的文件执行所给出的Linux命令。与-exec不同的是，它会询问用户是否需要执行该命令。

另外find命令可以使用 **glob风格通配符**

#### 案例：

```shell
# 在系统根目录下，查找文件类型为普通文件，
# 属于will用户的，2天以前的，
# 并且不包含/usr/bin目录的文件名为main.c的文件，并将结果输出到屏幕。

find / -path "/usr/bin" -prune -o -name "main.c" -user will -type f -mtime +2 -print

# -prune 必须和 -path， -o 一起使用
# -prune -o 的顺序不 能调换
# -name等必须放在-prune -o后面才能使用
```

```shell
# 对上例中查找的结果进行删除操作

find / -path "/usr/bin" -prune -o -name "main.c" -user will -type f -mtime +2 -print -exec rm {} \;
```

```shell
# 在系统根目录下查找不在/var/log和/usr/bin目录下的，名为“main.c”的普通文件

find / \( -path "/var/log" -o -path "/usr/bin" \) -prune -o -name "main.c" -type f -print

# \ 表示引用，告诉shell不对后面的字符做特殊解释，而留给find命令去解释其意义
# \(-path中，在（和-path之间是有空格的，同时/usr/bin\）在bin和\之间也是有空格的
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-29 at 11.37.49.png" alt=""><figcaption></figcaption></figure>

```shell
# 查找系统中所有大小为0的普通文件，并列出它们的完整路径

find / -type f -size 0 -exec ls -al {} \;
```

```shell
# 查找系统/var/log目录中修改时间在7天以前的普通文件，
# 然后以交互方式删除

find /var/log/ -type f -mtime +7 -ok rm {} \;
```

```shell
# 在当前目录及子目录下查找所有*.txt文件

find ./ -name "*.txt" -print
```

```shell
# 在用户自己的根目录下查找文件名以一个大写字母开头，
# 紧接着是一个小写字母和两个数字，
# 最后以.txt结尾的文件

find / -name "[A-Z][a-z][0-9][0-9]*.txt" -print
```
