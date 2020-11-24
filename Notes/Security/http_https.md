# http and https

## 网络协议

主要有两种分层方式；OSI（Open System Interconnect） 7 层协议；TCP/IP 4 层协议

两者之间的对应关系：

| OSI    | TCP/IP     | 功能                               | 协议                        |
| :----- | :--------- | :--------------------------------- | :-------------------------- |
| 应用层 | 应用层     | 应用程序，用户接口                 | HTTP，FTP，TFTP，NFS        |
| 表示层 | 应用层     | 数据表示，压缩和加密               | TELNET，SNMP                |
| 会话层 | 应用层     | 建立终止会话                       | SMTP，DNS                   |
| 传输层 | 传输层     | 提供端到端可靠豹纹传递和错误恢复   | TCP，UDP                    |
| 网络层 | 网际互联层 | 提供数据包从源到宿的传递和网际交互 | IP，ICMP，ARP，RARP，UUCP   |
| 链路层 | 网络接口层 | 讲比特组装层帧和点到点传递         | FDDI，SLIP，PPP，PDN        |
| 物理层 | 网络接口层 | 二进制形式传输数据                 | ISO2110，IEEE802，IEEE802.2 |

## http 请求头和响应头

* Host：服务器的域名
* Connection： keep-alive 不关闭TCP连接，close 表示关闭，默认为keep-alive
* Accept-Encoding：接收的压缩方式
* User-Agent： 用户代理，是服务器用来识别客户端的操作系统
* Content-Type： 数据格式，MIME TYPE
* Content-Encoding： 服务器数据压缩方式
* Transfer-Encoding：chunked表示采用分块传输编码，有该字段则无需使用Content-Length字段
* Content-Length：声明数据的长度，请求和回应头部都可以使用该字段

## TCP 请求

建立连接的三次握手和断开连接的四次握手，这里就不详述了。

主要介绍一下https流程。https 主要是在Http下加入SSL层（现在主流的是SLL/TLS），SSL是Https协议的安全基础。Https默认端口号是443.

1. 客户端给出协议版本号、一个客户端随机数A（Client random）以及客户端支持的加密方式
2. 服务端确认双方使用的加密方式，并给出数字证书、一个服务器生成的随机数B（Server random）
3. 客户端确认数字证书有效，生成一个新的随机数C（Pre-master-secret），使用证书中的公钥对C加密，发送给服务端
4. 服务端使用自己的私钥解密出C
5. 客户端和服务器根据约定的加密方法，使用三个随机数ABC，生成对话秘钥，之后的通信都用这个对话秘钥进行加密。

证书类型：

* 域名认证（DV=Domain Validation）：最低级别的认证，可以确认申请人拥有这个域名
* 公司认证（OV=Organization Validation）：确认域名所有人是哪家公司，证书里面包含公司的信息
* 扩展认证（EV=Extended Validation）：最高级别认证，浏览器地址栏会显示公司名称。
