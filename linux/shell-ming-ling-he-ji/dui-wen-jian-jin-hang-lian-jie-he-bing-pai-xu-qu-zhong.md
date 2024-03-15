# 对文件进行连接、合并、排序、去重

### 文件连接命令join

join命令用于将两个文件中指定列中内容相同的行连接起来。找出两个文件中指定列内容相同的行，并加以合并，再输出到标准输出设备。

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption><p>join命令选项及含义</p></figcaption></figure>

#### 案例1：

```shell
join testfile_1 testfile_2

join testfile_2 testfile_1
```

![](<../../.gitbook/assets/Screen Shot 2023-01-30 at 14.13.55.png>)

<mark style="color:red;">**相同列的内容会被提取到第一列显示**</mark>

#### 案例2：

```shell
join -t ":" -1 3 testfile_3 -2 3 testfile_4
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-30 at 14.38.12.png" alt=""><figcaption></figcaption></figure>

从testfile\_3和testfile\_4中可以看出，testfile\_3中以：分割的第3列和testfile\_4中以：分割的第3列内容相同，因此这两个文件可以合并整合在一起。

可以看出，通过-t选项指定了分隔符后，输出的内容是将testfile\_3文件的第3列和testfile\_4文件的第3列进行整合的结果，两个文件合并后，相同的字段部分被移动到每行最前面了。



### 合并文件列命令paste

paste命令用于合并文件的列。它会把每个文件以列对列的方式，一列列地加以合并。paste比join简单很多，它其实就是直接将两个文件中相同的两行贴在一起，且中间以〈Tab〉键隔开。

#### 案例1：

```shell
paste testfile_3 testfile_4
```

![](<../../.gitbook/assets/Screen Shot 2023-01-30 at 14.48.47.png>)

注：如果文件内容有空行，空行也算内容，如下

![](<../../.gitbook/assets/Screen Shot 2023-01-30 at 14.51.37.png>)

#### 案例2：

```shell
cat /etc/group | paste /etc/passwd /etc/shadow - | head -3
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-30 at 15.02.21.png" alt=""><figcaption></figcaption></figure>

这个例子的重点是-的使用，-代表标准输入，在这里会接收/etc/group的内容，因为通过cat /etc/group将此文件内容送到了标准输入，而-刚好接收了这个内容。因此，这个组合其实是3部分文件内容的合并，是/etc/passwd、/etc/shadow和/etc/group 3个文件内容合并的结果，而每行内容中通过默认的〈Tab〉键隔开。



### 文本内容排序命令sort

sort这个命令很好用，主要用来排序，基本使用格式为：

```shell
sort [-t 分隔符] [-kn1,n2] [-nru]
```

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption><p>sort命令选项及含义</p></figcaption></figure>

#### 案例1：

```shell
cat /etc/passwd | sort | head
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-30 at 15.15.10.png" alt=""><figcaption></figcaption></figure>

这是最简单的一个sort排序，没指定任何参数，所以默认以英文字母顺序进行排序，head表示默认显示前10行数据，要显示指定行数据，可以通过head -N实现。

#### 案例2：

```shell
cat /etc/passwd | sort -t ":" -k3 -n | head
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-30 at 15.17.52.png" alt=""><figcaption></figcaption></figure>

这个例子是通过指定分隔符 ":" 对指定的列进行排序，k3表示以 ":" 作为分隔符的第3列，也就是以第3列为准进行排序，由于第3列都是数字，所以还需要-n参数。



### 检查并删除文件中的重复行命令uniq

uniq主要用于检查及删除文本文件中重复出现的行，一般与sort命令结合使用，常用命令选项及含义见下表

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption><p>uniq命令选项及含义</p></figcaption></figure>

#### 案例1：

```shell
uniq testfile_6

uniq testfile_6 -c
```

![](<../../.gitbook/assets/Screen Shot 2023-01-30 at 15.30.20.png>)

可以看到有重复行，并且重复行都是相邻的，要删除重复行，uniq就派上用场了，如果要统计重复行出现的次数，加上-c参数即可。

#### 案例2：

上面的testfile\_6文件有些特殊，因为实际使用中，重复行不可能都是相邻在一起的，那继续来看另一个文件的内容：

```shell
cat testfile_7

uniq testfile_7
```

这是一个重复行不相邻的文件，实际环境中，很多都是类似这样的文件，再通过uniq看看是否能够删除重复行。

![](<../../.gitbook/assets/Screen Shot 2023-01-30 at 15.38.18.png>)

可以看到，文件原样输出了，也就是说uniq对这些重复行不相邻的内容无能为力。怎么办呢？sort可以解决。sort是排序用的，那就先把这个文件进行排序，这样，重复行就自动相邻了，操作如下：

```shell
sort testfile_7

sort testfile_7 | uniq

sort testfile_7 | uniq -c
```

经过sort排序后，重复行相邻了，接着通过管道后面接uniq命令即可过滤删除重复行了

![](<../../.gitbook/assets/Screen Shot 2023-01-30 at 15.40.43.png>)

果然，经过sort排序后，uniq又可以正常工作了，这也是为什么sort经常和uniq一起使用的原因了。

#### 案例3：

下面这个例子使用了uniq的-d参数，也就是显示重复行有哪些：

```shell
sort testfile_7 | uniq -d
```

![](<../../.gitbook/assets/Screen Shot 2023-01-30 at 15.45.01.png>)

注：只显示重复行，只出现一次的不显示
