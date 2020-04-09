---
title: 没有那个文件或目录
date: 2020-2-1 12:12:12
tags: [Linux]
categories: Linux
---

### 正义

---
为了作一个小工具，在Linux下手写了一个.sh脚本，在执行时却报出：
**-bash: ./listeneHexo.sh: /bin/bash^M: 坏的解释器: 没有那个文件或目录**
的错误。

### 分析

---
研究发现是由于windows和unix编译方式不同引起的差异

### 判断方法

---

1. cat -A filename 从显示结果可以判断，dos格式的文件行尾为^M$，unix格式的文件行尾为$；
2. od -t x1 filename 如果看到输出内容中存在0d 0a的字符，那么文件是dos格式，如果只有0a，则是unix格式；
3. vi filename打开文件，执行 : set ff，如果文件为dos格式在显示为fileformat=dos，如果是unxi则显示为fileformat=unix。
4. vim test.sh
   :set ff?
   如果出现fileforma＝dos那么就是dos格式

### 解决方法

---
1. 使用linux命令dos2unix filename，直接把文件转换为unix格式；
2. 使用sed命令sed -i "s/\r//" filename 或者 sed -i "s/^M//" filename直接替换结尾符为unix格式；
3. vi filename打开文件，执行 : set fileformat=unix 设置文件为unix，然后执行:wq，保存成unix格式。

我使用的是1方法判断，3方法解决的。
### 后记

---
我使用vim来写的脚本，但还是出现了dos混入的问题，可能由于shell也是dos客户端的原因吧（以后找到原因了再来补上）.
