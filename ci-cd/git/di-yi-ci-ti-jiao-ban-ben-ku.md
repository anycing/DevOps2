# 第一次提交版本库

```
# 生成 git 工作区
git init learn_git

# 查看 git 工作区中的本地仓库
cd learn_git
ls -a

# 查看工作区的信息（文件的变动状态，未跟踪、已跟踪）
git status

# 在工作区进行文件创建，发生一些变化
echo 'hello' > hello.txt
git status  # 此时git提示，是否要git add添加到暂存区

# 确认要添加跟踪的文件
git add .
git status  # git提示是否要提交到本地仓库

# 
git commit -m 'Will's first commit'  # -m git提交的注释信息
git status
```
