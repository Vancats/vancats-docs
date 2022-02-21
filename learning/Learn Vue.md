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
```js
{ path: '/params/:foo?' }
{ path: '/params/*' }
{ path: '/params/:foo+' }
{ path: '/params/:id(\\d+)' }
{ path: '/params/(foo/)?bar'}
```
4. 路由视图
```js
<router-view class="view one"></router-view>  
<router-view class="view two" name="a"></router-view>  
<router-view class="view three" name="b"></router-view>

{  
	path: '/',  
	components: {  
		default: Foo,  
		a: Bar,  
		b: Baz  
	}  
}
```
5. 重定向的路由，导航守卫