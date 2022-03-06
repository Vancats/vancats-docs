---
date created: 2022-03-03 00:34
date updated: 2022-03-06 15:02
---

HTML5

- 语义化标签
- 表单功能增强
- 音视频标签
- Canvas SVG
- 拖放功能
- 本地存储
- WebWorker
- 地理位置

网络基础（了解）
TCP/UDP（掌握）
HTTP（精通）

TCP/IP参考模型

- 应用层：HTTP、FTP、Telnet...
- 传输层：TCP、UDP
- 网络层：IP
- 网络接口层（数据链路层+物理层）

#### IP

**地址**

- IPv4 32位4字节：192.168.0.1
- IPv6 128位6字节

**协议**

- 只尽**最大努力交付**，可能会中途丢失
- IP 报文不能确保顺序
- 是无连接的协议，只管发

#### TCP

- 端口
	- 共有 2^16 0~65535
	- 前 8000 个需要申报
- 单域名最多允许同时建立 6 条连接
	- keep-alive
	- 精灵图：减少 HTTP 请求，本质是减少建立多个 TCP 的性能损耗
	- 多域名：将资源放入不同的域名下，可以突破浏览器的单域名并发连接限制
	- HTTP2：单个 TCP 连接的多路复用

#### HTTP

Request
Response

- 跨域
	- JSONP
- 缓存
	- 强缓存
		- expires
		- cache-control
	- 协商缓存
		- last-modified & if-last-modified-since
		- etag & if-node-match
- HTTP2
	- 多路复用
	- 首部压缩
	- 服务端推送
