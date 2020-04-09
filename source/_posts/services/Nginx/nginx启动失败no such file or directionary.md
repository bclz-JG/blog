---
title: nginx启动失败
date: 2020-3-17
tags: [Nginx, Linux]
categories: services
---

### 正文

---
启动Nginx失败，问题：nginx: [error] open() "/etc/nginx/logs/nginx.pid" failed (2: No such file or directory)

找不到指定的pid文件

用指定conf的方式启动

`nginx -c /etc/nginx/nginx.conf`