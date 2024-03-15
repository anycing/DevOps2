# 实践获取远程仓库代码

### 克隆远程仓库到本地

#### 使用ssh-key协议

```
git clone git@gitee.com:anycing/teach_git.git
```

{% hint style="info" %}
需提前配置ssh免密登陆，配置详情见上一章节：

[https://devops.will-say.com/ci-cd/gitee/shi-jian-dai-ma-cang-ku-tui-song#pei-zhi-sshkey-mian-mi-tui-song](https://devops.will-say.com/ci-cd/gitee/shi-jian-dai-ma-cang-ku-tui-song#pei-zhi-sshkey-mian-mi-tui-song)
{% endhint %}

#### 使用https协议

```
git clone https://gitee.com/anycing/teach_git.git
```

{% hint style="success" %}
若获取的代码为开源代码，则不需要验证
{% endhint %}



### 更新本地仓库代码（与远程仓库代码保持一致）

{% hint style="success" %}
适用于远程仓库有更新，本地已经克隆过老版本仓库的情况
{% endhint %}

```
git pull origin "master"

# origin 为远程仓库对应本地仓库的别名
# master 为主分支
```
