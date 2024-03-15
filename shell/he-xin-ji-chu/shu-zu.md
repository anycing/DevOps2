# 数组

### 数组的赋值与引用

#### 定义数组

* IPTS=(10.0.01 10.0.0.2 10.0.0.3)

#### 显示数组的所有元素

* echo ${IPTS\[@]}

#### 显示数组元素个数

* echo ${#IPTS\[@]}

#### 显示数组的第一个元素

* echo ${IPTS\[0]}



### Demo

```bash
[root@localhost ~]# ipts=(10.0.0.1 10.0.0.2 10.0.0.3)
[root@localhost ~]# echo $ipts
10.0.0.1
[root@localhost ~]# echo ${ipts[@]}
10.0.0.1 10.0.0.2 10.0.0.3
[root@localhost ~]# echo ${#ipts[@]}
3
[root@localhost ~]# echo ${ipts[0]}
10.0.0.1
[root@localhost ~]# echo ${ipts[1]}
10.0.0.2
[root@localhost ~]# echo ${ipts[2]}
10.0.0.3
```

