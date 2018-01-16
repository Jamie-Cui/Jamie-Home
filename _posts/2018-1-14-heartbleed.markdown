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

**心脏出血漏洞**，也被称为心血漏洞，是一个出现在加密程序的OpenSSL的程序错误，于2014年底被发现。简单的来讲该程序库广泛用于传输层安全（TLS）协议，针对的是客户端和服务器双方，因此只要运行了这个有bug的OpenSSL程序，入侵者就可以利用这个漏洞进行攻击。心脏出血漏洞的原理是在TLS的心跳扩展中没有应用正确的输入校验（due to a missing bonus check）。

- 该漏洞的类型是 buffer over-read （缓冲区过读...其实和buffer overflow原理相同）
- [CVE-2014-0160](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0160)
- [“心脏出血”漏洞代码](https://git.openssl.org/gitweb/?p=openssl.git;a=commitdiff;h=96db9023b881d7cd9f379b0c154650d6c108e9a3)

<img src="{{site.url}}{{site.baseurl}}/img/heartbleed-flow.png" alt="Drawing" style="width: 400px;"/>　*- 简单心脏出血漏洞图解*

这次漏洞问题在于 ssl/dl_both.c 文件中。OpenSSL Heartbleed模块存在一个BUG，当攻击者构造一个特殊的数据包，满足用户心跳包中无法提供足够多的数据会导致memcpy把SSLv3记录之后的数据直接输出，该漏洞导致攻击者可以远程读取存在漏洞版本的openssl服务器内存中长大64K的数据。

SSL(Secure Socket Layer)安全套接层，TLS(Transport Layer Security)传输层安全是为网络通信提供安全以及数据完整性校验的一种安全协议。TLS和SSL在传输层对网络连接进行加密，因此只要运行了TLS和SSL协议再用wireshark进行抓包的时候，就会抓到加密过的信息。（路由器是三层结构：物理层，数据链路层，网络层）。这类安全协议和RSA等加密协议不同，RSA只是单纯为双方建立加密通道，而TLS和SSL提供了证书的校验等过程，使连接更加安全。

TSL是标准化的SSL协议，SSL的工作流程如下：

<!-- 详细的TLS流程图 -->
<!-- <img src="{{site.url}}{{site.baseurl}}/img/SSL.png" alt="Drawing" style="width: 400px;"/>　 -->

- 客户端发送[Client Hello]消息
- 客户端收到[Sever Hello]消息
- 双方交换连接参数之后，客户端和服务端交换证书
- 服务端请求客户端公钥
- 客户端和服务端通过公钥保密协商共同的公钥私钥 ---例如 Diffie-Hellman

在上述流程中证书是由一个可信任的第三方数字签名的，一旦服务器端校验证书数字签名之后就会向第三方请求客户端的公钥进行加密，以此来验证客户端身份。（如果验证服务器端身份流程反过来）

下面是针对“心脏出血”漏洞加入的长度校验代码。
~~~
/* Read type and payload length first */
if (1 + 2 + 16 > s->s3->rrec.length)
        return 0; /* silently discard */
hbtype = *p++;
n2s(p, payload);
if (1 + 2 + payload + 16 > s->s3->rrec.length)
        return 0; /* silently discard per RFC 6520 sec. 4 */
pl = p;
~~~




