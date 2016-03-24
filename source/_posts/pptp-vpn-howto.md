title: 极速搭建一个私有pptp vpn
date: 2016-02-25 17:30:19
tags:
    - docker
    - vpn
    - pptp
---
现在网络上卖vpn的暴利，1毛前成本卖100块。我们还是来自己动手自己搭一个吧。大约需要十分钟。
<!-- more -->

当然，这里只是说省一些钱。

免费=体验差

1. 先挑选一个VPS供应商。例如[digitalocean](https://www.digitalocean.com/)、[vultr](https://www.vultr.com/)等等。这一类厂商大多给新用户有一些优惠，比如注册送50刀，100刀什么的。
2. 由于只是当梯子用，所以基本上最低端的配制就行。这里我选的是vultr的
<center>
![img](http://7xr6gl.com1.z0.glb.clouddn.com/vultr_vps.png)
</center>
每月1TB带宽，不挂机下载基本用不完。看视频的话基本上可以每天看两三个小时的1080p。
节点的话看自己需求选择日本、欧洲或者北美的。
50刀的券免费用10个月还是很划算的。

3. 我们先来装一个docker，详见[docker.io](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

```
$ sudo apt-get update
$ sudo apt-get install linux-image-extra-$(uname -r)
$ sudo apt-get install apparmor
$ sudo apt-get update
$ sudo apt-get install docker-engine
$ sudo service docker start
$ sudo docker run hello-world
```
见到下图说明docker安装ok了
<center>
![img](http://7xr6gl.com1.z0.glb.clouddn.com/docker_helloworld.png)
</center>

4. 在docker hub上找个pptp vpn容器，这里用的是[mobtitude/vpn-pptp](https://hub.docker.com/r/mobtitude/vpn-pptp/)

5.创建账户文件
```
mkdir -p /etc/ppp
touch /etc/ppp/chap-secrets
echo "# Secrets for authentication using PAP" >> /etc/ppp/chap-secrets
echo "# client    server      secret      acceptable local IP addresses
" >> /etc/ppp/chap-secrets
echo "stan    *           smith    *" >> /etc/ppp/chap-secrets

```
[账户验证文件详情](https://hub.docker.com/r/mobtitude/vpn-pptp/)

6.启动pptp服务
```
docker run -d --privileged --net=host -v /etc/ppp/chap-secrets:/etc/ppp/chap-secrets mobtitude/vpn-pptp

```

7.Enjoy your own ladder.
