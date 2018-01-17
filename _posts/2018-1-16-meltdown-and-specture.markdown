---
layout: post
title:  "Meltdown & Specture 熔毁和幽灵漏洞"
date:   2018-1-16 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

官方文档：[https://meltdownattack.com/](https://meltdownattack.com/)

首先引用一句话：

> Vulnerabilities in modern computers leak passwords and sensitive data.

熔断和幽灵揭露了在CPU中存在的关键漏洞。这些硬件漏洞允许程序窃取当前在计算机上处​​理的数据，虽然程序通常不允许从其他程序读取数据，但恶意程序可以利用“熔毁”和“幽灵”来获取存储在其他正在运行的程序的内存中的秘密。这可能包括存储在密码管理器或浏览器中的密码，个人照片，电子邮件，即时消息甚至关键业务文档。

熔断(Meltdown)漏洞利用CPU乱序执行技术的缺陷，破坏了内存隔离机制，使恶意程序可越权访问操作系统内存数据，造成敏感信息泄露。
- 硬件漏洞(Hardware Vulnerability)
- [CVE-2017-5754](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5754)

幽灵(Spectrue)漏洞利用CPU推测执行技术的设计缺陷，破坏了不同应用程序间的逻辑隔离，使恶意应用程序可能获取其他应用程序的私有数据，造成敏感信息泄露。
- 硬件漏洞(Hardware Vulnerability)
- [CVE-2017-5753](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5753)

> Meltdown and Spectre exploit critical vulnerabilities in modern processors. These hardware vulnerabilities allow programs to steal data which is currently processed on the computer. While programs are typically not permitted to read data from other programs, a malicious program can exploit Meltdown and Spectre to get hold of secrets stored in the memory of other running programs. This might include your passwords stored in a password manager or browser, your personal photos, emails, instant messages and even business-critical documents.

> Meltdown and Spectre work on personal computers, mobile devices, and in the cloud. Depending on the cloud provider's infrastructure, it might be possible to steal data from other customers.


