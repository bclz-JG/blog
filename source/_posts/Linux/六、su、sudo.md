---
title: 六、su、sudo
date: 2018-05-12 16:56:24
tags: Linux
categories: Linux
---
# 六、su、sudo
## 6.1 su

su用于用户之间的切换。但是切换前的用户依然保持登录状态。

如果是root 向普通或虚拟用户切换不需要密码，反之普通用户切换到其它任何用户都需要密码验证。


```
su test     :切换到test用户，但是路径还是/root目录
su - test   :切换到test用户，路径变成了/home/test
su          :切换到root用户，但是路径还是原来的路径
su -        :切换到root用户，并且路径是/root
```

**su不足：** 如果某个用户需要使用root权限、则必须要把root密码告诉此用户。

退出返回之前的用户：exit

## 6.2 sudo

命令格式：sudo linux命令

sudo是为所有想使用root权限的普通用户设计的。可以让普通用户具有临时使用root权限的权利。只需输入自己账户的密码即可。以此来弥补su命令的不足，但该管理员权限是临时获取，执行完命令即修复到原用户权限。
