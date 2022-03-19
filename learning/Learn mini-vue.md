---
date created: 2022-03-19 17:24
date updated: 2022-03-20 00:17
---

##### 配置 jest 环境

- 安装依赖
	- typescript、jest、@types/jest    **添加 tsconfig.json  npx tsc --init**
	- babel-jest @babel/preset-env @babel/core @babel/preset-typescript

## Reactive

#### effect

**全局变量**

- `activeEffect`
- `targetMap` = new Map(target, `depsMap` = new Map(key, `deps` = new Set(activeEffect)))

**函数**

- class `ReactiveEffect` (fn, scheduler?)
	- 参数：fn, scheduler, deps(存放所有和该 effect 相关的 dep), active(stop 中性能优化使用), onStop(使用 stop 时的回调)
	- `run`
	- `stop`
- export `track` (target, key) 依赖收集，分为三层
- export `trigger` (target, key)
- export `effect`
	- 通过 ReactEffect 生成 `_effect`
	- 返回值 `runner`：调用 fn，并且返回 fn 的返回值
	- `options`
		- `scheduler`：初始化执行 fn，后续的 trigger 执行 scheduler
		- `stop`：通过直接删除和该 effect 相关的所有 deps 来中止后续的响应式

#### reactive

- new `Proxy`(raw,  { `get`(target,  key) { `track` () }, `set`(target, key, value) { `trigger` () } })
