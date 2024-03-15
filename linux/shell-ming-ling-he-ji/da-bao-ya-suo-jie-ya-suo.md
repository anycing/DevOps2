# 打包、压缩、解压缩

### tar

tar是Linux下经常使用的归档工具，可以对文件或者目录进行打包归档，归成一个文件，但是并不进行压缩。其格式如下：

```shell
tar [主选项+辅助选项] 文件或目录
```

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption><p>tar命令主选项含义</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>tar命令辅助选项含义</p></figcaption></figure>

#### 打包

```shell
# 将/etc目录下的所有文件打包，并显示打包的详细文件，
# 设置打包文件名为etc.tar，同时保存文件到/opt目录下。

tar -cvf /opt/etc.tar /etc

# 这里的档案名为etc.tar，档案名可以随意起，
# Linux上利用tar命令打包出来的档案文件一般用.tar作为标识。
```

#### 打包并压缩

```shell
# 将/etc目录下的所有文件打包并使用gzip压缩，然后显示打包的详细文件，
# 设置打包文件名为etc.tar.gz，同时保存文件到/opt目录下。

tar -cvzf /opt/etc.tar.gz /etc


# 将/etc目录下的所有文件打包并使用bzip2压缩，然后显示打包的详细文件，
# 设置打包文件名为etc.tar.bz2，同时保存文件到/opt目录下。

tar -cvjf /opt/etc.tar.bz2 /etc


# 打包备份test1、test2、test3目录，但是不备份test1下的test目录

tar --exclude=test1/test -cvzf test.tar.gz test1 test2 test3
```

```shell
# 打包备份test1、test2、test3目录，但是不备份test1下的test目录

tar --exclude=test1/test -cvzf test.tar.gz test1 test2 test3
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-28 at 14.43.11.png" alt=""><figcaption></figcaption></figure>

```shell
# 打包备份/var/log/sa目录中2023/01/17以后的文件

tar -N "2023/01/17" -cvzf sa.tar.gz /var/log/sa
```

#### 查看压缩包内容

```shell
# 查阅上面/opt/etc.tar.gz压缩包的内容

tar -tvzf /opt/etc.tar.gz


# 查阅上面/opt/etc.tar.bz2压缩包的内容

tar -tvjf /opt/etc.tar.bz2


# etc.tar.gz可能包含很多个文件，无法一个屏幕显示完毕，
# 这时可以使用more命令，例如，tar -tvzf /opt/etc.tar.gz | more
```

#### 解压缩

```shell
# 将/opt/etc.tar.gz解压到/usr/local/src下

tar -xvzf /opt/etc.tar.gz -C /usr/local/src/

# 不使用-C指定解压目录则会解压到当前目录


# 仅解压/opt/etc.tar.gz压缩文件中的etc/tcsd.conf文件
tar -xvzf /opt/etc.tar.gz etc/tcsd.conf
```

```shell
# 将/etc目录打包压缩后直接解压到/opt目录下，而不生成打包的档案文件

tar -cvzf - /etc | tar -xvzf - -C /opt

# 第一个紧跟在f后面的-是将创建的档案文件输出到标准输出上，
# | 在Linux下表示管道符，
# 第二个紧跟在f后面的-表示将tar命令通过管道传入的档案文件作为需要解压的数据来源
```



### gzip/gunzip

gzip/gunzip表示将一般的文件进行压缩或者解压。压缩文件预设的扩展名为.gz，其实gunzip就是gzip的硬链接，因此无论是压缩或者解压都可以通过gzip来实现。

<mark style="color:red;">**注意：**</mark>gzip只能对文件进行压缩，不能压缩目录，即使指定压缩的目录，也只能压缩目录内的所有文件。

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>gzip命令选项含义</p></figcaption></figure>

#### 压缩

```shell
# 将/etc目录下的所有文件以及子目录进行压缩，
# 备份压缩包etc.zip到/opt目录，
# 然后对etc.zip文件进行gzip压缩，设置gzip的压缩级别为9

zip -r /opt/etc.zip /etc
gzip -9v /opt/etc.zip
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-28 at 18.56.08.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-28 at 18.56.29.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-28 at 18.56.41.png" alt=""><figcaption></figcaption></figure>

#### 查看压缩文件

```shell
gzip -l /opt/etc.zip.gz
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-28 at 19.02.48.png" alt=""><figcaption></figcaption></figure>

#### 解压

```shell
gzip -d /opt/etc.zip.gz

gunzip /opt/etc.zip.gz

# 以上命令等价
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-28 at 19.04.53.png" alt=""><figcaption></figcaption></figure>



### bzip2/bunzip2

bzip2/bunzip2表示对文件进行压缩与解压缩。此命令类似于gzip/gunzip命令，只能对文件进行压缩。只能压缩目录下的所有文件，压缩完成后，在目录下生成以.bz2为扩展名的压缩包。bunzip2其实是bzip2的符号链接，即软链接，因此压缩解压都可以通过bzip2实现。

<figure><img src="../../.gitbook/assets/image (90).png" alt=""><figcaption><p>bzip2命令选项及含义</p></figcaption></figure>

#### 压缩

```shell
# 将/opt目录下的log.zip、log2.zip进行压缩，
# 设置压缩级别为最高，
# 同时在压缩完毕后不删除原始文件，
# 显示压缩过程的详细信息。

bzip2 -9vk /opt/log.zip /opt/log2.zip

# 压缩完毕后，在/opt下就会生成相应的log.zip.bz2、log2.zip.bz2文件
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-29 at 10.46.58.png" alt=""><figcaption></figcaption></figure>
