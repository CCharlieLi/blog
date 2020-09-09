---
title: Android Development Environment(no L included)
date: "2014-01-12T23:46:37.121Z"
template: "post"
draft: false
slug: "Android"
category: "Tech"
tags:
  - "Android"
  - "Tech"
description: "Android开发这玩意儿一会儿不碰你就得倒腾一阵子！还记得刚刚碰Android开发的时候才出到2.2，当时Eclipse+ADT+SDK都弄得妥妥的，然后就没有然后了，谁知去年第二次碰它的时候MD已经是4.+，各种升级一堆的问题啊！！！最近出了个Android Studio更是蛋疼啊！还不如Eclipse啊！！！"
socialImage: "/media/image-book.jpg"
---

Android开发这玩意儿一会儿不碰你就得倒腾一阵子！

还记得刚刚碰Android开发的时候才出到2.2，当时Eclipse+ADT+SDK都弄得妥妥的，然后就没有然后了，谁知去年第二次碰它的时候MD已经是4.+，各种升级一堆的问题啊！！！最近出了个Android Studio更是蛋疼啊！还不如Eclipse啊！！！

于是乎，写一个各种问题的解决方案备忘：

- 自从升级到ADT2.2以后都可能遇到过找不到R.java的问题（蛋疼地让我倒腾了好久），问题在于SDK和ADT一定要更新到最新！妈蛋！Eclipse的clean和build倒是其次！

- 有时候更新都更新不了啊！！！是因为伟大的GFW吗！！！SDK根本找不到啊！全是Fail啊！解决方法：

1. 在`SDK Manager`菜单`Tools`->`Options`打开了SDK Manager的`Settings`，选中`Force https://… sources to be fetched using http://…`，强制使用http协议。            

2. 修改`hosts`文件。Windows在`C:\WINDOWS\system32\drivers\etc`目录下，Linux用户打开/etc/hosts文件。添加： 
    

```shell
203.208.46.146 www.google.com
74.125.113.121 developer.android.com
203.208.46.146 dl.google.com
203.208.46.146 dl-ssl.google.com  
```

- 再reload搞定！！！


- 这还不是最惨的啊！！！最惨的是JAVA 7的问题啊！！！java 7不完整啊！Android环境根本跑不了啊！莫名其妙的问题出来了你连原因都不知道啊！！！网上说那些莫名其妙的解决方案也都不行啊！

- 还是google靠谱，解决方法就是：换回JAVA 6啊！！！！

世界又美好了有没有啊！！！

----------------------

最近发布的x86 64-bit Andoid L 还没有尝试过，不过我也在尝试用其他方法开发Android应用。