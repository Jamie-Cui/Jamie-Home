---
layout: post
title:  "TLS Heartbleed （心脏出血漏洞）"
date:   2018-1-15 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

**心脏出血漏洞**，也被称为心血漏洞，是一个出现在加密程序的OpenSSL的程序错误，与2014年底被发现。简单的来讲该程序库广泛用于传输层安全（TLS）协议，针对的是客户端和服务器双方，因此只要运行了这个有bug的OpenSSL程序，入侵者就可以利用这个漏洞进行攻击。心脏出血漏洞的原理是在TLS的心跳扩展中没有应用正确的输入校验（due to a missing bonus check）。

- 该漏洞的类型是 buffer over-read （缓冲区过读...其实和buffer overflow原理相同）
- CVE-2014-0160

<img src="{{site.url}}{{site.baseurl}}/img/heartbleed-flow.png" alt="Drawing" style="width: 400px;"/>　*- 简单心脏出血漏洞图解*


