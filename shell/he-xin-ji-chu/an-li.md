# 案例

### $\* 与 $@ 区别

```sh
#!/bin/bash

echo 'print each param from "$*" :'
for i in "$*"
do
    echo $i
done

echo 'print each param from "$@" :'
for j in "$@"
do
    echo $j
done
```

#### 输出：

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>
