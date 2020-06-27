## Ansible安装前准备
### 一、简介
Ansible更加简洁的自动化运维工具，不需要在客户端上安装agent，基于Python开发。可以实现批量操作系统配置、批量程序的部署、批量运行命令。

**特点：**
1. no agents：不需要在被管控主机上安装任何客户端；
2. no server：无服务器端，使用时直接运行命令即可；
3. modules in any languages：基于模块工作，可使用任意语言开发模块；
4. yaml，not code：使用yaml语言定制剧本playbook；
5. ssh by default：基于SSH工作；
6. strong multi-tier solution：可实现多级指挥。

**优点：**
1. 轻量级，无需在客户端安装agent，更新时，只需在操作机上进行一次更新即可；
2. 批量任务执行可以写成脚本，而且不用分发到远程就可以执行；
3. 使用python编写，维护更简单，ruby语法过于复杂；
4. 支持sudo。

### 二、安装准备
#### 2.1、准备两台机器 Centos6.7_64，这两台机器都关闭 selinux，清空 iptables 规则并保存。
```
web9：101.200.131.184
web10：101.200.148.30
```
#### 2.2、设置 hostname
```
[root@iZ25cepqmeiZ ~]# vim /etc/sysconfig/network
NETWORKING=yes
NETWORKING_IPV6=no
PEERNTP=no
GATEWAY=101.200.131.247
HOSTNAME=web9.gz.com
```
```
[root@iZ28sspgp53Z ~]# vim /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=web10.gz.com
NETWORKING_IPV6=no
PEERNTP=no
GATEWAY=101.200.151.247
HOSTNAME=web10.gz.com
```
#### 2.3、编辑 hosts 文件
```
[root@web9 ~]# vim /etc/hosts
127.0.0.1 localhost
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
10.170.178.91 iZ25cepqmeiZ
101.200.131.184 web9.gz.com
101.200.148.30 web10.gz.com
```
重启一下,关闭防火墙
#### 2.4、安装Ansible，只在服务端安装即可
```
[root@web9 ~]# yum install -y epel-release
[root@web9 ~]# yum install -y ansible
```
#### 2.5 SSH密钥配置
```
[root@web9 ~]# mkdir /root/.ssh  创建秘钥目录
[root@web9 ~]# chmod 700 /root/.ssh  给秘钥权限 
[root@web9 ~]# ssh-keygen -t rsa  生成秘钥对
Generating public/private rsa key pair.   回车
Enter file in which to save the key (/root/.ssh/id_rsa):  输入目录所在的路径，默认的在这里。我直接回车，没有设置。
Enter passphrase (empty for no passphrase):       我是做实验就没设置密码，为了安全建议设置密码。
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
ad:4d:49:52:db:18:32:55:56:13:26:d1:2b:f8:93:3c root@web9.gz.com
The key's randomart image is:
+--[ RSA 2048]----+
| o.+.=+=. |
| + * o.. |
| . +.. . |
| +... . |
| S +o o |
| + E |
| . . o |
| |
| |
+-----------------+
```
#### 2.6、查看生成的文件
```
[root@web9 ~]# ls -la /root/.ssh/
total 16
drwx------ 2 root root 4096 Jul 15 21:19 .
dr-xr-x---. 5 root root 4096 Jul 15 21:18 ..
-rw------- 1 root root 1671 Jul 15 21:19 id_rsa
-rw-r--r-- 1 root root 398 Jul 15 21:19 id_rsa.pub
[root@web9 ~]# cat /root/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA1sFP/y5BZwNT6NmqQOcBgIkJ2UruStCTMRgzZgS0N0OjZlOx037tu5ANNrvJY7CjvWzGoYGElhxIqw4RS5JD6CzQklZfVw7fIsoCREH3qUMmNdlvCMKSB0EQK/iL5TEcuceKRJNhqomG6Mpg4bEHlbCiNHnr9WmDneRSAzTaMo6cSUsxpq1O29aOLsMuSNU6WqGdfDfI/RaWNp446/7ccAa1Z/SlUMktnVPGcj/GZsATBaoR+4baQogVfy8Cfi6yAxlub4TL+ttth4V40hQpaytTIuMjwU5E0rL6BI4zFwn2TTNBYOjS1OZmB6m4nD4iGdpIQZEpNZMyHOIMGF73YQ== root@web9.gz.com
```

#### 2.7、把公钥（id_rsa.pub）内容放到本机和远程客户机的 /root/.ssh/authorized_keys 里面本机
```
[root@web10 ~]# mkdir /root/.ssh
[root@web10 ~]# chmod 600 /root/.ssh
[root@web10 ~]# vim /root/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA1sFP/y5BZwNT6NmqQOcBgIkJ2UruStCTMRgzZgS0N0OjZlOx037tu5ANNrvJY7CjvWzGoYGElhxIqw4RS5JD6CzQklZfVw7fIsoCREH3qUMmNdlvCMKSB0EQK/iL5TEcuceKRJNhqomG6Mpg4bEHlbCiNHnr9WmDneRSAzTaMo6cSUsxpq1O29aOLsMuSNU6WqGdfDfI/RaWNp446/7ccAa1Z/SlUMktnVPGcj/GZsATBaoR+4baQogVfy8Cfi6yAxlub4TL+ttth4V40hQpaytTIuMjwU5E0rL6BI4zFwn2TTNBYOjS1OZmB6m4nD4iGdpIQZEpNZMyHOIMGF73YQ== root@web9.gz.com
```
### 三、验证一下有没有成功
#### 3.1、首先要关闭防火墙
```
[root@web9 ~]# yum install -y openssh-clients
```
#### 3.2、在web9上登录web10
```
[root@web9 ~]# ssh web10.gz.com
The authenticity of host 'web10.gz.com (101.200.148.30)' can't be established.
RSA key fingerprint is f9:a9:c3:da:3d:fa:ba:21:e1:66:f9:f9:d7:83:ec:4d.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'web10.gz.com,101.200.148.30' (RSA) to the list of known hosts.
Welcome to aliyun Elastic Compute Service!
```

#### 3.3、在web10上登录web9
```
[root@web10 ~]# ssh web9.gz.com
The authenticity of host 'web9.gz.com (101.200.131.184)' can't be established.
RSA key fingerprint is e6:09:c6:91:92:6c:7f:98:db:d4:9e:2c:80:25:ab:5d.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'web9.gz.com,101.200.131.184' (RSA) to the list of known hosts.
root@web9.gz.com's password:
Welcome to aliyun Elastic Compute Service!
```

#### 3.4、退出
```
[root@web10 ~]# logout
Connection to web10.gz.com closed.
[root@web9 ~]#
```
</br>
</br>
[文章来源]
* [90root工作室](https://www.90root.com/post/23.html)
