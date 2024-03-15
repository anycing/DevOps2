# 批量修改文件名

### 需求

有如下一批文件，现在需要删除掉png格式的文件中的 "\_finished" 字符串，写脚本实现

```bash
# 文件创建命令
touch my_{1..5}_finished.jpg
touch my_{1..5}_finished.png
```

![](<../../.gitbook/assets/image (123).png>)



### 脚本内容

```sh
#!/bin/bash

for pic_name in `ls *.png`
do
  mv ${pic_name} ${pic_name//_finished/}
done
```



### 知识点解析

{% hint style="success" %}
使用子串的字符串替换功能：<mark style="color:orange;">**`${变量//pattern/word}`**</mark> 返回用word替换所有匹配后的值
{% endhint %}
