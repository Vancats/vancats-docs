---
date created: 2022-05-10 01:44
date updated: 2022-05-10 21:19
---

```js
nginx -v
// 开启 nginx
sudo nginx
// 关闭 nginx
sudo nginx -s stop
// 重新加载 nginx
sudo nginx -s reload

// 给予管理员权限
sudo chown root:wheel /usr/local/Cellar/nginx/1.2.6/bin/nginx
sudo chmod u+s /usr/local/Cellar/nginx/1.2.6/bin/nginx
```
