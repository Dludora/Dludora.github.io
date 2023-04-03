---
title: vue事件修饰符
date: 2023-04-03 19:19:05
tags: "vue"
category: "vue"
---
# vue事件修饰符
## 事件修饰符
Vue 为 `v-on` 提供了事件修饰符。修饰符是用 . 表示的指令后缀，包含以下这些：        
| 修饰符   | 含义                                                               |
| -------- | ------------------------------------------------------------------ |
| .stop    | 阻止事件向上冒泡（不会触发父元素的事件）                           |
| .prevent | 阻止默认事件（比如组织`<a>`的默认跳转）                            |
| .capture | 添加事件监听器时使用事件捕获模式（先触发父元素事件，再触发子元素） |
| .self    | 只当事件在该元素本身（比如不是子元素时）触发事件                   |
| .once    | 事件只能触发一次                                                   |

```html
<!-- 单击事件将停止传递 -->
<a @click.stop="doThis"></a>

<!-- 提交事件将不再重新加载页面 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修饰语可以使用链式书写 -->
<a @click.stop.prevent="doThat"></a>

<!-- 也可以只有修饰符 -->
<form @submit.prevent></form>

<!-- 仅当 event.target 是元素本身时才会触发事件处理器 -->
<!-- 例如：事件处理器不来自子元素 -->
<div @click.self="doThat">...</div>

```
## 按键修饰符

### 按键别名
vue为一些常用的按键提供了别名
- .enter
- .tab
- .delete (捕获“Delete”和“Backspace”两个按键)
- .esc
- .space
- .up
- .down
- .left
- .right

## 鼠标按键修饰符

- .left
- .right
- .middle
这些修饰符将处理程序限定为由特定鼠标按键触发的事件。