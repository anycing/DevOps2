# 给其他命令传递参数的过滤器命令xargs

Linux命令可以从两个地方读取要处理的内容：一个是通过命令行参数，一个是标准输入。例如，cat、grep就是这样的命令，举例如下：

```shell
echo 'hello' | cat test_file
```

![](<../../.gitbook/assets/Screen Shot 2023-01-29 at 16.11.18.png>)

这个命令组合中cat会输出test\_file的内容，而不是'hello'字符串，如果test\_file文件不存在，则cat命令报告该文件不存在，而不会尝试从标准输入中读取；echo 'hello'|会通过管道将echo的标准输出（也就是字符串'hello'）导入到cat的标准输入，也就是说此时cat的标准输入中是有内容的，其内容就是字符串'hello'，但是上面的命令中cat并不会从它的标准输入中读入要处理的内容。

标准输入其实是一个缓冲区，从标准输入中读取数据，实际上是从标准输入的缓冲区中读取的。基本上Linux的命令中很多命令的设计都是先从命令行中获取参数，然后从标准输入中读取，例如：

![](<../../.gitbook/assets/Screen Shot 2023-01-29 at 16.11.27.png>)

此时，cat会从其标准输入中读取内容并处理，也就是会输出 'hello'字符串。echo命令将其标准输出的内容 'hello' 通过管道定向到cat的标准输入中。其实还可以这样写这个命令：

![](<../../.gitbook/assets/Screen Shot 2023-01-29 at 16.11.35.png>)

同时指定test\_file和-参数，此时cat程序不但会显示test\_file的内容，还会输出'hello'字符串，也就是说此时cat可以接受第二个标准输入了。

这是cat命令的灵活用法，但并不是所有命令都跟cat一样，可以接受标准输入过来的数据，例如，kill、rm这些程序如果命令行参数中没有指定要处理的内容，则不会默认从标准输入中读取。所以类似

```shell
echo '516' | kill
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-29 at 16.13.57.png" alt=""><figcaption></figcaption></figure>

的写法是不能执行的。同样如下命令也是无法执行的。

```shell
echo 'test' | rm
```

![](<../../.gitbook/assets/Screen Shot 2023-01-29 at 16.16.27.png>)

这两个命令只接受命令行参数中指定的处理内容，不从标准输入中获取处理内容。但有时在实际的运维场景中，经常需要echo '516' | kill这样的效果，如ps-ef | grep 'abc' | kill，也就是筛选出符合某条件的进程PID，然后kill掉。这种需求是很常见的，那么应该怎样达到这样的效果呢？有以下几个解决办法。

1）通过kill ps-ef|grep 'ddd' 命令组合。这种形式是先得到PID，然后执行kill，实际上等同于拼接字符串得到的命令，其效果类似于kill$pid。

2）命令组合如下：

```shell
for procid in $(ps -aux | grep "some search" | awk '{print $2}'); do kill -9 $procid; done
```

这个命令组合与第一种原理一样，只不过是利用for循环的方式，通过ps -aux |grep"some search"|awk '{print$2}'先拿到需要kill的所有PID，然后调用kill-9命令，每次处理一个，循环处理删除。

3）ps -ef | grep 'ddd' | xargs kill命令。下面重点介绍xargs命令。xargs命令可以通过管道接收字符串，并将接收到的字符串通过空格分割成许多参数（默认情况下是通过空格分割），然后将参数传递给其后面的命令，作为后面命令的命令行参数。有了xargs可以批量删除筛选出来的进程，非常简单。

下面来看看xargs如何使用，还是利用上面的那个例子：

```shell
echo '--help' | cat
echo '--help' | xargs cat
```

![](<../../.gitbook/assets/Screen Shot 2023-01-29 at 16.26.09.png>)

这两个命令中，第1个输出的是字符串--help，也就是将echo的内容当作cat处理的文件内容，实际上就是echo命令的输出通过管道定向到cat的输入。然后cat从其标准输入中读取待处理的文本内容。所以这个命令的输出结果为--help。

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-29 at 16.26.31.png" alt=""><figcaption></figcaption></figure>

第2个命令echo '--help' | xargs cat等价于cat --help命令组合。怎么理解呢？就是xargs将其接收的字符串--help做成cat的一个命令参数来运行cat命令。

下面通过几个案例来说明。首先看第1个例子

```shell
touch {1..3}.txt

ls | xargs -t ls -lh

ls | xargs -i -t ls -lh {}
```

![](<../../.gitbook/assets/Screen Shot 2023-01-29 at 18.22.47.png>)

```shell
# 要批量重命名文件夹下的文件，可执行如下操作：

