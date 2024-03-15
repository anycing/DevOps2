# test

### 条件测试常用语法

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>



### 文件类型判断

<table><thead><tr><th width="175">选项</th><th>解释</th></tr></thead><tbody><tr><td>-e</td><td>FILE exists</td></tr><tr><td>-d</td><td>FILE exists and is a directory</td></tr><tr><td>-f</td><td>FILE exists and is a regular file</td></tr><tr><td>-b</td><td>FILE exists and is block special</td></tr><tr><td>-c</td><td>FILE exists and is character special</td></tr><tr><td>-S</td><td>FILE exists and is a socket</td></tr><tr><td>-p</td><td>FILE exists and is a named pipe</td></tr><tr><td>-h</td><td>FILE exists and is a symbolic link (same as -L)</td></tr><tr><td>-L</td><td>FILE exists and is a symbolic link (same as -h)</td></tr><tr><td>-s</td><td>FILE exists and has a size greater than zero</td></tr></tbody></table>

```bash
[root@localhost ~]# ll
total 8
-rw-------. 1 root root 2819 Apr 25 18:38 anaconda-ks.cfg
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Desktop
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Documents
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Downloads
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Music
-rw-------. 1 root root 2099 Apr 25 18:37 original-ks.cfg
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Pictures
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Public
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Templates
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Videos
[root@localhost ~]# 
[root@localhost ~]# test -d Desktop && echo yes || echo no
yes
[root@localhost ~]# test -e Desktop && echo yes || echo no
yes
[root@localhost ~]# test -f Desktop && echo yes || echo no
no
[root@localhost ~]# test -f anaconda-ks.cfg && echo yes || echo no
yes
```



### 文件权限判断

<table><thead><tr><th width="181">选项</th><th>解释</th></tr></thead><tbody><tr><td>-r</td><td>FILE exists and read permission is granted</td></tr><tr><td>-w</td><td>FILE exists and write permission is granted</td></tr><tr><td>-x</td><td>FILE exists and execute (or search) permission is granted</td></tr><tr><td>-u</td><td>FILE exists and its set-user-ID bit is set</td></tr><tr><td>-g</td><td>FILE exists and is set-group-ID</td></tr><tr><td>-k</td><td>FILE exists and has its sticky bit set</td></tr></tbody></table>



### 两个文件之间的比较

<table><thead><tr><th width="184">选项</th><th>解释</th></tr></thead><tbody><tr><td>-nt</td><td>FILE1 -nt FILE2<br>FILE1 is newer (modification date) than FILE2 文件1是否比文件2新</td></tr><tr><td>-ot</td><td>FILE1 -ot FILE2<br>FILE1 is older than FILE2 文件1是否比文件2旧</td></tr><tr><td>-ef</td><td><p>FILE1 -ef FILE2</p><p>FILE1 and FILE2 have the same device and inode numbers 文件1与文件2是否为同一个文件</p></td></tr></tbody></table>

```bash
[root@localhost ~]# ll
total 8
-rw-------. 1 root root 2819 Apr 25 18:38 anaconda-ks.cfg
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Desktop
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Documents
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Downloads
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Music
-rw-------. 1 root root 2099 Apr 25 18:37 original-ks.cfg
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Pictures
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Public
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Templates
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Videos
[root@localhost ~]# 
[root@localhost ~]# test anaconda-ks.cfg -nt original-ks.cfg 
[root@localhost ~]# 
[root@localhost ~]# echo $?
0
[root@localhost ~]# 
[root@localhost ~]# test anaconda-ks.cfg -ot original-ks.cfg 
[root@localhost ~]# 
[root@localhost ~]# echo $?
1
[root@localhost ~]# test anaconda-ks.cfg -ef original-ks.cfg 
[root@localhost ~]# 
[root@localhost ~]# echo $?
1
```



### 两个整数之间的比较

