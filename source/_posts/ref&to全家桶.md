---
title: ref&to全家桶
date: 2023-03-20 21:07:34
tags: "vue"
---

# Ref全家桶

## `ref`

接受一个内部值并返回一个**响应式**且可变的 ref 对象。ref 对象仅有一个 `.value` property，指向该内部值。

**案例：**

我们这样操作是无法改变message 的值 应为message 不是响应式的无法被vue 跟踪要改成ref

```vue
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>
  </div>
</template>
 
<script setup lang="ts">
let message: string = "我是message"
 
const changeMsg = () => {
   message = "change msg"
}
</script>
```

改为ref

Ref TS对应的接口

```typescript
interface Ref<T> {
  value: T
}
```

## `isRef`

判断是不是一个ref对象

```typescript
import { ref, Ref,isRef } from 'vue'
let message: Ref<string | number> = ref("我是message")
let notRef:number = 123
const changeMsg = () => {
  message.value = "change msg"
  console.log(isRef(message)); //true
  console.log(isRef(notRef)); //false  
}
```

## shallowRef

创建一个跟踪自身 `.value` 变化的 ref，但不会使其值也变成响应式的（只让它自身变成响应式，它内部的值不是响应式的）

例子：

直接修改其内部属性，渲染这样是不会改变的

```vue
<template>
  <div>
    <button @click="changeMsg">change</button>
    <div>{{ message }}</div>
  </div>
</template>
 
<script setup lang="ts">
import { Ref, shallowRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "小满"
})

// 这里直接改变内部值，不会变成响应式渲染
const changeMsg = () => {
  message.value.name = '大满'
}
</script>
```

反而这样是可以渲染的

```typescript
import { Ref, shallowRef } from 'vue'
type Obj = {
  name: string
}
let message: Ref<Obj> = shallowRef({
  name: "小满"
})
 
const changeMsg = () => {
  message.value = { name: "大满" }
}
```

# to全家桶

## toRef

- 如果原始对象是 非响应式 的就不会更新视图，而且不会改变原数据的值（一点用都没有）
- 如果原始对象是 响应式 的是会更新视图，并且改变数据的

```vue
<template>
   <div>
      <button @click="change">按钮</button>
      {{state}}
   </div>
</template>
 
<script setup lang="ts">
import { reactive, toRef } from 'vue'
 
const obj = {
   foo: 1,
   bar: 1
}
 
 
const state = toRef(obj, 'bar')
// bar 转化为响应式对象
 
const change = () => {
   state.value++
   // 不会改变原数据的值,且视图无变化
   console.log(obj, state);
}
</script>
```

## toRefs

可以帮我们批量创建ref对象主要是方便我们解构使用

```typescript
import { reactive, toRefs } from 'vue'
const obj = reactive({
   foo: 1,
   bar: 1
})
 
let { foo, bar } = toRefs(obj)
 
foo.value++
console.log(foo, bar);
```

## toRaw

将响应式对象转化为普通对象

```typescript
import { reactive, toRaw } from 'vue'
 
const obj = reactive({
   foo: 1,
   bar: 1
})
const state = toRaw(obj)
// 响应式对象转化为普通对象
 
const change = () => {
   console.log(obj, state);
}
```
