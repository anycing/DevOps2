# 代码推送管理

### 代码推送验证方式

* 账号密码
* ssh-key



### 配置 ssh-key 免密推送

#### 生成ssh密钥

```
ssh-keygen -t rsa -b 2048 -N "" -f ~/.ssh/id_gitlab -q
```

#### 添加公钥至gitlab

设置 --> SSH keys

![](<../../.gitbook/assets/image (73).png>)

```
cat ~/.ssh/id_gitlab.pub
```

#### 配置gitlab使用指定的私钥

```
vim ~/.ssh/config
# gitlab
Host 10.0.0.11
HostName 10.0.0.11
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_gitlab
```



### Gitlab代码管理的几种形式

* Linux上没有代码
* Linux上已有代码
* Linux上已经有了git管理的代码仓库

#### 实践Linux上没有代码

```shell
# 克隆远程代码到本地
git clone git@10.0.0.11:cmdb_dev/learn_gitlab.git

# 添加代码
echo 'Add something' > a.txt
git add a.txt
git commit -m 'Add a.txt'

# 推送至远程仓库
git remote # (查看远程仓库别名)
git branch # (查看分支名称)
git push -u origin main # (别名 + 分支)
```

Linux上已有代码的情况同gitee章节中类似



### Gitlab推送分支代码

```
git checkout -b dev_will
echo 'dev_will code' > willcode.txt
git add willcode.txt
git commit -m 'dev_will首次提交'
git push -u origin dev_will
```

![](<../../.gitbook/assets/image (114).png>)

#### 合并分支代码

```
git checkout master
git merge dev_will
git push -u origin main
```



### Gitlab推送标签

```shell
# 打标签
git tag -a 'v1.0' -m 'bea8481添加的标签'

# 查看标签
git log --oneline --decorate --graph

# 推送标签
git push origin v1.0
```

![](<../../.gitbook/assets/image (112).png>)

