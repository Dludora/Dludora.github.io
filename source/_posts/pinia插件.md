---
title: pinia插件
date: 2022-09-01 19:32:54
tags: "vue"
---

# Pinia全局插件

### 1、安装pinia

```
npm install pinia -S
```

### 2、引入pinia

```typescript
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

app.use(createPinia())
app.mount('#app')
```

### 3、改变pinia

```typescript
import {defineStore} from 'pinia'

export const useCounterStore = defineStore({
    id: 'counter',
    state: () => ({
        count: 1,
        name: "ksr"
    }),
    getters: {
    },
    actions: {
        setCurrent(value: any) {
            this.count = value
        }
    },
})

```

**在组件中改变其值的方法**

```typescript
import {useCounterStore} from './stores/counter'

const counter = useCounterStore()
// 方法一
counter.count++
// 方法二
counter.$patch({current: 2, name: "kss"})
// 方法三
counter.$patch((state) => {
    state.current = 2
    state.name = "kss"
})
// 方法四
counter.$state = {current: 2, name: "kss"}
// 方法五
counter.setCurrent(2)
```

### pinia 持续化

```
npm install pinia-plugin-persistedstate -S
```

```typescript
import piniaPersist from 'pinia-plugin-persistedstate'
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

pinia.use(piniaPersist)
app.use(pinia)
app.mount('#app')
```

```typescript
import {defineStore} from 'pinia'

export const useCounterStore = defineStore({
    id: 'counter',
    state: () => ({
        count: 1,
        name: "ksr"
    }),
    getters: {
    },
    actions: {
        setCurrent(value: any) {
            this.count = value
        }
    },
    persist: true
})
```

