# 宝塔面板

**安装宝塔**
Centos安装命令：

```
yum install -y wget && wget -O install.sh https://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
```


内网面板地址: https://92.118.229.241:21829/5c7f057d
username: 5drubuxc
password: ac67fcdf


# vpn 搭建步骤

[科学上网服务器搭建 不再受限于人！（v2ray/xray/trojan） | SKY博客](https://www.sky350.com/576.html)

 这个教程少了一步非常重要的地方，就是要在宝塔面板里面开放你的 v-ui 的端口，这样你才能进入 x-ui 的面板


命令行命令为(以80端口为例， 完成后需重启 vps)：

```
firewall-cmd --query-port=80/tcp --zone=public  #查询80端口是否开启

firewall-cmd --zone=public --add-port=80/tcp --permanent #添加80端口
```



另外， 在 x-ui 中新建了节点之后，需要 设置你服务器节点的 流量，然后在宝塔面板里面开放你的 vpn 的端口


# 服务器管理
[Login - DediPath](https://portal.dedipath.com/clientarea.php?action=productdetails&id=62859)

Hostname: linux.690b8f.com
ip: 92.118.229.241

# x-ui 管理
http://92.118.229.241:54321/xui/inbounds
- caoshangfei
- 123456



# VPS 加速
[Vultr速度慢？Vultr一键开启BBR、BBRplus、魔改版BBR、锐速加速教程 - Vultr优惠网](https://www.vultryhw.cn/how-to-speed-up-vultr-vps/)



# 域名及 dns 解析

域名：   caoshangfei.online
DNS 服务器：
	- frederic.dnspod.net
	- raisins.dnspod.net

客户端证书
```
// key format
PEM

// CERTIFICATE
-----BEGIN CERTIFICATE-----
MIIEFTCCAv2gAwIBAgIUGS5JakAGHh0F+8mh7DS4vpqCCkgwDQYJKoZIhvcNAQEL
BQAwgagxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQH
Ew1TYW4gRnJhbmNpc2NvMRkwFwYDVQQKExBDbG91ZGZsYXJlLCBJbmMuMRswGQYD
VQQLExJ3d3cuY2xvdWRmbGFyZS5jb20xNDAyBgNVBAMTK01hbmFnZWQgQ0EgNzUz
NmFkYzNiZTVlNWNiZTc5NzdkMjU5Mjk3MWQ0NzEwHhcNMjMwNDA1MDI1OTAwWhcN
MzMwNDAyMDI1OTAwWjAiMQswCQYDVQQGEwJVUzETMBEGA1UEAxMKQ2xvdWRmbGFy
ZTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKogqMT3VfN4YLFwTqpp
1qvVVGgoKAWm0l8B6F+8ZHokEBuFTP0ra8GhnV3hLu2j7xdwkkmoP+xHCEcHPRi2
pB/6xYGQdiXL6AbEnaWVfEWGjrogjZVAg/3fPHFs0ru5ShbOU6d4ZwHpgHQIZsgk
6idlCjuXg3A/V0VUyKYwwIIRLl70yH1fnPuOCiTiDOdQ0aT9swkqcltTjffyKhWa
gvIvyY+OIxpqZdISZpf5Gb/RhXR4sGpRR10557PHKBqzh+Z0X+iD/3fyZoC9tNrC
zLfqBhOpdTeHdgQmX+oru5DCXSMM3jG08uLcEDwanv7VHUCMWbvek6cRG1olrZY2
wU0CAwEAAaOBuzCBuDATBgNVHSUEDDAKBggrBgEFBQcDAjAMBgNVHRMBAf8EAjAA
MB0GA1UdDgQWBBS8YlsjvMumggZ4fsi7qqxcdeeyPDAfBgNVHSMEGDAWgBRrJ1mA
p7ACKHRUkk1DqXJXbVymezBTBgNVHR8ETDBKMEigRqBEhkJodHRwOi8vY3JsLmNs
b3VkZmxhcmUuY29tLzhiYmRiNDAzLWIzZGMtNDI3MS05MzBjLTMyMDM4NDdjZjM3
Ny5jcmwwDQYJKoZIhvcNAQELBQADggEBACmBFRLojXxhbSa8snhBZ1jjn259mRcL
MBChsaGCETl24dESDiJ3tCsnn8RPI3dBdMLRBO9DFn44Yso2RfVoe2Q56n2BReKZ
bLgEuYL/gBhGQGWYelpTRbw8jxU3TdqEyaFWUTwM1O3IJVwEFIb3hHigwq9ZGzBW
ySGtE45rRuqxomev4I4qurfBxf4awzsPQyG6ee0G1dnEyC4/F5VsYt2w5mp8zZdp
Tb9k7AY9NyBKZ7KMs62PbOYfh/c2FfXkKWOoXpZkkdCszip2Zo13hzpR3poQTSwo
B2sl1zKNhrO044vxzX49HnNCoLSHv7AwwbIpoEcwmox2mj6+rENGVS8=
-----END CERTIFICATE-----


// PRIVATE KEY
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQCqIKjE91XzeGCx
cE6qadar1VRoKCgFptJfAehfvGR6JBAbhUz9K2vBoZ1d4S7to+8XcJJJqD/sRwhH
Bz0YtqQf+sWBkHYly+gGxJ2llXxFho66II2VQIP93zxxbNK7uUoWzlOneGcB6YB0
CGbIJOonZQo7l4NwP1dFVMimMMCCES5e9Mh9X5z7jgok4gznUNGk/bMJKnJbU433
8ioVmoLyL8mPjiMaamXSEmaX+Rm/0YV0eLBqUUddOeezxygas4fmdF/og/938maA
vbTawsy36gYTqXU3h3YEJl/qK7uQwl0jDN4xtPLi3BA8Gp7+1R1AjFm73pOnERta
Ja2WNsFNAgMBAAECggEABSOWwC8R0Jif9cbaXs7IUPPREz219R2/QxFwBIb7U1ky
3LH1IRH7tPioeUUrwWVEpqTxi6S3xu+1B02SVznFJsDQ0INy84dU9fILKmJOmQcM
hADRu2LHL0GxYLe/JzPcp2hfdzKYN02SDMSS3j7cjTNse1+t9RO6E3pmfpFVxK1V
jmjvSwwC7f9dNjHzgVMLmydbxBb+F2+2b+4KJC5ZcNRain8KdqNXfH2YuRfdEGKV
kNAgNTMrSMPlty7tDpqH7zZuvp3YHZw7WRyHX7xjASqG0p13iFSpmad87jmENhKJ
rhpTdpmbfqZyEshLPX55bA0TmFqkjxGOa4UWCYoD/QKBgQDdPpPIa4a+xsuGpjFv
1neyzZGQWnNcy04vtJCPqd1DUBCjVFZKoFrf4JLsfimKQv/8FZ8MQSb6EG1cXw5Q
xcamxQmmHCa8Hk4XlRZtNVmG58DBrlSgk7Kr29oSJtyk8gFPKOog8hkwqlqESy4y
DU45cU8M4wHCbq4WAToIytvTiwKBgQDE2mY2FSB337gOIlaQJE2vY7vCt1MDqWRY
O01VruBXfYJUM1dGOGTa1q0N54+SjxjsuBL31BIOuF2HsyAA0P3LDQYf3UWA+XkU
VvUbjwcR+P/CM2Dhguq9L1sZbXjuEYglILa7O2sJss1nZtqp1cLcEaSKABmxUR02
x2BBs5j5hwKBgCq1SLvYneM35tPwQvzOzr5yVlYiT8Cq8kXdOkaxSKgUzZFp11qC
h+hNpq8GT6iD9HxKBDDOZuLAxwucwduvwgAxawJozsVjqDl/Kxwbv1N+a0Q4DdgN
iuEICJmWbONeYAhS4pdHhLtTNzPwe1NoJCCMkfDv5UgOK4bN59EIr9SvAoGBALxE
wJKi/A2J2scx0iZDgkTnFtEwceXDoSO9e8Yh3Y/virEq9SJElzixLoto1uhtkYH8
vq9llOudKl65UzdUqhYD28Kn5mxmrIVmcT+tOC7ZPQqoBtVHV2genXshNxJBlDsm
IX3KYyHAbzCgryrVNWsyOHJ/jBMJJ+6XGplbwkeLAoGBANWoxigSU4hH+TGuN8HS
7DtlHyBbfT2ntIfsf1K7u9FbFSt43p/5YnjujyICTlFAI+bxFYU+Gh9zJXqpbBYA
+DWfaFr4KbhEtWkSzBePaDq/u4dcS72lTbGVmZZxfAAmoZ44DMFdfHx5nTqmEiND
xWiqGf0gbOwswIPzsGclROO5
-----END PRIVATE KEY-----


```
