---
date created: 2022-03-19 17:24
date updated: 2022-04-01 23:11
---

#### 配置环境

- 安装依赖
	- typescript、jest、@types/jest    **添加 tsconfig.json  npx tsc --init**
	- babel-jest @babel/preset-env @babel/core @babel/preset-typescript

## Reactive

### effect

###### 全局变量

- `activeEffect`
- `shouldTrack`
- `targetMap` = new Map(target, `depsMap` = new Map(key, `deps` = new Set(activeEffect)))

###### 函数

- export class `ReactiveEffect` (fn, scheduler?)
	- 参数：fn, scheduler, deps(存放所有和该 effect 相关的 dep), active(stop 中性能优化使用), onStop(使用 stop 时的回调)
	- `run`
	- `stop`
- export `track` (target, key) 依赖收集，分为三层；注意点：如果只是依赖收集，和 effect 相关的内容不执行
- `cleanupEffect`
- export `trigger` (target, key)
- `isTracking`：解决 stop 后，get 时继续收集依赖问题
- export `stop` 调用 ReactiveEffect 的 stop
- export `effect`
	- 通过 ReactEffect 生成 `_effect`
	- 返回值 `runner`：调用 fn，并且返回 fn 的返回值
	- `options`
		- `scheduler`：初始化执行 fn，后续的 trigger 执行 scheduler
		- `stop`：通过直接删除和该 effect 相关的所有 deps 来中止后续的响应式，可以实现回调

### reactive

###### 全局变量

- ReactiveFlags
	- IS_REACTIVE: `__v_isReactive`
	- IS_READONLY: `__v_isReadonly`

###### 函数

- export `reactive`
- export `readonly`
- export `shallowReadonly`
- `createActiveObject`
- export `isReactive`
- export `isReadonly`
- export `isProxy`

### baseHandles

- createGetter：实现对象嵌套 reactive
- createSetter
- mutableHandlers：注意使用外部 get，而不是每次都重复生成
- readonlyHandlers
- shallowReadonlyHandlers

### computed

- `computed`
- `ComputedRefImpl`
	- `dirty`
	- `getter`
	- `_value`
	- `_effect`

### ref

- class `RefImpl`
	- `_value`
	- `public dep`
	- `_rawValue`
	- `__v_isRef`
- `convert`
- `trackRefValue`
- `isRef`
- `unRef`
- `proxyRefs`

## Runtime-core

### createApp
- export createAppAPI
- createApp

### renderer
- export createRenderer
	- return createApp: createAppAPI(render)
- render
- patch
- processComponent
- mountComponent
- createComponentInstance
- setupComponent
- setupRenderEffect
- processElement
- mountElement
- mountChildren
- processText
- processFragment

### component

- export createComponentInstance
- export setupComponent
- setupStatefulComponent
- handerSetupResult
- finishComponentSetup
- export getCurrentInstance
- setCurrentInstance

### componentPublicInstance

- publicPropertiesMap
- export PublicInstanceProxyHandlers

### componentProps

- export initProps

### componentEmit

- export emit

### componentSlot

- export initSlots
- normalizeObjectSlots
- normalizeSlotValue

### renderSlots

### h

### ShapeFlags

### vnode

###### 全局变量

- export Text
- export Fragment

###### 函数

- export createVNode：设置 shapeFlag
- getShapeFlag
- export createTextVNode

### apiInject
- export inject
- export provide