---
date created: 2022-05-03 22:05
date updated: 2022-05-10 23:00
---

### HTTP 发展(扩展)

##### HTTP / 0.9

1. 只有 get 请求
2. 没有 HEADER 等描述信息
3. 服务器发送完毕，就直接关闭 TCP

##### HTTP / 1.0

1. 添加了很多的命令
2. 添加 HEADER 描述信息和 status code
3. 多字符集支持，多部分发送，缓存和权限

##### HTTP / 1.1

1. 持久连接
2. pipeline
3. 增加 host 和一些命令

##### HTTP 2

1. 所有数据以二进制传输
2. 同一连接的不同请求不必按顺序
3. 头信息压缩：1.1 中头信息都使用字符串传输，开销很大（扩展）
4. 服务端推送：如传回 html 时附带传对应的 css，js

### TCP 三次握手(扩展)

TCP connection

1. SYN = 1 seq= X
2. SYN = 1 seq = Y ACK = X + 1
3. seq = Z ACK = Y + 1

### URI

##### URI 统一资源标志符

##### URL 统一资源定位器

##### URN 永久统一资源定位符

#### 报文(扩展)

##### 请求报文

Method URL Version

##### 响应报文

Version Code Meaning

### 跨域(扩展)

请求与当前协议，域名，端口任一不同的接口

##### 服务器设置

1. 只允许 GET POST HEADER 跨域
2. 只允许 text/plain multipart/form-data application/x-www-form-urlencoded 跨域

修改服务器设置

1. Access-Control-Allow-Origin
2. Access-Control-Allow-Header
3. Access-Control-Allow-Methods
4. Access-Control-Max-Age 这个时间内不用重新

```js
res.writeHead(200, {
  'Access-Control-Allow-Origin': 'http://localhost:8888',
  'Access-Control-Allow-Headers': 'X-Test-Cors',
  'Access-Control-Allow-Methods': 'GET, PUT, POST, DELETE',
  // 请求建立时间内不用再次判断
  'Access-Control-Max-Age': 4
})
```

### Cache- Control

1. public 请求返回的内容所经过的任何路径，包括代理服务器和浏览器，都可以进行缓存的操作
2. private 只有发起请求的浏览器可以缓存
3. no-cache 允许缓存，但是每次发起请求都需要服务器端验证过才可以使用
4. no-store 不允许缓存
5. no-transform 不允许代理服务器改动返回的内容

##### 到期时间

1. s-maxage 代理服务器使用，优先级最高
2. `max-age`
3. max-stale 发起端设置，在时间内可以使用过期缓存，浏览器不使用

##### 缓存验证

1. `Last-Modified` --- `If-Modified-since` 对比资源修改时间验证 1. 编译但未改变内容 2. 修改文件速度过快
2. `Etag` 数据签名 --- `If-(none-)Match`

##### 重新验证(使用不多)

1. must-revalidate 如果数据过期，必须去原服务端验证
2. proxy-revalidate 含义同上，不过是缓存服务器过期时

在打包完成的 js 文件上加上相应的 hash，那么更改后请求的就不是原文件了，可以避免长缓存引起的问题

```js
const etag = req.headers['if-none-match']
if (etag === '777') {
  res.writeHead(304, {
    'Content-Type': 'text/javascript',
    'Cache-Control': 'max-age=200, no-cache',
    'Last-Modified': '123',
    'Etag': '777'
  })
  res.end('123')
} else {
  res.writeHead(200, {
    'Content-Type': 'text/javascript',
    'Cache-Control': 'max-age=200, no-cache',
    'Last-Modified': '123',
    'Etag': '777'
  })
  res.end('console.log("script loaded twice")')
}
```

### Cookie(扩展)

1. 服务端设置 Set-Cookie 请求头
2. 当本地有 cookie 键值对时，发送请求会自动带上
3. 可以设置 max-age 和 httpOnly(禁用 document.cookie
4. domain 跨子域
5. 不能超过 4k

##### session

1. session 是服务端的内容，可以储存任何数据
2. 第一次请求后，会发生 sessionId 并存储到 cookie 中，根据 sessionId 来判断用户状态
3. 也可以使用 token 机制

```js
const host = req.headers.host
if (host === 'test.com') {
  res.writeHead(200, {
    'Content-Type': 'text/html',
    // test.com 域名要一致，无法跨域名设置 cookie
    // 设置后，a.test.com b.test.com 可以使用 cookie
    'Set-Cookie': ['id=1223;max-age=2000;domain=test.com', 'name=vancats; httpOnly']
  })
}
```

### keep-alive

1. Connection 请求头
2. 默认打开，最多可同时建立 6 个 tcp 连接，可看 connection Id

### 数据协商

1. Accept：限制数据类型
2. Accept-Encoding：数据编码压缩方式
3. Accept-Language：希望的语言
4. User-Agent
5. Content-Type
6. Content-Encoding
7. Content-Language
8. X-Content-Type-Options：nosniff 不允许浏览器预测可能发送的类型

### Redirect

1. Location
2. 302
3. 301

### Content-Security-Policy

1. 限制资源获取
2. 报告资源获取越权
3. default-src 限制全局
4. connet-src 发送位置
5. img-src 图片来源
6. script-src js 来源
