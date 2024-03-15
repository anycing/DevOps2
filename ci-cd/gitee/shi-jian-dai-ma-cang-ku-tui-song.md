# 实践代码仓库推送

### Git 全局设置

```shell
git config --global user.name "Will"
git config --global user.email "anycing@qq.com"
```



### 无 git 仓库推送

```shell
# 创建代码仓库
mkdir teach_git
cd teach_git
git init 
touch README.md
git add README.md
git commit -m "first commit"

# 关联远程仓库地址至本地别名 origin
git remote add origin git@gitee.com:anycing/teach_git.git
git push -u origin "master"
```



### 已有 git 仓库推送

```shell
cd existing_git_repo
git remote add origin git@gitee.com:anycing/teach_git.git
git push -u origin "master"
```



### 配置 ssh-key 免密推送

#### 生成ssh密钥

```
ssh-keygen -t rsa -b 2048 -N "" -f ~/.ssh/id_git -q
```

#### 添加公钥至gitee

设置 --> 安全设置 --> SSH公钥

```
cat ~/.ssh/id_git.pub
```

#### 配置gitee使用指定的私钥

```
vim ~/.ssh/config
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_git
```

