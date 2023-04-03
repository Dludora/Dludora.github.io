---
title: ref模板引用
date: 2023-04-03 18:19:13
tags: "vue"
category:"vue"
---
# ref模板引用
Vue 为我们处理了所有的 DOM 更新，这要归功于响应性和声明式渲染。然而，有时我们也会不可避免地需要手动操作 DOM。

这时我们需要使用**模板引用**——也就是指向模板中一个 DOM 元素的 ref。我们需要通过这个特殊的 ref attribute 来实现模板引用：
```html
<p ref="p">hello</p>
```
要访问该引用，我们需要声明一个同名的 ref：
```javascript
const p = ref(null);
```
注意这个 `ref` 使用 `null` 值来初始化。这是因为当 `<script setup>` 执行时，DOM 元素还不存在。模板引用 `ref` 只能在组件挂载后访问。
要在挂载之后执行代码，我们可以使用 `onMounted()` 函数：
```html
<script setup>
import { ref, onMounted } from 'vue'

const p = ref(null)

onMounted(() => {
  p.value.textContent = 'mounted!'
})
</script>

<template>
  <p ref="p">hello</p>
</template>
```