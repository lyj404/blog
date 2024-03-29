---
title: JWT(JSON WEB TOKEN)
date: 2023-08-20 16:54:01
tags: JWT
categories: BackEnd
keywords: JWT
cover: https://s21.ax1x.com/2024/03/05/pFDcfoD.jpg
description: JWT 是 JSON Web Token 的缩写，是一种用于在网络应用环境间传递安全信息的简洁的、URL安全的表述性声明规范。
---
JWT 是 JSON Web Token 的缩写，是一种用于在网络应用环境间传递安全信息的简洁的、URL安全的表述性声明规范。JWT 可以被用来在不同的应用程序之间传递声明，例如用户身份信息、授权信息等。
# JWT的工作原理
* 服务端生成一个JWT令牌，并在其中包含需要传递的声明。
* 服务端将JWT令牌发送给客户端。
* 客户端在向服务端发送请求时，将JWT令牌包含在请求头中。
* 服务端验证JWT令牌的有效性和完整性，并根据声明做出相应的授权策略。
# JWT的优势
1. 简洁明了：JWT使用JSON格式，易于阅读和理解
2. URL安全：JWT可以安全的嵌入在URL中，而不会泄露敏感信息
3. 可扩展性：JWT可以扩展以支持新的声明和功能。
# JWT的应用场景
1. 用户身份验证：JWT可以用来在不同的应用程序之间传递用户身份信息，列入在单点登录(SSO)场景中。
2. 授权：JWT可以用来在不同的应用程序之间传递授权信息，例如允许用户访问特定的资源。
3. 信息交换：JWT可以用来在不同的应用程序之间安全的交换信息。
# JWT组成
**JWT有三部分组成：**
1. **头部（header）**：头部通常包含两个字段：
	* **类型（typ）：** 表示令牌的类型，通常为“JWT”
	* **签名算法：** 表示签名算法，例如"HS256"、"RS256" 等
2. **载荷（Payload）:** 载荷包含需要传递的声明，例如用户身份信息、授权信息等。声明可以是任何 JSON 对象，例如：
```json
{
  "iss": "issuer",
  "sub": "subject",
  "aud": "audience",
  "exp": 1577836800,
  "iat": 1577836000
}
```
3. **签名（Signature）:** 签名是用于验证令牌真实性和完整性的哈希值。它是使用头部和载荷以及一个密钥生成的。
[![jwt.png](https://s21.ax1x.com/2024/03/05/pFDcgL6.png)](https://imgse.com/i/pFDcgL6)
**JWT的作用主要是防止信息被篡改，而不是为了加密信息被人无法破解。**
* **防止篡改：** 由于签名是包含在令牌中的，并且是使用秘钥生成的，因此任何对令牌内容的修改都会导致签名验证失败。
* **防止伪造：** 由于签名是使用服务器端的秘钥生成的，因此只有服务器端才能生成有效的令牌。
 
 JWT 的头部和载荷是未加密的，因此任何人都可以查看其中的内容。即使签名是加密的，但签名算法的密钥也是可以被破解的。

> JWT常用于在网络应用环境间传递安全信息。JWT 可以防止信息被篡改，但并不是为了加密信息让人无法破解。