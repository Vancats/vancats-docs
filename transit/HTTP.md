---
date created: 2022-05-03 22:05
date updated: 2022-05-04 12:32
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
