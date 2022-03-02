---
date created: 2022-03-03 00:34
date updated: 2022-03-03 00:52
---

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

5. 重定向的路由，导航守卫应用在最终目标上
6. 使用 props 解藕 $route

```js
// router文件
// 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
{
	path: '/number/:name',
	props: true, 
	// 对象模式 props: { newsletterPopup: false }
	// 函数模式 props: (route) => ({ query: route.parmas.name }) 
	name: 'number',
	component: () => import( /* webpackChunkName: "number" */ './views/Number.vue')
}

export default {
	props: ['name']
}
```

7. push 和 replace 的第二个第三个参数

```js
eg1
// 组件1
this.$router.push({ name: 'number' }, () => {
	console.log('组件1：onComplete回调')
}, () => {
	console.log('组件1：onAbort回调')
})

// 组件2
beforeRouteEnter(to, from, next) {
	console.log('组件2：beforeRouteEnter')
	next()
},
beforeCreate() {
	console.log('组件2：beforeCreate')
},
created() {
	console.log('组件2：created')
}

// beforeRouteEnter -> onComplete -> beforeCreate -> created


eg2 不带参数的自我跳转，触发 onAbort
this.$router.push({ name: 'number'}, () => {
	console.log('组件2：onComplete回调')
}, () => {
	console.log('组件2,自我跳转：onAbort回调')
})


eg3 带参数的自我跳转，两个函数都不触发
this.$router.push({ name: 'number', params: { foo: this.number}}, () => {
	console.log('组件2：onComplete回调')
}, () => {
	console.log('组件2,自我跳转：onAbort回调')
})
```
