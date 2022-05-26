---
date created: 2022-05-03 22:05
date updated: 2022-05-16 11:58
---

### Content-Security-Policy

1. 限制资源获取
2. 报告资源获取越权
3. default-src 限制全局
4. connet-src 发送位置
5. img-src 图片来源
6. script-src js 来源

```js
'Content-Type': 'text/html',
'Content-Security-Policy': 'default-src http: https:' // 全局限制 只允许 http(s)
'Content-Security-Policy': 'default-src \'self\' http://code.jquery.com/' // 仅允许以下域名
'Content-Security-Policy': 'default-src \'self\'; form-action \'self\'' // 限制 form-data
'Content-Security-Policy': 'script-src \'self\'; form-action \'self\'; report-uri /report' // report
'Content-Security-Policy-Report-Only': 
	'script-src \'self\'; form-action \'self\'; report-uri /report' // 仅 report，不限制文件

<!-- 限制 Ajax 请求， 不允许写 report-uri -->
<meta http-equiv="Content-Security-Policy" content="connect-src 'self'; form-action 'self'; report-uri /report">
```
