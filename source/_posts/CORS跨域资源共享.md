---
title: CORS跨域资源共享
date: 2020-03-14 11:47:03
tags:
    - Network
    - 面试
categories:
    - Notes
---
CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。  
它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。
包括简单请求和非简单请求，其中非简单请求需要增加一次“预检”请求。
<!-- more -->

# 两种请求区别
只要同时满足以下两大条件，就属于简单请求。
### 1. 请求方法是以下三种方法之一：
   1. HEAD
   2. GET
   3. POST
### 2. HTTP的头信息不超出以下几种字段：
   1. Accept
   2. Accept-Language
   3. Content-Language
   4. Last-Event-ID
   5. Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain

# 简单请求
## 基本流程
如果是简单请求，浏览器将自动在头信息中添加一个**Origin**字段。该字段用来说明本次请求来自哪个源（协议 + 域名 + 端口）。服务器根据这个值，决定是否同意这次请求。
```
GET /cors HTTP/1.1
Origin: http://api.bob.com
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

## withCredentials属性
CORS请求默认不发送Cookie和HTTP认证信息。如果要把Cookie发到服务器，一方面要服务器同意，指定Access-Control-Allow-Credentials字段。  
另一方面，开发者必须在AJAX请求中打开withCredentials属性。
```
Access-Control-Allow-Credentials: true
```
```
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```


# 非简单请求
## 预检请求
非简单请求是那种对服务器有特殊要求的请求，比如请求方法是**PUT**或**DELET**E，或者**Content-Type**字段的类型是**application/json**。  
非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。  
例：
```
OPTIONS /cors HTTP/1.1
Origin: http://api.bob.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```
其中：
+ "预检"请求用的请求方法是OPTIONS，表示这个请求用来询问
+ 关键字段是**Origin**，表示来自哪个源
+ Access-Control-Request-Method，用来列出用到的HTTP方法
+ Access-Control-Request-Headers，指定额外发送的头信息字段

## 预检请求的回应
```
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Content-Type: text/html; charset=utf-8
Content-Encoding: gzip
Content-Length: 0
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain
```
**Access-Control-Allow-Origin**字段，表示**http://api.bob.com**可以请求数据。该字段也可以设为星号，表示同意任意跨源请求。
```Access-Control-Allow-Origin: *```
如果浏览器否定了"预检"请求，会返回一个正常的HTTP回应，但是不会有任何CORS相关的头信息字段。因此会触发一个错误，被XMLHttpRequest对象的onerror回调函数捕获。控制台会打印出如下的报错信息。
```
XMLHttpRequest cannot load http://api.alice.com.
Origin http://api.bob.com is not allowed by Access-Control-Allow-Origin.
```

## 正常请求的回应
一旦服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。

# 与JSONP比较
JSONP只支持GET请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。

# 参考
[阮一峰-跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)