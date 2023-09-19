想要使用加密通信的HTTPS协议不一定需要CA颁发的SSL证书
任何人都可以给自己签发SSL证书
自签名证书一样能够调用SSL/TLS协议，一样能够保证个人隐私信息不被泄露。存在的问题就是自签名证书无法保证网站的真实性。
## 自签名SSL证书
自签名证书可以指许多不同的证书类型，包括**SSL/TLS证书、S/MIME证书、代码签名证书**等，其中最常见的证书类型是自签名SSL证书。与CA颁发的SSL证书不同，自签名证书通常指的是那些未经第三方验证，直接上传到私有公钥基础结构 (PKI) 的证书文件。  
![](https://segmentfault.com/img/bVcVuZd)
一般来说，自签名证书可以用于内部测试环境或限制外部人员访问的Web服务器。
### 创建自签名证书
虽然自签名证书存在一定的安全隐患，但有它的优势，这里给大家分享一下创建自签名证书的方法。其实，创建自签名SSL证书很简单，主要取决于您的服务器环境，如Apache或Linux服务器。方法如下：  
**1、生成私钥**  
要创建SSL证书，需要私钥和证书签名请求（CSR）。您可以使用一些生成工具或向CA申请生成私钥，私钥是使用RSA和ECC等算法生成的加密密钥。生成RSA私钥的代码示例：openssl genrsa -aes256 -out servername.pass.key 4096，随后该命令会提示您输入密码。  
**2、生成CSR**  
私钥生成后，您的私钥文件现在将作为 servername.key 保存在您的当前目录中，并将用于生成CSR。自签名证书的CSR的代码示例：openssl req -nodes -new -key servername.key -out servername.csr。然后需要输入几条信息，包括组织、组织单位、国家、地区、城市和通用名称。通用名称即域名或IP地址。  
输入此信息后，servername.csr 文件将位于当前目录中，其中包含 servername.key 私钥文件。  
**3、颁发证书**  
最后，使用server.key（私钥文件）和server.csr 文件生成新证书（.crt）。以下是生成新证书的命令示例：openssl x509 -req -sha256 -days 365 -in servername.csr -signkey servername.key -out servername.crt。最后，在您的当前目录中找到servername.crt文件即可。

# OPENSSL
OpenSSL是一个开放源代码的软件库包，应用程序可以使用这个包来进行安全通信，避免窃听，同时确认另一端连接者的身份。

OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。

SSL是Secure Sockets Layer（安全套接层协议）的缩写，可以在Internet上提供秘密性传输。Netscape公司在推出第一个Web浏览器的同时，提出了SSL协议标准。其目标是保证两个应用间通信的保密性和可靠性,可在服务器端和用户端同时实现支持。已经成为Internet上保密通讯的工业标准。

# 单向认证和双向认证
![[Pasted image 20221202104804.png]]
客户端使用服务端返回的信息验证服务器的合法性，包括：
-   证书是否过期
-   发型服务器证书的CA是否可靠
-   返回的公钥是否能正确解开返回证书中的数字签名
-   服务器证书上的域名是否和服务器的实际域名相匹配

双向认证增加了服务端对客户端的认证
![[Pasted image 20221202104815.png]]

