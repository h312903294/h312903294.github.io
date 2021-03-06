---
layout: post
title: HTTPS和HTTP2.0
date: 2019-07-19
author: Nicholas Huang
header-img:
categories: HTTPS
tags:
    - HTTPS
---
# HTTPS和HTTP2.0
HTTPS可以解决窃听、篡改、冒充等风险
HTTPS的协议栈列表：

HTTPS|
-----|
HTTP1.1|
SSL or TLS|
TCP|
IP|

加密算法：

名称|特点
---|---
对称加密|加解密用同一个密钥
非对称加密|公钥和私钥，公钥和算法是公开的，性能较低，安全性超强
数字签名|将任意长度内容转换成固定长度值，和内容一起发送

HTTPS流程图：

```sequence
客户端->服务器: ① Client Hello
服务器->客户端: ② Server Hello 这是我的SSL证书
客户端->服务器: ③ [用对称加密，对称加密算法和对称密钥]\n公钥加密
Note left of 客户端: 公钥
Note right of 服务器: 私钥
服务器->客户端: ④ [好的]\n对称密钥加密
客户端->服务器: ⑤ [请求内容]\n对称密钥加密
Note left of 客户端: 对称密钥
Note right of 服务器: 对称密钥
服务器->客户端: ⑥ [响应内容]\n对称密钥加密
```

证书中的内容：
    
    1.证书的发布机构CA
    2.证书的有效期
    3.公钥
    4.证书所有者
    5.签名
    
客户端校验证书步骤：
    
    1.读取证书中的证书所有者、有效期等信息进行校验
    2.查找操作系统中已内置的受信任的证书发布机构CA，与服务器发来的证书中的颁发者CA比对，用于校验证书是否为合法机构颁发
    3.如果找不到，报错，说明服务器发的证书是不可信任的
    4.如果找到，那么客户端从操作系统中取出颁发者CA的公钥，然后对服务器发来的证书里面的签名进行解密
    5.客户端使用相同的hash算法计算出服务器发来的证书的hash值，将这个计算的hash值与证书中的签名做对比
    6.对比结果一致，则证明服务器发来的证书合法，没有被冒充
    7.此时浏览器就可以读取证书中的公钥，用于后续加密了    



