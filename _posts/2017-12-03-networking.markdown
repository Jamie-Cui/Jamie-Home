---
layout: post
title:  "Network Security"
date:   2017-12-04 12:00:00 +0000
categories: jekyll update
---
这一篇我们要讲一些关于网络安全的种种，和上一篇一样，先讲述一下关于 Computer networking 的一些基础知识。

# **URL --- 最基础的诈骗**

URL 的全名叫做(Uniform Resource Locators)， it is a standardized format for describing the location and access method of resources via the internet.

我们常见的URL格式是这样的:

`<scheme>://<user>:<password>@<host>:<port>/<url-path>?<query-string>`

举个例子:
[https://profile.facebook.com][facebook-profile]

在这里https就是运行的协议（超文本传输协议）; 没有用户和密码信息，注意这里指的用户和密码并不是指我们facebook用户的用户名和密码，而是facebook服务器管理员登陆facebook服务器来对网页进行维护时使用的用户名和密码; host 是profile.facebook.com，在这里com指的是商业网站（commercial），facebook指的是网站名称，而profile指的是facebook的一个分支服务器的名称； emmmm这个例子是很久之前的了，facebook好像已经修改了他们的网站结构。

接下来是一个典型URL诈骗的例子：[http://taobao.alibab.com][fake-url] 不要点！

（注意这个域名是无效的，但是不要去访问这个域名以防被不法人员注册），我们可以看到他的host为 *taobao.alibab.com* , 从后往前分析，商业网站(com)，网站名称(alibab)，子域名(taobao)，不注意的话很难发现他与真正URL的区别，运用这个网站，入侵者就可以执行一系列web入侵。

Anyway，举这个例子的目的就是以后大家不要被假冒的URL欺骗了！！！在访问网站之前一定要留心看一眼URL地址是不是对的

# **DNS --- 最常见的网络攻击**

网络上我们平时所说的网页地址都是域名地址，在真实的网络之中区分设备的唯一地址就是IP地址。因此在网络中就有了一系列对域名和IP地址进行解析的服务器---DNS服务器。而针对DNS服务的种种攻击可以造成目标断网等等。

首先我们先看一下2012年叙利亚断网事件, 来源 [CloudFlare][CloudFlare]：

简单来说在2012年11月29号，叙利亚忽然和全世界其他网络失去了连接，从那一刻开始所有叙利亚的IP地址停止了对外界的访问和响应。和[埃及][Egypt-cutoff]的例子不同，埃及断网是政府主动关闭了互联网的连接，也就是说关闭了ISP对外的连接，但是埃及内使用国外ISP的依然可以连接。但是叙利亚是全国完全被隔绝了，政府表示这和他们没有关系，这是一种典型的针对DNS服务器的一种攻击，下图展示了叙利亚的网络连接情况（概念图来源 [CloudFlare][CloudFlare]）。
<img src="{{site.url}}{{site.baseurl}}/img/Syria.png" alt="Drawing" style="width: 600px;"/>

在这张图中 29386节点是叙利亚的主节点，可以认为是叙利亚全国对外界互联网的主要节点或者AS。

我们可以看出来所有的网络流量都经过 9123 和 3491 两个关键节点连接的叙利亚网络。这两个节点分别是意大利和土耳其的电信运行商 PCCW 和 Turk ，就是他们两个运营商提供了叙利亚与全世界其他地方的连接。通过网络监控员的发现，就是因为叙利亚丢失了与这两个节点的连接而导致了叙利亚全国“失联”。同样的如果我们将 9123 和 3491 认为是 29386 的DNS server，就可以认为这是一个典型的DNS断网攻击了（孤立目标服务器），以下是DNS攻击的定义：

**DNS Servers are soft targets for attackers, take out the mapping and the website goes “offline”**

# **番外介绍** 

在继续介绍下去之前我们要先提出一个攻击模型: man-in-the-middle-attack, 概念图可以看下面这一张，假设用户是Alice，用户想访问的网站（服务器）是Bob，我们可以看到charlie就是中间人。在这个示例中，charlie作为网络中间人可以: 

嗅探网络流量(view traffic); 更改网络流量(change traffic); 随意发送网络流量(add traffic); 随意删除网络流量(delete traffic)。

听起来很恐怖是不是，但是大多数情况下charlie扮演的角色并不是入侵者，而是我们耳熟能详的人：

网络提供商(Internet Service Provider)； VPN供应商(VPN Provider); 公共WIFI提供商(WIFI provider); 弱鸡的网络管理员(Incompetent admin); 入侵者(Attacker)

经常性的我们不知道为什么会有免费的VPN服务商，咖啡店为什么会有免费WIFI（这个其实还好，毕竟买了咖啡hahahah）。实际上在我们通过免费VPN去访问外界连接的时候我们就会遇到 man-in-the-middle attack，免费VPN服务商会在我们访问网上的一些网站时更改我们获取到的网络流量（植入广告链接等）以此来达到盈利的目的。但是实际上这个模型考虑的是一个被动的入侵者(Passive Attacker)，不会主动入侵服务器等等，但实际情况千差万别。Note: 这个模型在密码学中经常用到。

<img src="{{site.url}}{{site.baseurl}}/img/man-in-the-middle.png" alt="Drawing" style="width: 600px;"/>    

# **Dos --- 最“著名”的攻击**

接下来就到了我们的明星“Dos”攻击，相信即使没有学习过计算机安全甚至没有学过计算机的人也多少有所耳闻这种攻击。Dos全名为拒绝服务攻击(Denial of Service)。

"Denial of service is typically accomplished by flooding the targeted machine or resource with superfluous requests in an attempt to overload systems and prevent some or all legitimate requests from being fulfilled"  ------  "Understanding Denial-of-Service Attacks". US-CERT. 6 February 2013. Retrieved 26 May 2016."

因此Dos攻击的方式是通过向目标网络中注入大量无用的重复信息导致目标网络阻塞，进而瘫痪来达到断网的目的。常见的Dos攻击分为一下几类：SYN Flooding, Spoofing and Smurfing（真的不知道中文怎么翻译）。

**SYN Flooding**

据统计，在所有黑客攻击事件中，SYN攻击是最常见又最容易被利用的一种攻击手法。相信很多人还记得2000年YAHOO网站遭受的攻击事例，当时黑客利用的就是简单而有效的SYN攻击，有些网络蠕虫病毒配合SYN攻击造成更大的破坏。本文介绍SYN攻击的基本原理、工具及检测方法，并全面探讨SYN攻击防范技术。[百度百科][syn-flooding-baidu]


<img src="{{site.url}}{{site.baseurl}}/img/Dos.png" alt="Drawing" style="width: 600px;"/>  

[CloudFlare]:https://blog.cloudflare.com/how-syria-turned-off-the-internet/
[Egypt-cutoff]:https://blog.cloudflare.com/what-egypt-shutting-down-the-internet-looks-l/
[facebook-profile]:https://profile.facebook.com
[fake-url]:http://taobao.alibab.com
[syn-flooding-baidu]:https://baike.baidu.com/item/SYN%E6%94%BB%E5%87%BB/14762413?fr=aladdin