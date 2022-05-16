---
date created: 2022-04-23 20:48
date updated: 2022-04-23 20:48
---

Vue3 定义 Props

```ts
// 1. ts 专用声明，无默认值
defineProps<{
	msg: string
}>

// 2. ts 专用声明，可以有默认值
interface Props {
	msg: string
}

const props = withDefaults(defineProps<Props>(), {
	msg: string
})

// 3. 非 ts 专用声明
defineProps({
	msg: String // 大写
})
```
