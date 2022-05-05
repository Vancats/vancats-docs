---
date created: 2022-05-03 22:05
date updated: 2022-05-05 22:04
---

#### HTTP 发展(扩展)

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
4. 推送：如传回 html 时附带传对应的 css，js

#### TCP 三次握手

TCP connection

1. SYN = 1 seq= X
2. SYN = 1 seq = Y ACK = X + 1
3. seq = Z ACK = Y + 1

#### URI

##### URI 统一资源标志符

##### URL 统一资源定位器

##### URN 永久统一资源定位符

#### 报文

##### 请求报文

Method URL Version

##### 响应报文

Version Code Meaning

#### 跨域

请求与当前协议，域名，端口任一不同的接口

#### 服务器设置

1. 只允许 GET POST HEADER 跨域
2. 只允许 text/plain multipart/form-data application/x-www-form-urlencoded 跨域

修改服务器设置

1. Access-Control-Allow-Origin
2. Access-Control-Allow-Header
3. Access-Control-Allow-Methods
4. Access-Control-Max-Age 这个时间内不用重新

#### Cache- Control

1. Public HTTP 请求返回的内容所经过的任何路径，包括代理服务器和浏览器，都可以进行缓存的操作
2. Private 只有发起请求的浏览器可以缓存
3. no-cache 不允许缓存

##### 到期时间

1. s-maxage 代理服务器使用，优先级最高
2. max-age
3. max-stale 发起端设置，在时间内可以使用过期缓存，浏览器不使用

##### 重新验证

1. must-revalidate 如果数据过期，必须去原服务端验证
2. proxy-revalidate 含义同上，不过shi