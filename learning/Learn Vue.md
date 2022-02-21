###### VueRouter
1. 响应路由参数变化
	1. `watch: { '$route' (to, from) {} }`
	2. `beforeRouteUpdate (to, from, next) {}`
2. 路由匹配：当含有通配符时，存在 patchMatch
```js
{ path: '/user-*'}
this.$router.push('user-admin')
this.$route.patchMatch = admin
```
3. 高级匹配模式
4. 