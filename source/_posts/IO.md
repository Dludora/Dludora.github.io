---
title: "Java IO流"
date: 2022-06-30T11:42:08+08:00
tags: "Java"
---

# Java IO 流

## 一、文件操作

### 1、创建文件

`new File(String pathname)`	根据路径构建一个File对象

`new File(File parent, String child)`	根据父目录文件+子路径构造

`new File(String parent, String child)`	根据父目录+子路径构建

创建文件对象后,执行  `file.createNewFile`

### 2、获取文件信息

`file.getName()`文件名字

`file.getAbsolutePath()`文件绝对路径

`file.getParent`文件父级目录

`file.length()`文件大小(字节)

`file.exists()` 文件/目录是否存在

`file.isFile()`是否是文件

`file.isDirectory()`是否是目录

### 3、目录操作

`file.delete()`	删除文件/目录

`file.mkdir()`	创建一级目录

`file.mkdirs()`	创建多级目录

## 二、Java IO流原理及其分类

​	I/O是input/output的缩写，按照**操作数据单位**不同分为`字节流`，`字符流`。字节流操作二进制文件时更加适合，字符流操作文本文件更加适合。按照**流的角色**分为`节点流`、`处理流/包装流`。

​	**IO流使用完毕后必须关闭!**

| 抽象基类 |    字节流    | 字符流 |
| :------: | :----------: | :----: |
|  输入流  | InputStream  | Reader |
|  输出流  | OutputStream | Writer |

节点流是从某一个特定的数据源读写数据，如`FileReader`、`FileWriter`。处理流是包装节点流，可以消除不同节点流的实现差异，以更加方便的方法实现输入输出，使用了修饰器设计模式，不会直接与数据源相连，如`BufferedReader`、`BufferedWriter`。

### 1、InputStream

#### 1.1 BufferedInputStream	缓冲字节流

#### 1.2 FileInputStream	文件字节流

构造方法：

- FIleInputStream(File file)
- FIleInputStream(String filePath)
- FIleInputStream(FileDescription fdObj)

方法

- `int read(byte[] b)`读取一个字节的数据，返回实际读取的字节数。若达到文件末尾，返回-1。如果在其中加入`byte[] b`，则一次获取最多`b.length`字节的数据到字节数组。 可以把`byte`数组转成字符串`new String(b, 0, readLen)`

#### 1.3 ObjectInputStream	对象字节流

**需求**

- 将`int num = 100`这个int数据保存到文件中，且能从文件中直接回复int 100
- 将`Dog dog = new Dog("小黄", 3)`这个dog对象保存到文件中，并且能从文件恢复
- 能够将**基本数据类型**或者**对象**进行**序列化**和**反序列化**操作

### 2、OutputStream

#### 2.1 FileOutputStream

`FileOutputStream(filePath, append) `如果`append`是`true`而不是`false`时，以追加而不是覆盖的方式写入文件。

- `write(byte[] b, off, len)`会覆盖之前的文件，可以通过`string.getBytes()`把字符串转换为字节数组，输出到对应文件。

### 3 Reader

#### 3.1 FileReader

与`FileInputStream`类似，读取单位变为字符。

#### 3.2 BufferedReader

**构造**

- `BufferedReader(Reader in)`	使用默认的缓冲区大小8192个字符来创建缓冲字符输入流
- `BufferedReader(Reader in, int sz)`	指定缓冲区大小

**方法**

- `fill()`	从底层输入流中填充字符到缓冲区中
- `string = readLine()` 读取一行字符串
- `read(c[], off, len)`将最多len个字符读入数组中，返回实际读入的字符个数，督导文件末尾时返回-1

### 4 Writer

#### 4.1 BufferedWriter

**构造**

- `BufferedRWriter(Reader out)`	使用默认的缓冲区大小8192个字符来创建缓冲字符输入流
- `BufferedWriter(Readered out, int sz)`	指定缓冲区大小

**方法**

- `write(char c[], off, len)`
- `write(int c)`
- `write(String s, off, len)`
- `close()`
- `newLine()` 写入一个行分隔符
- `flush()`  刷新该流的缓冲

#### 4.2 ObjectWriter

## 三、Properties类

**需求**

如下一个配置文件`mysql.properties`,通过编程获取ip，user，pwd

```
ip=192.168.0.13
user=root
pwd=12345
```

**方法**

- `load(Reader reader)` 加载配置文件的键值对到`Properties`对象
- `list(PrintStream out)`将数据显示到指定设备
- `getProperty(key)`根据键获取值
- `setProperty(key, value)`设置键值对到`Properties`对象
- `store(Writer writer, String Comments)`将`Properties`中的键值对存储到配置文件
