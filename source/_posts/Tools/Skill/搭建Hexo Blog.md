---
title: 使用Hexo服务搭建自己的Blog
date: 2019-5-5
tags: [skill,tools]
categories: Tools
---

### 正文

---
相信很多小伙伴都想拥有自己的博客，尤其对于计算机专业的同志们，csdn等之类的博客虽然非常流行，但是样式，设计过于统一，很大一部分童鞋希望可以自己设计自己的Blog，在这里我向大家推荐一个很好用的工具——Hexo

### 基础环境安装

---
本次是在CentOS 7.6环境下部署

- Git安装

Hexo环境依赖于Git环境，首先我们需要安装Git。已经安装的小伙伴可以跳过这一节进入到下一阶段。
如果输入命令：
````
git --version
````
无法显示git版本信息，则看以下教程：
**git yum安装**
执行命令：
````
yum -y install git
````
ps: 卸载系统自带git
````
yum remove git
````
但是yum安装的git版本较低。如需安装高版本的git，我们可以选择使用源码安装。
git源码安装
（1）安装依赖的包
````
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
````
（2）下载git源码并解压
````
wget https://github.com/git/git/releases/tag/v2.11.0    //版本可能会更替，自行在github上找一下哈
tar zxvf git-2.11.0.tar.gz
cd git-2.11.0
````
（3）编译安装
````
make prefix=/usr/local/git all
make prefix=/usr/local/git install
````
（4）查看git
````
git --version
````
（5）配置环境变量
````
vim /etc/profile
加入export PATH=$PATH:/usr/local/git/bin
source /etc/profile
生效配置文件
````
- node.js安装

请参考另一篇：[biu~](http://blog.bclz.work/blog/services/Node.js/CentOS7%E5%88%A9%E7%94%A8yum%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85node/)

- hexo安装

执行命令
````
npm install hexo-cli -g
````
````
mkdir  /root/hexo (根据自己的情况创建hexo博客存放的文件夹)
cd /root/hexo
hexo init
````
此时hexo博客的基础目录已经创建，需要进行相应配置，[参照此处](https://hexo.io/zh-cn/docs/)

- 主题下载

这里有一个比较完整的主题“yelee”大家可以参考，参照文档 -> [biu~](http://moxfive.coding.me/yelee/)

### hexo常用命令

---
- 启动hexo（可指定启动端口，默认4000）

````
hexo s -p {port}
````

- 清除缓存

````
hexo clean
````

- hexo启动后台脚本

````
nohup hexo s > my.log 2>&1 &
````
新建一个run.sh脚本在hexo博客根目录下，输入上述脚本，执行:
````
chmod +x run.sh   (获得执行权限)
./run.sh           （执行脚本）
````
此时，hexo处于后台启动，后台启动脚本参照此处 -> [biu~]()