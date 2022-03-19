---
date created: 2022-03-19 17:24
date updated: 2022-03-19 20:16
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

- class `ReactiveEffect` (fn)
	- `run`
- export `track` (target, key)
- export `trigger` (target, key)
- export `effect` (fn) { return `runner` }

#### reactive

- new `Proxy`(raw,  { `get`(target,  key) { `track` () }, `set`(target, key, value) { `trigger` () } })
