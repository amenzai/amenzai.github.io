---
title: HTTP 协议笔记
comment: true
date: 2018-10-18 01:14:43
tags: 协议
categories: 前端学习笔记
---

HTTP 协议笔记。

<!-- more -->

**http基础**

![20180815112941](https://user-images.githubusercontent.com/20960902/44130210-f40a4ce2-a07e-11e8-95bc-45ca00f17d95.png)
![20180815113006](https://user-images.githubusercontent.com/20960902/44130216-fb7398c6-a07e-11e8-863d-27c8f343efd0.png)

**url**

![20180815113023](https://user-images.githubusercontent.com/20960902/44130221-03b1b734-a07f-11e8-8d7e-dc8c7dac8bc6.png)

**http 客户端**

- 浏览器
- curl 命令：curl -v www.baidu.com

**CORS跨域请求的限制与解决**

简单请求：

- 请求方法为：HEAD or GET or POST
- 头信息
  - Accept
  - Accept-Language
  - Content-Language
  - Last-Event-ID
  - Content-Type：只限于三个值：application/x-www-form-urlencoded or multipart/form-data or text/plain

凡是不同时满足上面两个条件，就属于非简单请求。一句话，简单请求就是简单的 HTTP 方法与简单的 HTTP 头信息的结合。

cors 的 http 响应：
```
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```

（1）Access-Control-Allow-Origin

该字段是必须的。它的值要么是请求时Origin字段的值，要么是一个*，表示接受任意域名的请求。

（2）Access-Control-Allow-Credentials

该字段可选。它的值是一个布尔值，表示是否允许发送 Cookie。默认情况下，Cookie 不包括在 CORS 请求之中。设为true，即表示服务器明确许可，浏览器可以把 Cookie 包含在请求中，一起发给服务器。这个值也只能设为true，如果服务器不要浏览器发送 Cookie，不发送该字段即可。

（3）Access-Control-Expose-Headers

该字段可选。CORS 请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个服务器返回的基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。上面的例子指定，getResponseHeader('FooBar')可以返回FooBar字段的值。

非简单请求：

非简单请求是那种对服务器提出特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。

非简单请求的 CORS 请求，会在正式通信之前，增加一次 HTTP 查询请求，称为“预检”请求（preflight）。浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些 HTTP 动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。这是为了防止这些新增的请求，对传统的没有 CORS 支持的服务器形成压力，给服务器一个提前拒绝的机会，这样可以防止服务器大量收到DELETE和PUT请求，这些传统的表单不可能跨域发出的请求。


**Cache-Control**

- public：http请求 经过的任何地方都可以缓存
- private：只有浏览器才可以缓存
- no-cache：会先服务器验证
- `max-age=<seconds>`：多长时间缓存过期
- `s-maxage=<seconds>`：用于代理服务器缓存
- `max-stale=<seconds>`：即便缓存过期，只要没有超过这里设置的如期，仍可去缓存取
- must-revalidate：重新验证
- proxy-revalidate
- no-store：所有地方缓存都不生效，每次都要服务器拿
- no-trnsform

**Last-Modified**

- 上次修改时间
- 配合 If-Modified-Since 或者 If-Unmodified-Since 使用
- 对比上次修改时间以验证资源是否需要更新

![20180815113037](https://user-images.githubusercontent.com/20960902/44130234-11b279f4-a07f-11e8-87cf-3254a39d029b.png)

**Etag**

- 数据签名
- 配合 If-Match 或者 If-Non-match 使用
- 对比资源的签名判断是否使用缓存

**Cookie**

- 通过 Set-Cookie 设置
- 下次请求会带上
- 键值对，可以多个
- max-age 和 expires 设置过期时间
- Secure 只在 https 的时候发送
- HttpOnly 无法通过 document.cookie 访问

**HTTP 长连接**

- Connection: keep-Alive / close
- 浏览器并发连接数有限制：chrome 6个

**数据协商**
- request
  - Accept
  - Accept-Encoding
  - Accept-Language
  - User-Agent
- response
  - Content-type
  - Content-Encoding
  - Content-Language

**跳转 Redirect**

[参考代码](https://github.com/amenzai/code-snippet/blob/master/note/httpProtocol/server.js)

**content-security-policy**

限制资源获取

**Nginx**

下载地址：http://nginx.org/en/download.html

**Nginx代理配置和代理缓存**

安装目录下的`conf>nginx.conf`文件：

```bash
# 自定义配置文件
# nginx.conf 中添加此内容
include          servers/*.conf;

# test.conf文件内容如下：
proxy_cache_path cache levels=1:2 keys_zone=my_cache:10m; # 代理缓存配置
# server {
#   listen       80;
#   server_name  test.com; # 代理 test.com 到 http://127.0.0.1:8888
#   location / {
#     proxy_cache       my_cache; # 设置代理缓存，如果设置了浏览器缓存，那么代理也会实施缓存，那么使用其他浏览器访问，就会从代理缓存取数据（或者其他用户进行访问）
#     proxy_pass        http://127.0.0.1:8888;
#     proxy_set_header  Host $host; # 代理头 设置host为test.com
#   }
# }

server {
  listen       80 default_server; # 自动跳转 https
  listen       [::]:80 default_server;
  server_name  test.com;
  return 302 https://$server_name$request_uri;
}

server {
  # listen       443; # 配置https
  listen       443 http2; # 配置http2
  server_name  test.com;
  http2_push_preload on; # http2配置 服务端推送

  ssl on;
  ssl_certificate_key ../certs/localhost-privkey.pem; # 那个命令生成的两个文件的地址
  ssl_certificate ../certs/localhost-cert.pem;

  location / {
    proxy_cache       my_cache;
    proxy_pass        http://127.0.0.1:8888;
    proxy_set_header  Host $host;
  }
}
```
代理服务器可以很容易修改http请求的请求和响应内容，因为 http 请求是明文传输，可以解析得到，https 是不可以的，因为 https 进行了加密

**HTTPS解析**

- http明文传输，可以拦截取到
- 私钥、公钥

![20180815113052](https://user-images.githubusercontent.com/20960902/44130243-1c1e7a50-a07f-11e8-95b6-8ad60995f25a.png)

**Nginx部署HTTPS**

本地生成 公钥私钥
```bash
# git bash 执行命令 生成两个文件
openssl req -x509 -newkey rsa:2048 -nodes -sha256 -keyout localhost-privkey.pem -out localhost-cert.pem
```

**http2**

浏览器并发 TCP连接数 限制
- 优势
  - 信道复用（一个 TCP 链接就行）
  - 分帧传输
  - server push(服务端主动推送内容)

ALPN nginx 兼容处理 http2:
```bash
# 测试
curl -v -k https://test.com
curl -v -k --http1.1 https://test.com # 使用http1.1
```