---
layout: post
title:  "流加密"
date:   2018-5-07 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

- [One Time Pad](#one-time-pad)
- [RC4](#rc4)
- [WEP](#wep)
- [LFSR](#lfsr)
- [CSS uses LFSR](#css-uses-lfsr)

流加密，是对称加密算法的一种，加密和解密双方使用相同伪随机加密数据流（pseudo-randomstream）作为密钥，明文数据每次与密钥数据流顺次对应加密，得到密文数据流。实践中数据通常是一个位（bit）并用异或（xor）操作加密。

该算法解决了对称加密完善保密性（perfectsecrecy）的实际操作困难。完善保密性由香农于1949年提出。由于完善保密性要求密钥长度不短于明文长度，故而实际操作存在困难，改由较短数据流通过特定算法得到密钥流。

加密过程：首先生成一个对称秘钥，使用用户公钥加密这个对称秘钥后存储在文件头，然后用生成的对称秘钥加密文件数据存储。

解密过程：首先使用自己的私钥解密被加密的对称秘钥，再用该对称秘钥解密出数据原文。

# One-Time-Pad

OTP是流加密中最简单也是最经典的一种，假设我们现在有一个长度为l的二进制message需要加密，我们首先要生成一个长度为l的二进制key，然后让message和key进行二进制加法就完成了加密过程，因为二进制加法的特殊性，对于解密过程只要使用相同的key加上密文就可以还原message。下图给了一个简单的OTP例子：

<img src="{{site.url}}{{site.baseurl}}/img/OTP.png" alt="Drawing" style="width: 400px;"/>

我们可以证明OPT是满足完美保密的。什么是完美保密？在密码中我们假设有长度相同的两个信息m1和m2，对于相同的加密密钥k，所有密文空间中的任意一个c，满足以下条件：

<img src="http://chart.googleapis.com/chart?cht=tx&chl= |Prob(E(k,m_1)=c)-Prob(E(k,m_2)=c)|<negl" style="border:none;">

等同于commitment scheme中的hiding性质，更加通俗的理解方法为，我们现在向加密协议输入两个message m1和m2,加密协议自己选择使用任意一个message进行加密合成密文c，完美保密要求入侵者不能有效的分辨出来c到底是m1生成的还是m2生成的(入侵者不知道k)，也就是说密文不能泄漏任何原文信息。那么为什么OPT满足完美加密呢？我们可以想象对于已经满足binding性质的加密协议来说(不存在collision)，那么对于一个已经固定的c来说，成功猜出来m1的概率和成功猜出来m2的概率相同(1/{2^(m)},m is the length of message and k)。

但是，OTP具有很多缺点：

* 第一个缺点就是key太长了！如果我们的message很长的话使用OTP的key就会特别长。
* 第二个就是key不是真正的随机，而是在通信前存在交流双方电脑中的固定key，并不能保证每次的通信都使用不一样的key

可以攻击OTP的方式举例：

1. two-time-pad-attack

    假如我们现在有两个人Alice和Bob想互相传递信息m，Alice先生成了一个key，因此加密后的信息为(m+k)，Bob通过((m+k)+k)就可以完整还原m。现在考虑一个入侵者嗅探Alice和Bob的包，因此入侵者是可以获取到密文(m+k)的。
    - 我们首先假设入侵者也可以访问加密数据库，也就是说入侵者可以随意生成一个假冒的message让加密方程用key来生成相应的密文。没错！在这时入侵者可以吧(m+k)当作明文输入到加密数据库里面从而使加密数据库输出结果为((m+k)+k)，也就是真正的明文，入侵完成。
    - 那么如果不能访问加密数据库呢？也很容易破解，我们假设Alice和Bob通信了两次c1和c2,分别对应m1和m2,那么入侵者可以通过c1+c2来得到m1+m2的和，英语并不能保证破解m1+m2的过程是很难计算的，也就是说很可能被计算出来。

2. man-in-the-middle attack

    这样Eve可以仿造自己发送出去的包。

    <img src="{{site.url}}{{site.baseurl}}/img/OTP_attack.png" alt="Drawing" style="width: 600px;"/>

# RC4

RC4协议是在1987年被Ron Rivest发明的，RC4的全称为(Ron Cipher 4)人家有命名资格回合，而且Ron大佬还是著名顶顶RSA项目组中的头号人物，RSA是个常用的加密算法会在后面提到。 RC4包括两部分，初始化和密钥生成。目前在HTTPS和WEP中使用。

根据定义，RC4算法有两个阶段，1)初始化; 2)生成密钥流

<img src="{{site.url}}{{site.baseurl}}/img/rc4.png" alt="Drawing" style="width: 400px;"/>

在初始化阶段会生成一个长度为2048比特（256字节）的数组(值为0-255)，并且将数组顺序打乱。在下面的代码中先生成一个S数组(顺序0-255)，之后按照算法(伪随机)交换S数组中的元素。key是固定的密钥(RC4的随机key生成是建立在固定密钥至上的)

```
// 初始化算法，伪代码
for i from 0 to 255
    S[i] := i
endfor
j := 0
for( i=0 ; i<256 ; i++)
    j := (j + S[i] + key[i mod keylength]) % 256//算出来一个j的值
    swap values of S[i] and S[j]
endfor
```

接下来是生成密钥流的算法，RC4算法保证了每256个循环中每个元素都会被遍历一次。

```
// 产生密钥流 算法，伪代码
i := 0
j := 0
while GeneratingOutput:
    i := (i + 1) mod 256   //
    j := (j + S[i]) mod 256 //
    swap values of S[i] and S[j]  //交换某两个元素
    k := inputByte ^ S[(S[i] + S[j]) % 256]//input和某个元素二进制相加
    output K
endwhile
```

RC4有以下几个缺点：

1. 我们可以观察到在初始化算法中第一个字节的数据永远不会被交换，因此在实际计算过程中应该把第一个byte放到第256个位置上，以此来加入随机循环。
2. 依然容易受到related key attacks(会在WEP中具体解释)，因为随机密钥生成还是基于固定的长期密钥。

# WEP

WEP全名叫做有线等效加密，常用于无线通信加密。

<img src="{{site.url}}{{site.baseurl}}/img/wep.png" alt="Drawing" style="width: 400px;"/>

过程描述：IV为24bit的初始化向量，双方利用这个变量来借助RC4生成密钥流。在发送密文时会将IV贴合在密文前部(只获得IV并不能完全得到密钥，因为seed由IV和WEP key(固定key)组成)。

缺点：

1. two-time pad attack：IV只有24字节长，所以key会在2^24之后重用---应该使用更加长的IV
2. FMS attack(related key attack)

因为密钥生成固定使用IV和长期key，所以所有的密钥只有前24个字节不同。并且对于密文来说第一个byte的message永远是相同的(因为WEP的应用导致发送信息总有一个固定的header)，因此可以减少一个byte的计算量。

# LFSR

- 密钥空间：K = {0,1}^s
- 寄存器R的长度：s
- 初始化状态：R=k

比较容易想象的结构。

<img src="{{site.url}}{{site.baseurl}}/img/lfsr.png" alt="Drawing" style="width: 400px;"/>



# CSS uses LFSR

- 密钥空间：K = {0,1}^40
- 寄存器R的长度：17bit的LFSR和25bit的LFSR
- 初始化状态：R17=1||K[0-15]
- 初始化状态:R25=1||K[16-39]

<img src="{{site.url}}{{site.baseurl}}/img/css.png" alt="Drawing" style="width: 400px;"/>