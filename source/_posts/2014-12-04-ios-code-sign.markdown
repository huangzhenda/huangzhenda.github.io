---
layout: post
title: "iOS代码签名探究"
date: 2014-12-04 18:10:18 +0800
comments: true
categories: iOS
keyword: 代码签名，iOS,Code Sign

---

参考：

* [objccn.io: 代码签名探析](http://objccn.io/issue-17-2/)
* [王中周的技术博客： iOS Code Signing 学习笔记](http://foggry.com/blog/2014/10/16/ios-code-signing-xue-xi-bi-ji/)



##数字签名
   数字签名（又称公钥数字签名、电子签章），使用了公钥加密领域的技术实现，一套数字签名通常定义两种互补的运算，一个用于签名，另外一个验证。
   
   签名过程：
   
   发送报文时，发送方用一个```哈希函数```从报文文本中生成```报文摘要```,然后用自己的```私人密钥```对这个摘要进行加密，这个加密后的摘要将作为报文的数字签名和报文一起发送给接收方，接收方首先用与发送方一样的```哈希函数```从接收到的原始报文中计算出```报文摘要```，接着再用发送方的```公用密钥```来对报文附加的数字签名进行解密，如果这两个摘要相同、那么接收方就能确认该数字签名是发送方的。

数字签名功效：
 
 1. 确定消息确实由发送方签名并发出的，别人假冒不了。
 2. 数字签名能确定消息的完整性。因为报文文本如果发生改变，报文摘要也会发生改变，这样与解密后的报文摘要就会不一致。
 
 以上内容摘自<http://baike.baidu.com/view/7626.htm>。抽取了我认为比较有用的信息。
 
##证书
证书的作用是什么？怎么得到证书？证书是怎么使用的？

* 首先先介绍怎么得到证书。

![alt:证书获取流程](/images/ios_code_sign/certifiate_create_process.png)

流程大概描述为，开发者通过开发者中心，申请证书，并上传通过keychain生成的CSR文件，提交给苹果的 ```Apple Worldwide Developer Relations Certification Authority```(简称WWDR)证书认证中心进行签名，最后从开发者中心上面下载并且安装证书，这个过程还会产生一个私钥。 

安装后，你会在 ```钥匙串访问(keychain)```中点击 ```我的证书(My Certificates)```查看到。

![alt:证书](iphone-developer-keychain.png)

另外，还可通过命令行，快速显示你系统中，能用来签名的认证方法，那就是利用用户广泛的命令行工具 ```security```:

	$ security find-identity -v -p codesigning                   
	1) 01C8E9712E9632E6D84EC533827B4478938A3B15 "iPhone Developer: Thomas Kollbach (7TPNXN7G6K)"
	
证书大概的讲，一个证书就是一个公钥加上许多附加信息，这些附加的信息被某些认证机构（Certificate Authority简称CA ，iOS中的CA当然是WWDR）进行签名认证的。

![alt:CA签名认证过程](/images/ios_code_sign/CA_sign.png)

经过签名认证后的证书长这样：

![alt:certificate](/images/ios_code_sign/certificate.png)


* 怎么使用证书

1. iOS系统中，持有 ```WWDR```的公钥，系统先用证书的内通过 哈希算法 得到一个 信息摘要； 
2. 然后在使用 ```WWDR```的公钥对证书中包含的数字签名进行解密，得到经过 ```WWDR``` 私钥加密过后的信息摘要。
3. 最后对比两个信息摘要，如果内容相同，说明该证书可信。

![alt: 证书使用过程](/images/ios_code_sign/cer_sy_ver_flow.png)

* 证书的作用

在整个过程中，其实iOS最想得到的就是证书上的开发者公钥。通过这个公钥就可以验证，应用中的代码跟资源签名是否正确，是否完整。 

那怎么才能确认开发者公钥就是可信任的呢？ 这时候，就得通过 ```WWDR```(也就是苹果自己的认证中心)利用CA 私，对搭载 开发者公钥的证书进行签名，然后在通过内置在iOS系统的 ```WWDR```公钥进行解密。通过自家签名后，才能保证证书是可以被信任的，开发者的公钥是可信任的。

这个就是证书存在的作用。



##授权机制


##配置文件

