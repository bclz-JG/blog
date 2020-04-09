---
title: 检测文件夹内容变化的插件
date: {{date}}
categories: Linux
tags: [skill,Linux]
---

## 正文

---
在部署Blog时，遇到一个很麻烦的问题，每次在windows本地写好的博客，提交到服务器上时，都要手动重启服务，非常麻烦

后来有个朋友介绍给我inotifywait这个插件，运行之后，只要远程提交blog他就会自动重启服务，很好用

需要写一个.sh的脚本

```
#!/bin/bash
echo start instener blog!!""
inotifywait -m -e close_write -r /public/blog/source |
while read events;
do
echo $events; 
hexoboot; //这里可以改成想要执行的命令
done
```

最后让他在后台运行这个.sh就可以
关于在后台运行程序可以参考另一篇 
[后台运行程序](http://blog.bclz.work/blog/Linux/%E5%90%8E%E5%8F%B0%E8%BF%90%E8%A1%8C%E7%A8%8B%E5%BA%8F/)