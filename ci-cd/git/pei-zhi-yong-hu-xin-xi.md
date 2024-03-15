# 配置用户信息

### 配置项简介

* \--system    针对任意登陆该Linux系统的用户都生效，git的配置信息写入到 /etc/gitconfig（一般不用）
* \--global     全局，只针对当前登陆的用户生效（当前用户的所有仓库），git配置写入 \~/.gitconfig （用的最多）
* \--local        本地（只针对某一个仓库（文件夹）生效）， /xxx/.git/config

{% hint style="success" %}
优先级排序：

local > global > system

当 local和global 同时设置的时候，local起作用（优先级更高）
{% endhint %}



### 配置实操

#### 配置

```
git config --global user.name 'Will'

git config --global user.email 'anycing@gmail.com'

git config --global color.ui true

# 最小配置：配置用户名(user.name)和邮箱(user.email)，方便别人在做code review时找到你
```

#### 查看

```
git config --system --list

git config --global --list

git config --local --list

git config --list
```
