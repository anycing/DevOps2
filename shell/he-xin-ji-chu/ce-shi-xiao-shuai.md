# 测试效率

### 测试统计字符串长度的效率

#### 子串

```sh
#!/bin/bash

for num in {1..10000}
do
  str1=`seq -s ':' 100`
  echo ${#str1} &> /dev/null
done
```

![](<../../.gitbook/assets/image (55).png>)



#### | wc -L

```sh
#!/bin/bash

for num in {1..10000}
do
  str1=`seq -s ':' 100`
  echo ${str1} | wc -L &> /dev/null
done
```

![](<../../.gitbook/assets/image (62).png>)



#### expr length

```sh
#!/bin/bash

for num in {1..10000}
do
  str1=`seq -s ':' 100`
  expr length ${str1} &> /dev/null
done
```

![](<../../.gitbook/assets/image (117).png>)



#### | awk

```sh
#!/bin/bash

for num in {1..10000}
do
  str1=`seq -s ':' 100`
  echo ${str1} | awk '{print length($0)}' &> /dev/null
done
```

![](<../../.gitbook/assets/image (70).png>)



{% hint style="warning" %}
Shell 编程，尽量使用 Linux 内置的命令、内置的函数和内置的操作，效率最高，尽量减少管道符的使用。
{% endhint %}

