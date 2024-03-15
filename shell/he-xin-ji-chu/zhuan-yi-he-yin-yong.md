# 转义和引用

### 特殊字符

特殊字符：一个字符不仅有字面意义，还有元意（meta-meaning）

* \#注释
* ; 分号
* \ 转义符号
* " 和 ' 引号



### 转义符号

单个字符前的转义符号

* \n    \r    \t    单个字母的转义
* \\$    \\"    \\\    单个非字母的转义



### 常用的引用符号

* " 双引号 （不完全引用，会解释变量）
* ' 单引号 （完全引用，不解释变量）
* \` 反引号



### Demo

```bash
[root@localhost ~]# var1=1223456
[root@localhost ~]# echo '$var1'
$var1
[root@localhost ~]# echo "$var1"
1223456
```