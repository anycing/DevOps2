# Jenkins 结合 Gitlab

{% hint style="danger" %}
&#x20;需要提前安装 Github 插件
{% endhint %}

### 新建构建项

![](<../../.gitbook/assets/image (54).png>)

![](<../../.gitbook/assets/image (39).png>)

![](<../../.gitbook/assets/image (13).png>)

![](<../../.gitbook/assets/image (57).png>)

{% hint style="danger" %}
此处配置为重点，git仓库地址、凭证、构建的分支名称（新版gitlab的主分支名称为main）

凭证的话可以配置ssh-key免密，需要手动把公钥添加到gitlab里（具体配置见gitlab配置章节）
{% endhint %}

![此处为执行的shell命令](<../../.gitbook/assets/image (121).png>)

