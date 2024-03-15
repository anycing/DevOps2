# DDNS

### AliyunDDNS

#### 阿里云CLI

```shell
# 此方式需要借助于“阿里云CLI”，故而需先安装“阿里云CLI”
# 参考官方文档：https://help.aliyun.com/document_detail/121544.html

# 然后使用“阿里云CLI”进行登陆信息的配置
# 获取 RAM KEY ，访问：https://ram.console.aliyun.com/overview
aliyun configure

# 获取 RecordId
aliyun alidns DescribeDomainRecords --DomainName will-say.com
```

#### 完整 shell

```
#!/bin/sh
localWanIP=$(curl -s ip.sb)

aliyun alidns UpdateDomainRecord --RecordId 787127134678477824 --RR home-router --Type A --Value $localWanIP --Line default >> ./aliyunDDNS.logs
```



### 3322 DDNS

```
curl -u user:password "http://members.3322.net/dyndns/update?system=dyndns&hostname=anycing.f3322.org"
```