<table><thead><tr><th width="185">选项</th><th>解释</th></tr></thead><tbody><tr><td>-eq</td><td>INTEGER1 -eq INTEGER2 , INTEGER1 is equal to INTEGER2</td></tr><tr><td>-nq</td><td>INTEGER1 -ne INTEGER2 , INTEGER1 is not equal to INTEGER2</td></tr><tr><td>-gt</td><td>INTEGER1 -gt INTEGER2 , INTEGER1 is greater than INTEGER2</td></tr><tr><td>-lt</td><td>INTEGER1 -le INTEGER2 , INTEGER1 is less than or equal to INTEGER2</td></tr><tr><td>-ge</td><td>INTEGER1 -ge INTEGER2 , INTEGER1 is greater than or equal to INTEGER2</td></tr><tr><td>-le</td><td>INTEGER1 -le INTEGER2 , INTEGER1 is less than or equal to INTEGER2</td></tr></tbody></table>



### 字符串的判断

<table><thead><tr><th width="191">选项</th><th>解释</th></tr></thead><tbody><tr><td>-z</td><td>-z STRING , the length of STRING is zero</td></tr><tr><td>-n</td><td>-n STRING , the length of STRING is nonzero</td></tr><tr><td>= 或 ==</td><td>STRING1 = STRING2 , the strings are equal  = 和 == 一样</td></tr><tr><td>!=</td><td>STRING1 != STRING2 , the strings are not equal</td></tr></tbody></table>

```bash
[root@localhost ~]# test -z "" && echo yes || echo no
yes
[root@localhost ~]# test -z " " && echo yes || echo no
no
[root@localhost ~]# test -n "" && echo yes || echo no
no
[root@localhost ~]# test -n " " && echo yes || echo no
yes
[root@localhost ~]#
[root@localhost ~]#
[root@localhost ~]# test "hello" = "world"
[root@localhost ~]# echo $?
1
[root@localhost ~]# test "hello" = "hello"
[root@localhost ~]# echo $?
0
[root@localhost ~]# 
[root@localhost ~]# test "hello" != "world"
[root@localhost ~]# echo $?
0
[root@localhost ~]# test "hello" != "hello"
[root@localhost ~]# echo $?
1
```



### 表达式的判断

<table><thead><tr><th width="195">选项</th><th>解释</th></tr></thead><tbody><tr><td>-a</td><td>EXPRESSION1 -a EXPRESSION2<br>both EXPRESSION1 and EXPRESSION2 are true</td></tr><tr><td>-o</td><td>EXPRESSION1 -o EXPRESSION2<br>either EXPRESSION1 or EXPRESSION2 is true</td></tr><tr><td>!</td><td>! EXPRESSION<br>EXPRESSION is false 取反</td></tr></tbody></table>

```bash
[root@localhost ~]# ll
total 8
-rw-------. 1 root root 2819 Apr 25 18:38 anaconda-ks.cfg
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Desktop
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Documents
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Downloads
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Music
-rw-------. 1 root root 2099 Apr 25 18:37 original-ks.cfg
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Pictures
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Public
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Templates
drwxr-xr-x. 2 root root    6 Apr 27 16:20 Videos
[root@localhost ~]# 
[root@localhost ~]# 
[root@localhost ~]# test -r Desktop/ -a -x Desktop/
[root@localhost ~]# echo $?
0
[root@localhost ~]# 
[root@localhost ~]# test -r anaconda-ks.cfg -a -x anaconda-ks.cfg 
[root@localhost ~]# echo $?
1
[root@localhost ~]# 
[root@localhost ~]# 
[root@localhost ~]# test -x Desktop -o -x anaconda-ks.cfg 
[root@localhost ~]# echo $?
0
[root@localhost ~]# 
[root@localhost ~]# test -x original-ks.cfg -o -x anaconda-ks.cfg 
[root@localhost ~]# echo $?
1
[root@localhost ~]# 
[root@localhost ~]# 
[root@localhost ~]# test ! -x anaconda-ks.cfg && echo yes || echo no
yes
```
