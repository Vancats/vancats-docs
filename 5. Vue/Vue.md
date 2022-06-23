---
date created: 2022-05-28 23:50
date updated: 2022-06-23 16:18
---

### MVVM 的理解

1. MCV：用户操作请求服务端路由，路由调用相应控制器，它获取数据再返给前端渲染
2. MVVM：用户不用手动操作DOM，而是绑定到 ViewModel 层，视图变化则更新数据

### 响应式数据的原理

主要原理是使用 Object.defineProperty 来实现对 get set 属性的拦截，在 get 操作时，会为该 key 创建一个 dep，用于收集与该值相关的所有 watcher，而在修改该值的时候，会执行 dep.notify 函数，它会遍历存储的所有 watcher，执行它的 get 来更新所有相关数据

### 如何检测数组变化

在 vue2 中，数组和对象的处理方式实际上是不同的，首先是覆盖其原型链上可以修改数组的七个方法，使其可以进行响应式通知，对有可能生成的新数据也需要进行响应式

### nextTick

这是 vue 的异步机制，我们在操作时，往往一次性会有许多的更新操作，每一步的操作都执行更新函数显然不必要，所以用异步的方式来进行数据的更新会节省很多性能。当我们需要进行异步更新的时候，我们会打开一个 waiting 的打开，进而开启 nextTick，传入 flushSchedulerQueue，这是一个执行所有 watcher 的函数（会排序），我们会把这个函数存入一个 callbacks 数组中，然后打开 pending 开关，开启 timeFunc 的异步事件，其中有异步的降级策略，Promise - MutationObserver - setImmediate - setTimeout，然后循环执行 callbacks 中的每一项，也就是每一个 flushSchedulerQueue

### 更新流程

dep.notify - watcher.update - queueWatcher - nextTick(flushSchedulerQueue) - timeFunc - flushScheduler - watcher.run / get

### computed 实现

首先我们要知道的是，Computed 也是依靠于 Watcher 来实现的，只不过在 new Watcher 的时候会额外传一个 lazy 属性，里面会维护一个 dirty 属性，该属性决定是否需要重新加载，从而获得懒执行的效果。当我们更新的时候，会走 watcher.update，如果是 computed，它只会更新 dirty 属性为 true，为后续操作做服务。computed 覆盖了原有的 get set 属性，使其走的是 watcher.evaluate，该函数执行了 watcher.get 并且置 dirty 为 false

### watcher deep 实现

如果开启了 deep 属性，那么会在 watcher.get 的 finally 中执行一个 traverse 函数，它会递归遍历所有值，此时还处在收集状态，因此会对所有值进行依赖收集
**为什么 computed 不需要 deep**：底层采用 JSON.stringify，总是会对所有属性值取值

### v-if/show

if 与 show 都是控制显隐的 API，但是他们本质上还是有一定区别的，在编译过后，if 会直接变成三元表达式，show 会编译出一个 directives，其中的 value 是 false，最终在 runtime 时，会进行 bind 绑定操作，其中会把当前的 el display none

```js
<div id="test">
  <p v-if="false">测试</p>
</div>

with (this) {
  return _c('div', { attrs: { "id": "test" } }, [
    (false) ? _c('p', [_v("测试")]) : _e()
  ])
}

<div id="test">
  <p v-show="false">测试</p>
</div>

with (this) {
  return _c('div', { attrs: { "id": "test" } }, [
    _c('p', {
      directives: [{ name: "show", rawName: "v-show", value: (false), expression: "false" }]
    }, [_v("测试")])])
}

bind (el: any, { value }: VNodeDirective, vnode: VNodeWithData) {
  vnode = locateNode(vnode)
  const transition = vnode.data && vnode.data.transition
  const originalDisplay = el.__vOriginalDisplay =
    el.style.display === 'none' ? '' : el.style.display
  if (value && transition) {
    vnode.data.show = true
    enter(vnode, () => {
      el.style.display = originalDisplay
    })
  } else {
    el.style.display = value ? originalDisplay : 'none'
  }
},
```

### v-for/if

```js
<div id="test">
  <ul>
    <li v-for="ind in 5"> // 1
    <li v-for="ind in 5" v-if="ind === 5"> // 2
      {{ ind }}
    </li>
  </ul>
</div>`

with (this) {
  return _c('div', { attrs: { "id": "test" } }, [
    _c('ul',
      _l((5), function (ind) {
		return               _c('li', [_v("\n      " + _s(ind) + "\n    ")]) // 1
        return (ind === 5) ? _c('li', [_v("\n      " + _s(ind) + "\n    ")]) : _e() // 2
      }), 0)
  ])
}
```

### v-html/text

```js
<div id="test">
  <p v-html="'<div>23231213{{title}}<div>'">123</p>
</div>

with (this) {
  return _c('div', { attrs: { "id": "test" } }, [
    _c('p', {
      domProps: { "innerHTML": _s('<div>23231213{{title}}<div>') }
    }, [_v("123")])
  ])
}

<div id="test">
  <p v-text="'<div>23231213{{title}}<div>'">123</p>
</div>

with (this) {
  return _c('div', { attrs: { "id": "test" } }, [
    _c('p', {
      domProps: { "textContent": _s('<div>23231213{{title}}<div>') }
    }, [_v("123")])
  ])
}
```

### v-model

这是和 input 类型节点进行双向绑定，是赋值加触发事件的的语法糖，其对于普通节点，采用的是 input 触发事件，如果有 lazy 则改为 change，对于 radio checkbox select textarea 采用 change 事件，如果有 number trim 则会在开始会 forceUpdate，如果是组件的话，会对其设值以及添加回调函数，而此外，对于非 lazy 的普通 input，还会绑定键盘事件。

```js
<div id="test">
  <input v-model="msg" />
</div>

with (this) {
  return _c('div', { attrs: { "id": "test" } }, [
    _c('input', {
      directives: [{ name: "model", rawName: "v-model", value: (msg), expression: "msg" }],
      domProps: { "value": (msg) },
      on: {
        "input": function ($event) {
          if ($event.target.composing) return
          msg = $event.target.value
        }
      }
    })
  ])
}
```

### diff 流程

首先判断两个节点的 text children，主要对比两个 children，依次按照头头，尾尾，头尾，尾头的顺序对比，有相同的直接patch，然后首先我们会获取一份旧DOM的 key 的映射数组，如果没有设置key，那么会直接根据索引映射。之后在新DOM中查找，有key直接数据找，没key遍历查找，如果找不到或者是同key不同元素就创建新节点，找到了就patch。如果v-for不设置key，那么两个节点基本只要标签相同就会进入diff，会进行很多无谓的操作。
