---
date created: 2022-03-19 17:24
date updated: 2022-03-19 21:35
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
	- `run`
- export `track` (target, key) 依赖收集，分为三层
- export `trigger` (target, key)
- export `effect`
	- 返回值 `runner`：调用 fn，并且返回 fn 的返回值
	- `scheduler`：初始化执行 fn，后续的 trigger 执行 scheduler

#### reactive

- new `Proxy`(raw,  { `get`(target,  key) { `track` () }, `set`(target, key, value) { `trigger` () } })
