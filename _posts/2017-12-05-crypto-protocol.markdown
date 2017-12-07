---
layout: post
title:  "密码学协议 --- Cryptography Protocols"
date:   2017-12-05 12:00:00 +0000
categories: jekyll update
---
{% include 123.html %}

# **流加密(Stream Ciphers)**

流加密是密码学的第一课，因为流加密使用了对称加密算法,所以相应的加密解密方程比较好理解。

**One-Time-Pad (OTP)**

OTP是流加密中最简单也是最经典的一种，假设我们现在有一个长度为l的二进制message需要加密，我，们首先要生成一个长度为l的二进制key，然后让message和key进行二进制加法就完成了加密过程，因为二进制加法的特殊性，对于解密过程只要使用相同的key加上密文就可以还原message。下图给了一个简单的OTP例子：

<img src="{{site.url}}{{site.baseurl}}/img/OTP.png" alt="Drawing" style="width: 400px;"/>

我们可以证明OPT是满足完美保密的，在这里我就不证明了这个证明比较简单。接下来来说OTP的一些缺点：

* 第一个缺点就是key太长了！如果我们的message很长的话使用OTP的key就会特别长。
* 第二个就是key不是真正的随机

可以攻击OTP的方式举例：

1. 利用二进制加法 m+k+k = m 的性质

    假如我们现在有两个人Alice和Bob想互相传递信息m，Alice先生成了一个key，因此加密后的信息为(m+k)，Bob通过((m+k)+k)就可以完整还原m。现在考虑一个入侵者嗅探Alice和Bob的包，因此入侵者是可以获取到密文(m+k)的，我们假设入侵者也可以访问加密数据库，也就是说入侵者可以随意生成一个假冒的message让加密方程用key来生成相应的密文。没错！在这时入侵者可以吧(m+k)当作明文输入到加密数据库里面从而使加密数据库输出结果为((m+k)+k)，也就是真正的明文，入侵完成。

2. 使用man-in-the-middle attack，很容易改变头信息

    这样Eve可以仿造自己发送出去的包。

    <img src="{{site.url}}{{site.baseurl}}/img/OTP_attack.png" alt="Drawing" style="width: 600px;"/>

# **RC4**
