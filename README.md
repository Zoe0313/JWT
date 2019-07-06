## JWT认证

	Json web token (JWT), 根据官网的定义，是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准（(RFC 7519).该token被设计为紧凑且安全的，特别适用于分布式站点的单点登录（SSO）场景。JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其它业务逻辑所必须的声明信息，该token也可直接被用于认证，也可被加密。

## JWT特点

  - 体积小，因而传输速度快

  - 传输方式多样，可以通过URL/POST参数/HTTP头部等方式传输

  - 严格的结构化。它自身（在 payload 中）就包含了所有与用户相关的验证消息，如用户可访问路由、访问有效期等信息，服务器无需再去连接数据库验证信息的有效性，并且 payload 支持为你的应用而定制化。

  - 支持跨域验证，可以应用于单点登录。

## JWT原理
  - 由.分为三段，通过解码可以得到：
    1. 头部 header
        {
		  "alg": "HS256", //加密算法 通常使用HMAC或SHA256
		  "typ": "JWT"	  //类别 这里是JWT
		}

    2. 载荷 payload
    	载荷就是存放有效信息的地方。
    	一般添加用户的相关信息，但不建议添加敏感信息，该部分仍是可逆的base64编码。
    	一些可选的参数：
    	  iss  jwt签发者
    	  sub  jwt所面向的用户
    	  aud  接收该jwt的一方
    	  exp  什么时候过期
    	  nbf  在什么时间之后
    	  ...
    3. 签名 signature
        根据加密算法alg与私有秘钥key进行加密得到的签名字串。
        这一段是最重要的敏感信息，只在服务端解密。
        由三部分组成：
            header（base64后的）
            payload（base64后的）
            key
        目的：签名实际上是对头部以及载荷内容进行签名。如果有人对头部以及载荷的内容解码之后进行修改，再进行编码的话，那么新的头部和载荷的签名和之前的签名就将是不一样的。而且，如果不知道服务器加密的时候用的密钥的话，得出来的签名也一定会是不一样的。这样就能保证token不会被篡改。
        token 生成好之后，接下来就可以用token来和服务器进行通讯了。client端会将JWT存放在storage中，之后每次需要认证的请求都要把JWT发送过来。


## JWT使用场景

	JWT的主要优势在于使用无状态、可扩展的方式处理应用中的用户会话。服务端可以通过内嵌的声明信息，很容易地获取用户的会话信息，而不需要去访问用户或会话的数据库。在一个分布式的面向服务的框架中，这一点非常有用。
	但是，如果系统中需要使用黑名单实现长期有效的token刷新机制，这种无状态的优势就不明显了。

	- 优点
		快速开发
		不需要cookie
		JSON在移动端的广泛应用
		不依赖于社交登录
		相对简单的概念理解

	- 缺点
		Token有长度限制
		Token不能撤销
		需要token有失效时间限制(exp)

## python关于JWT的第三方库
    pip3 install pyjwt