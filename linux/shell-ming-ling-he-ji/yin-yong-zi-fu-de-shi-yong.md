# 引用字符的使用

在Bash中有很多特殊字符，这些字符本身就具有特殊含义，如果在shell的参数中使用它们，就会出现问题。Linux中使用了“引用”技术来忽略这些字符的特殊含义，引用技术就是通知shell将这些特殊字符当作普通字符处理。

shell中用于引用的字符有转义字符 **\\**、单引号 **' '**、双引号 **" "**。



### 转义字符 \\

如果将 **\\** 放到特殊字符前面，shell就忽略这些特殊字符的原有含义，将其当作普通字符对待，例如：

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-19 at 15.59.00.png" alt=""><figcaption></figcaption></figure>

上面是将abc?\*重命名为abc，将C:\backup重命名为backup。因为文件名中含有特殊字符，所有都使用了转义字符\。



### 单引号 ' '

```shell
# 将字符串放到一对单引号之间，那么字符串中所有字符的特殊含义将被忽略，例如：
mv C\:\\backup backup
mv 'C:\backup' backup

# 上面两条命令完全等效，此时第二条命令也可以使用双引号
```



### 双引号 ""

双引号的引用与单引号基本相同，包含在双引号内的大部分特殊字符可以当作普通字符处理

```shell
mv "C:\backup" backup
mv 'C:\backup' backup

# 上面两条命令完全等效
```

但是仍有一些特殊字符即使用双引号括起来，也仍然保留自己的特殊含义，例如，**$** 、 **\\** 和 **\`** 。看下面几个例子：

```shell
str1="The \$SHELL current shell is $SHELL"
echo $str1

str2="\$$SHELL"
echo $str2
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-19 at 16.17.55.png" alt=""><figcaption></figcaption></figure>

从上面输出可以看出，**$** 和 **\\** 在双引号内仍然保留了特殊含义。继续看下面例子：

```shell
str="This hostname is `hostname`"
echo $str
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-19 at 16.21.24.png" alt=""><figcaption></figcaption></figure>

上面的输出中，字符 **\`** 在双引号中也保留了自己的特殊含义。

注：使用 **$()** 也能达到和 **\`\`** 相同的效果

```shell
str3="This hostname is $(hostname)"
echo $str3
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-19 at 16.27.29.png" alt=""><figcaption></figcaption></figure>

