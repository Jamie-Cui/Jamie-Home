---
layout: post
title:  "Network Security"
date:   2017-12-03 12:38:16 +0000
categories: jekyll update
---
这一篇我们要讲一些关于网络安全的种种，和上一篇一样，先讲述一下关于 `Computer networking` 的一些基础知识。

**URL---最基础的诈骗**

URL 的全名叫做(Uniform Resource Locators)， it is a standardized format for describing the location and access method of resources via the internet.

我们常见的URL格式是这样的:

`<scheme>://<user>:<password>@<host>:<port>/<url-path>?<query-string>`

举个例子:
[https://profile.facebook.com][facebook-profile]

在这里https就是运行的协议（超文本传输协议）; 没有用户和密码信息，注意这里指的用户和密码并不是指我们facebook用户的用户名和密码，而是facebook服务器管理员登陆facebook服务器来对网页进行维护时使用的用户名和密码; host 是profile.facebook.com，在这里com指的是商业网站（commercial），facebook指的是网站名称，而profile指的是facebook的一个分支服务器的名称； emmmm这个例子是很久之前的了，facebook好像已经修改了他们的网站结构。接下来是一个典型URL诈骗的例子：

`http://taobao.alibab.com`

（注意这个域名是无效的，但是不要去访问这个域名以防被不法人员注册），我们可以看到他的host为`taobao.alibab.com`,从后往前分析，商业网站(com)，网站名称(alibab)，子域名(taobao)，不注意的话很难发现他与真正URL的区别，运用这个网站，入侵者就可以执行一系列web入侵。

Anyway，举这个例子的目的就是以后大家不要被假冒的URL欺骗了！！！在访问网站之前一定要留心看一眼URL地址是不是对的

**DNS---最常见的网络攻击**




[facebook-profile]:https://profile.facebook.com