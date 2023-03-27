---
title: python语法糖
date: 2022-09-14 17:47:47
tags: "python"
---

# Python语法糖

## if...else三元表达式

可以简化分支判断语句

```python
x = y.lower() if isinstance(y, str) else y
```

## 列表生成式

是一个用来生成列表的特定语法形式的表达式

```python
[exp for iter_var in iterable]
```

相当于

```python
L = []
for iter_var in iterable:
    L.append(exp)
```

### 带过滤功能

```python
[exp for iter_var in iterable if_exp]
```

相当于

```python
L = []
for iter_var in iterable:
    if_exp:
        L.append(exp)
```

### 循环嵌套

```python
[exp for iter_var_A in iterable_A for iter_var_B in iterable_B]
```

相当于

```python
L = []
for iter_var_A in iterable_A:
    for iter_var_B in iterable_B:
        L.append(exp)
```

## 生成器

按照某种算法不断生成新的数据，直到满足一个指定的条件结束

### 构造示例

```python
# 使用类似列表生成式的方式构造生成器
g1 = (2*n + 1 for n in range(3, 6))

# 使用包含yield的函数构造生成器
def my_range(start, end):
    for n in range(start, end):
        yield 2*n + 1

g2 = my_range(3, 6)
print(type(g1))
print(type(g2))
```

#### yield关键字实例

```python
# 生成斐波那契数列
def fib(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
        yield a


def main():
    for val in fib(20):
        print(val)


if __name__ == '__main__':
    main()
```

