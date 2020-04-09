---
title: Not allowed to load local resource
date: 2019-10-8
tags: [拦截器, Java]
categories: Develop
---

### 正文

---
今天有个小伙伴遇到一个难题，图片路径在后台获取到，返回给web却无法显示图片,控制台显示：
*Not allowed to load local resource: file:///D:/images/2001.jpg*
我检查后发现`img`标签的`src`是正确的，直接在浏览器中输入路径，图片也可以在浏览器中正常打开。

忽然想到，先前在做APP接口时，遇到交给APP的图片路径是正确的，但是APP中却无法正常显示，最后经排查，是拦截器将该路径下的请求全部拦截了下来。
这是防止有恶意程序而设置的将一切不属于系统范围的路径全部拦截，此处的img引用的是外部文件夹中的文件，也应当被拦截了下来。

想到这里，便去配了拦截器，开放了存放img的外部目录的路径，具体代码如下：

- webMvcConfigurer
```
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry){
    registry.addResourceHandler(...pathPatterns:"/image/**").addResourceLocations("file:D:/image/");
    //super.addResourceHandlers(resgistry);
}
```
- web
```
<img src="/xxx/image/xxx.jpg>
```

文件保存在`D:\image\`目录下，访问的时候使用虚拟路径`/image/`，比如`x.jpg`，就直接`/image/x.jpg`就OK。
当然也可以写成配置文件的形式，但还是要在类里面读取所以就直接在类里面写了。

### 总结

---
当后台返回的路径正常，前端却无法正常显示的时候，可以优先考虑检查拦截器的配置