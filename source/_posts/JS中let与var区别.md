---
title: JS中let与var区别
date: 2022-09-09 15:31:29
tags: "Javascript"
---

# JS中let与var区别

## var声明

### var声明作用域

var操作符定义的变量作用域为包含它的函数

```javascript
function test(a) {
	if(a === 1) {
		var b = 2;
		console.log(b);	// b = 2
	}
	console.log(b);		// b = 2
}
console.log(b);			// b = undefined
```

### var声明提升

```javascript
function foo() {
	console.log(age);
	var age = 26;
}
```

等价于

```javascript
function foo() {
	var age;
	console.log(age);
	age = 26;
}
```

## let声明

### let声明作用域

let声明的范围是**块作用域**

```javascript
function test(a) {
	if(a === 1) {
		var b = 2;
		console.log(b);	// b = 2
	}
	console.log(b);		// b = undefined
}
console.log(b);			// b = undefined
```

### 全局声明

与var不同，let在全局作用域中声明的变量不会成为windows对象的属性

```javascript
var name = "koushurui";
console.log(window.name);

let age = 26;
console.log(window.age);
```

### for循环中的let声明

在let出现之前，for循环定义的迭代变量回渗透到循环体外部

```javascript
for(var i = 0; i < 5; i++) {
	// 循环
}
console.log(i);
```

使用let后这个问题得到了解决

同样，在使用var的时候，最常见的问题是对迭代变量的奇特声明和修改

```javascript
for(var i = 0; i < 5; i++) {
	setTimeout(() => console.log(i), 0)
}
// 输出5, 5, 5, 5, 5
```

​	之所以会这样，是因为在退出循环时，迭代变量保存的是导致循环退出的值：5。在之后执行超时逻辑时，每个i都是同一变量。

​	而使用let声明迭代变量时，js后台会为每个循环迭代声明一个新的迭代变量。
