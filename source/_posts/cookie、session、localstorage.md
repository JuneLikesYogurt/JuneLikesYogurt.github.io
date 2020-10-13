---
title: cookie、session，tooken、localstorage区别与联系
date: 2020-02-28 11:16:55
tags:
    - 前端刷题
categories:
    - Notes
---
# cookie和session
由于http的无状态性，为了使某个域名下的所有网页能够共享某些数据，session和cookie出现了。 

简而言之:
1. session 有如用户信息档案表, 里面包含了用户的认证信息和登录状态等信息.
2. 而 cookie 就是用户通行证

## 客户端访问服务器的流程如下：
1. 客户端会发送一个http请求到服务器端
2. 服务器端接受客户端请求后，建立一个session，并发送一个http响应到客户端，这个响应头，其中就包含**Set-Cookie**头部。**该头部包含了sessionId。**
3. 在客户端发起的第二次请求，假如服务器给了set-Cookie，浏览器会自动在请求头中添加cookie
4. 服务器接收请求，分解cookie，验证信息，核对成功后返回response给客户端  
<br>
![](cookie、session、localstorage/cookie.png)

## 区别
1. cookie数据存放在客户的浏览器上，session数据放在服务器上
2. session会在一定时间内保存在服务器上，当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用cookie 
3. 建议将登录信息等重要信息存放为session，其他信息如果需要保留，可以放在cookie中 
4. session中保存的是**对象**，cookie中保存的是**字符串** 
5. session不能区分路径，同一个用户在访问一个网站期间，所有的session在任何一个地方都可以访问到，而cookie中如果设置了路径参数，那么同一个网站中不同路径下的cookie互相是访问不到的。 
6. **Cookie支持跨域名访问**，例如将domain属性设置为“.biaodianfu.com”，则以“.biaodianfu.com”为后缀的一切域名均能够访问该Cookie。跨域名Cookie如今被普遍用在网络中，例如Google、Baidu、Sina等,而**Session则不会支持跨域名访问**。Session仅在他所在的域名内有效。


# token
token 也称作令牌，由uid+time+sign[+固定参数]  
token 的认证方式类似于临时的证书签名, 并且是一种服务端无状态的认证方式, 非常适合于 REST API 的场景.（无状态就是服务端并不会保存身份认证相关的数据）

## 组成
+ uid: 用户唯一身份标识
+ time: 当前时间的时间戳
+ sign: 签名, 使用 hash/encrypt 压缩成定长的十六进制字符串，以防止第三方恶意拼接
+ 固定参数(可选): 将一些常用的固定参数加入到 token 中是为了避免重复查库

## 存放
在客户端一般存放于localStorage，cookie，或sessionStorage中。  
在服务器一般存于数据库中

## token认证流程
与cookie相似
1. 用户登录，成功后服务器返回Token给客户端。
2. 客户端收到数据后保存在客户端
3. 客户端再次访问服务器，将token放入**headers**中
4. 服务器端采用filter过滤器校验。校验成功则返回请求数据，校验失败则返回错误码



# sessionStorage、localstorage
localStorage 生命周期是永久，除非主动清除localStorage信息，否则这些信息将永远存在。存放数据大小为一般为5MB,而且它仅在客户端（即浏览器）中保存，不参与和服务器的通信。  
sessionStorage 仅在当前会话下有效，关闭页面或浏览器后被清除。存放数据大小为一般为5MB,而且它仅在客户端（即浏览器）中保存，不参与和服务器的通信。

## 复杂数据存储
上面都是对于简单的数据类型的存储，但当要存储的数据是一个对象或是数组的时候，直接存储是不行的。  
存储数据前：利用**JSON.stringify**将对象转换成字符串  
获取数据后：利用**JSON.parse**将字符串转换成对象




# 参考
[彻底弄懂session，cookie，token](https://segmentfault.com/a/1190000017831088?utm_source=tag-newest)  
[Cookie、session和localStorage、以及sessionStorage之间的区别](https://www.cnblogs.com/jing-tian/p/10991431.html)  
[HTML5 Web 存储（localStorage和sessionStorage）](https://blog.csdn.net/sleepwalker_1992/article/details/82832123?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)