---
layout: post
title:  "WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!"
date:   2017-02-09 10:17:31 +0800
categories: jekyll update
---
![](http://upload-images.jianshu.io/upload_images/3538284-5fbde0a2f0be8d7d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 更新时间: 2017-02-10 
今天又出现了相同的问题, 检查发现是服务器的问题, 重启服务器就好了(大写加粗的囧字,尴尬五分钟)

![尴尬...](http://upload-images.jianshu.io/upload_images/3538284-814055a93083f569.gif?imageMogr2/auto-orient/strip)

# 问题
---
> 今天使用`git`更新本地服务器的代码时, 出现了如下错误, 此处记录一下解决方法 和用到的几条终端命令

```
Air:operation4ios sungrow$ git pull
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:8lUkY9ticgAJrMjZIS8nD/PhJgJY2PGYYZr3kFWrTr4.
Please contact your system administrator.
Add correct host key in /Users/sungrow/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/sungrow/.ssh/known_hosts:9
RSA host key for 192.168.64.216 has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

# 解决步骤
---
### 1. 重启服务器
### 2. 使用`ssh-keygen -l -f ~/.ssh/known_hosts` 命令查看`known_hosts`里的所有认证信息

>  Air:operation4ios sungrow$ ssh-keygen -l -f ~/.ssh/known_hosts

```
2048 SHA256:Ozg57nLbtZoA89n/N95+1iwJpyDgehOYtHrVIQVF4MY s1.cyssh.com,103.6.85.132 (RSA)
256 SHA256:FQGC9Kn/eye1W8icdBgrQp+KkGYoFgbVr17bmjey0Wc git.oschina.net,218.60.114.30 (ECDSA)
256 SHA256:FQGC9Kn/eye1W8icdBgrQp+KkGYoFgbVr17bmjey0Wc 218.11.0.86 (ECDSA)
2048 SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8 github.com,192.30.253.113 (RSA)
256 SHA256:qfBHoh4vTfFHbgaD3BQf/ZwO6BPfUdBLLzIJTXW2QcY 192.168.64.160 (ECDSA)
256 SHA256:FQGC9Kn/eye1W8icdBgrQp+KkGYoFgbVr17bmjey0Wc 116.211.167.14 (ECDSA)
2048 SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8 192.30.253.112 (RSA)
256 SHA256:qfBHoh4vTfFHbgaD3BQf/ZwO6BPfUdBLLzIJTXW2QcY 192.168.64.59 (ECDSA)
256 SHA256:qfBHoh4vTfFHbgaD3BQf/ZwO6BPfUdBLLzIJTXW2QcY 192.168.64.216 (ECDSA)
```

### 3. 使用vim编辑器`vim ~/.ssh/known_hosts`, 找到和当前IP地址相同的信息然后删掉
> Air:operation4ios sungrow$ vim ~/.ssh/known_hosts

```
192.168.64.160 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL2myNoDAkyW0aeTsCJmkHPn2bV3MHfiD8xfOpCkQtomE43zbENYDLFCallpPNXr3cHHUekoCkvFpueqwfbY6Ro=
116.211.167.14 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMuEoYdx6to5oxR60IWj8uoe1aI0X1fKOHWOtLqTg1tsLT1iFwXV5JmFjU46EzeMBV/6EmI1uaRI6HiEPtPtJHE=
192.168.64.216 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL2myNoDAkyW0aeTsCJmkHPn2bV3MHfiD8xfOpCkQtomE43zbENYDLFCallpPNXr3cHHUekoCkvFpueqwfbY6Ro=
```

### 4. 使用`ssh-keygen -R 服务器IP地址`命令重新连接
> Air:operation4ios sungrow$ ssh-keygen -R 192.168.64.216

```
# Host 192.168.64.216 found: line 9
/Users/sungrow/.ssh/known_hosts updated.
Original contents retained as /Users/sungrow/.ssh/known_hosts.old
Air:operation4ios sungrow$ vim ~/.ssh/known_hosts
Air:operation4ios sungrow$ ssh-keygen -R 192.168.64.216
Host 192.168.64.216 not found in /Users/sungrow/.ssh/known_hosts
```

### 5. 此时可以正常使用`git`命令
> 
例: 使用 `git pull`命令, 更新代码
Air:operation4ios sungrow$ git pull
依次输入yes 和服务器密码后, 一切又都可以正常使用了

```
The authenticity of host '192.168.64.216 (192.168.64.216)' can't be established.
ECDSA key fingerprint is SHA256:qfBHoh4vTfFHbgaD3BQf/ZwO6BPfUdBLLzIJTXW2QcY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.64.216' (ECDSA) to the list of known hosts.
Password:
Already up-to-date.
```

# 后记
---
> 论终端使用的熟练程度, 我只服我们老大,   哈哈哈~~~
简直是出神入化, 6到飞起  -----------来自一位菜鸟程序员的感慨
🖥
 ⌨️ 🖱

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


