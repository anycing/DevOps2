# git 命令

### 常用命令

#### 把文件交给git追踪

```
git add <file>
```

```
# -u 把Git已经跟踪的文件一起提交到暂存区
git add -u
```

#### 重命名文件

```
git mv readme readme.md
git commit -m 'Rename readme to readme.md'
```

#### 恢复暂存区里的文件

```
git checkout <file>

# 将暂存区里的版本恢复到本地
```

#### 从本地仓库中删除对某个文件的跟踪

```
git rm --cached hi.txt

# 将文件，回到未跟踪的状态

# --cached参数只从暂存区删除，不删除本地文件
# 不加--cached参数会直接删除本地文件
```

#### 撤回刚才的git操作

```
git reset HEAD <file>
```



### 版本管理

#### 版本回退

```
git log # 查看版本记录
```

```
# 回退方式一：回退到指定版本
git reset --hard <ID>

# 回退方式二：回退到上一个版本或上上个版本
git reset --hard HEAD^
git reset --hard HEAD^^
```

#### 版本穿梭

```
git reflog  # 查看git记录的每次提交与回退日志
```

```
git reset --hard <ID>  # 穿梭到指定ID版本
```

```
git log --oneline
```



### git stash 实践

{% hint style="info" %}
将暂存区还未提交的内容临时存放到一个区域，方便日后取回来再用
{% endhint %}

#### 保存暂存区，工作区进度到stash区

```
git stash save "注释"
```

#### 查看stash保存的列表以及id

```
git stash list
```

#### 恢复stash进度

```
# 恢复到最新的stash进度到工作区

git stash pop


# 恢复指定的stash进度

git stash pop stash_id
```

#### 清空所有存储的stash进度

```
git stash clear
```

#### 删除指定id的stash进度

```
git stash drop stash_id
```



### git 分支

#### 查看当前的分支情况

```
git branch
```

#### 创建一个分支（表示该员工可以使用该分支，进行自己的独立空间的代码管理）

```
git branch will
```

```
# 创建分支并切换到该分支
git checkout -b will
```

#### 切换到新创建的分支

```
git checkout will

# 在该分支下创建文件，提交暂存区，提交版本。（此时该文件就提交到了该分支下的版本空间内）
echo 'test will branch' > will.txt
git add will.txt
git commit -m '我是will分支下的第一次版本提交'
```

#### 切回master分支并查看版本日志

```
git checkout master

git log
# 返回结果中无刚才commit的版本（因为刚才是在新分支下commit的）
```

#### 合并分支并查看版本日志

{% hint style="success" %}
技术老大认可了分支所写代码，允许合并到master上，则进行分支合并操作
{% endhint %}

```
git merge will

git log
```

#### 删除分支

{% hint style="success" %}
当分支的提交被合并后，该分支就可以删除了，随时用分支随时创建即可
{% endhint %}

```
# 在代码未进行合并的情况下使用 -D 进行强制删除
git branch -D will

# 合并完代码后使用 -d 进行分支删除
git branch -d will
```



### git tag

#### 对当前最新的版本记录加上一个标签

```
git tag -a 'v1.0' -m '开发完了xxx.py的xxx功能'

# -a 标签名称
# -m 注释信息
```

#### 给指定版本记录加标签（带上id即可）

```
git tag -a 'v0.9' 84228d6 -m '开发完了a.py的xxx功能'
```

#### 查看标签

```
git tag
```

#### 查看带标签的版本记录信息

```
git log --oneline --decorate --graph
```

#### 查看标签的具体信息（含版本记录）

```
git show v1.0
```

#### 删除标签

```
git tag -d v1.0
```



### git remote

#### 查看远程仓库在本地的别名

```
git remote

# -v 显示别名对应的url
git remote -v
```
