---
title: watch侦听器
date: 2023-03-20 21:57:46
tags: "vue"
---







# 认识watch侦听器

`watch` 需要侦听特定的数据源，并在单独的回调函数中执行副作用

- watch第一个参数监听源

- watch第二个参数回调函数cb（newVal,oldVal）

- watch第三个参数一个options配置项是一个对象

  {

  - immediate: true,			是否立即调用一次
  - deep: true                      是否深度侦听

  }

### 监听Ref 案例

```typescript
import { ref, watch } from 'vue'

let message = ref({
    nav:{
        bar:{
            name:""
        }
    }
})
 
watch(message, 
(newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
},
{
    immediate:true,
    deep:true
})
```

### 侦听多个ref

```typescript
import { ref, watch ,reactive} from 'vue'
 
let message = ref('')
let message2 = ref('')
 
watch([message,message2], (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
})
```

### 监听Reactive

使用reactive监听深层对象开启和不开启deep 效果一样

```typescript
import { ref, watch ,reactive} from 'vue'
 
let message = reactive({
    nav:{
        bar:{
            name:""
        }
    }
})

watch(message, (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
})
```

### 监听reactive 单一值

```typescript
import { ref, watch ,reactive} from 'vue'
 
let message = reactive({
    name:"",
    name2:""
})

watch(()=>message.name, (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
})
```

