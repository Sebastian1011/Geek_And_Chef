# JWT 

JWT全称：Json Web Token，是一个非常轻量的规范，用于在client 和 server之间安全的传递信息

JWT 由三部分组成，三者之间使用`.`号进行连接：

* Header
* Payload
* Signature

他们之间的关系如图：

![JWT](./../../assets/graphes/jwt.png)


## Header

描述JWT的基本信息，如类型和加密算法, 用base64编码

## Payload

client和server传递信息的json, 使用base64编码，JWT标准规定了5个必填字段：

* iss: 该JWT的签发者
* sub: 该JWT所面向的用户
* aud: 接收该JWT的一方
* exp(expires): 过期时间
* iat(issued at): 签发时间

## Signature

将Header 与 Payload使用 `.` 号连接，并使用HS256算法和提供的密钥进行加密，就得到了Signature。

这里Signature是对 Header 和 Payload 进行签名的，由于base64只是一种编码方式，是可逆的，所以在payload中不建议放敏感信息，根据 Signature 可以判断 Payload 和 Header 的信息是否被篡改，从而保证了信息的安全。