ls | xargs -t -i mv {} {}.bak
```

![](<../../.gitbook/assets/Screen Shot 2023-01-29 at 18.24.46.png>)

对上面操作过程解释如下：

xargs -i：该选项在逻辑上用于接收传递的分批结果。如果不使用-i，则默认是将分割处理后的结果整体传递到命令的最尾部。但是有时候需要传递到多个位置，不使用-i就不知道传递到哪个位置了。例如，重命名备份的时候在每个传递过来的文件名加上扩展名.bak，这需要两个参数位。使用xargs -i时以大括号{}作为替换符号，传递的时候看到{}就将被结果替换。可以将{}放在任意需要传递的参数位上，如果多个地方使用{}就实现了多个传递。

\-t：该选项表示每次执行xargs后面的命令都会先在stderr上打印一遍命令的执行过程，然后才正式执行。类似的还是一个-p选项。使用-p选项是交互询问式的，只有每次询问的时候输入y（或yes）才会执行，直接按〈Enter〉键是不会执行的。使用-p或-t选项就可以根据xargs后命令的执行顺序进行推测，xargs是如何分段、分批以及如何传递的，通过它们有助于理解xargs的各种选项。

<img src="../../.gitbook/assets/Screen Shot 2023-01-29 at 18.28.06.png" alt="" data-size="original">

**继续看第2个例子**，要批量删除目录下扩展名为.txt的文件，可执行如下操作：

```shell
find . -name "*.bak" | xargs -p rm -rf
```

![](<../../.gitbook/assets/Screen Shot 2023-01-29 at 18.31.45.png>)

从这个操作中，可以看出-i、-t和-p参数的作用。

**接着看第3个例子**：默认情况下xargs将其标准输入中的内容以空白（包括空格、Tab、回车换行等）分割成多个之后当作命令行参数传递给其后面的命令并运行，可以使用-d参数指定分隔符，例如：

```shell
echo "11@22@33" | xargs -d "@" echo

echo "11@22@33" | xargs -d "@"
```

<img src="../../.gitbook/assets/Screen Shot 2023-01-29 at 18.37.12.png" alt="" data-size="original">

这里通过参数-d指定了分隔符，所以等价于echo 11 22 33，相当于给echo传递了3个参数，分别是11、22、33。在第二个命令中，去掉了echo，得到的结果跟第一个结果一样，其实xargs命令后面不加命令的话，默认会自动调用echo命令。这可以通过如下方式验证：

```shell
echo "11@22@33" | xargs -d "@" -t
```

<img src="../../.gitbook/assets/Screen Shot 2023-01-29 at 18.38.33.png" alt="" data-size="original">

加上-t参数后，可以清晰看出，xargs默认调用的是echo命令。

**继续看第4个例子**：来看看xargs的分批执行，也就是每次传递几个xargs生成的命令行参数给其后面的命令执行。如果xargs从标准输入中读入内容，然后以分隔符分割之后生成的命令行参数有10个，使用-n 3表示一次传递给xargs后面的命令是3个参数，因为一共有10个参数，所以要执行4次才能将参数用完，例如：

```shell
ls | xargs -n 3 -t
```

<img src="../../.gitbook/assets/Screen Shot 2023-01-30 at 10.15.41.png" alt="" data-size="original">

从输出可以看到，输入有10个参数，每次传递给echo命令3个参数，最后还剩一个，就直接传递一个参数，总共传递了4次。

**再看最后一个例子**：xargs和find同属于一个rpm包findutils，xargs原本就是为find而开发的，它们之间的配合应当是天衣无缝的。一般情况下它们随意结合，按正常方式进行即可。但是当删除文件时，特别需要将文件名含有空白字符的文件纳入考虑。看下面这个例子：

```shell
touch "one space.log"
```

<img src="../../.gitbook/assets/Screen Shot 2023-01-30 at 10.22.51.png" alt="" data-size="original">

这里如果直接交给xargs rm-rf去删除，由于xargs处理后不指定分批选项时以空格分段，所以实际执行的是rm-rf./one space.log，这表示要删除的是当前目录下的one和当前目录下的space.log，这显然是错误的，要真正删除的只是one space.log一个文件而已。

有多种方法可以解决这个问题。思路是让找到的one space.log成为一个段，而不是两个段。这里给出了常见的两种方法。方法一如下：

```shell
find -name "* *.log" -print0 | xargs -0 rm -rf
```

这是通过常用的find的-print0选项使用\0来分隔，而不是\n分隔，再通过xargs-0来配对，保证one space.log的整体性。加上-print0参数表示find输出的每条结果后面加上 '\0'而不是换行，这样，由于-print0后one space.log的前后各有一个\0，但是中间没有文件名。所以上面命令可以执行成功。操作过程如下：

```shell
find -name "* *.log" -print0 | xargs

find -name "* *.log" -print0 | xargs -0 -t

find -name "* *.log" -print0 | xargs -0 rm -rf
```

<figure><img src="../../.gitbook/assets/Screen Shot 2023-01-30 at 10.30.06.png" alt=""><figcaption></figcaption></figure>

这里使用了xargs -0，其实xargs -0的行为和xargs -d基本是一样的，只是-d是指定分隔符，-0是指定固定的\0作为分隔符。因此，xargs -0就是特殊的xargs -d，它等价于xargs -d "\0"。

第二个方法是不在find上处理，而在xargs上处理，只要通过配合-i选项就能保证它的整体性。命令如下：

```shell
find -name "* *.log" | xargs -i rm -rf "{}"
```

注意，最后的大括号必须有双引号。相比较而言，方法一的使用更广泛，而方法二则更具有通用性，对于非find命令（如ls）也可以进行处理。
