# 查看版本提交日志

### 通过 git log 查看版本演变历史

```bash
git log 
```

#### 查看最近2条历史记录

```bash
git log -2

# 等价

git log -n 2
```

#### 查看简洁版信息

```bash
git log --oneline

git log --pretty=oneline

# 区别在于第一个的哈希值短一些（只显示7位），第二种显示完整的哈希值
```

#### 查看所有分支的版本历史

```bash
git log --all
# 这样显示的信息不存在父子关系，直接看看不出
# 一般加 --graph 参数

git log --all --graph
```

