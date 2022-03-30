

OpenID Connect Core 1.0 规范定义了核心的OpenID Connect功能: 身份认证构建在OAuth 2.0之上并且使用Claims来传递最终用户(End-User)相关信息. 它同时描述了在使用OpenID Connect的安全与隐私考虑.

要點

1  構建在OAuth2之上

2  使用Claims来传递


在背后, OAuth 2.0授权框架 [RFC6749] 与 OAuth 2.0 Bearer Token Usage [RFC6750] 规范为第三方应用程序获取与使用受保护的HTTP资源提供了一个通用的框架. 他定义了获取与使用Access Token去访问资源的机制但却未定义提供身份信息的方式方法. 值得注意的是, 在OAuth 2.0协议中, 没有提供关于认证最终用户(End-User)的相关信息. 在本协议中将看到读者所期望的内容.


OpenID Connect在OAuth 2.0授权流程的基础上,扩展实现了认证功能. 在客户端(Clients)发起授权请求时扩展了请求的范围(scope)值包含openid. 认证执行返回的信息是一个JSON Web Token (JWT) [JWT] 名叫 ID Token (详见 第2节). OAuth 2.0 认证服务端实现了 OpenID Connect 功能也被称作 OpenID 提供商 (OPs). OAuth 2.0 客户端(Clients) 使用 OpenID Connect 功能也被称作依赖方 (RPs).


该规范假定依赖方(Relying Party)已经从OpenID提供商(Provider)获取了相关 配置信息, 包括它的授权端点(Authorization Endpoint)与令牌端点(Token Endpoint)地址. 这些信息通常是通过正常方式或其他机制获取的, 详见 OpenID Connect Discovery 1.0 [OpenID.Discovery]部分的描述.


同样地, 该规范也假定依赖方(Relying Party)已经从OpenID提供商(Provider)获取了足够的凭证(sufficient credentials) 且提供了必要的信息. 这通常是通过动态注册(Dynamic Registration)或其他机制获取的, 详见 OpenID Connect Dynamic Client Registration 1.0 [OpenID.Registration]部分的描述.


OpenID Connect 协议可以抽象为以下几个步骤


RP (客户端) 发送请求给 OpenID 提供商 (OP).

提供商(OP) 认证最终用户(End-User) 并获取授权.

提供商(OP) 响应一个 ID Token 与一个 Access Token.

RP 使用 Access Token 发送一个请求给 UserInfo Endpoint.

UserInfo Endpoint 返回有关最终用户(End-User)的 Claims.

这些步骤如下图所示:
```
+--------+                                   +--------+
|        |                                   |        |
|        |---------(1) AuthN Request-------->|        |
|        |                                   |        |
|        |  +--------+                       |        |
|        |  |        |                       |        |
|        |  |  End-  |<--(2) AuthN & AuthZ-->|        |
|        |  |  User  |                       |        |
|   RP   |  |        |                       |   OP   |
|        |  +--------+                       |        |
|        |                                   |        |
|        |<--------(3) AuthN Response--------|        |
|        |                                   |        |
|        |---------(4) UserInfo Request----->|        |
|        |                                   |        |
|        |<--------(5) UserInfo Response-----|        |
|        |                                   |        |
+--------+                                   +--------+
```