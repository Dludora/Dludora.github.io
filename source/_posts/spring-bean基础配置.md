---
title: spring_bean基础配置
date: 2022-10-03 17:04:19
tags: "SSM"
---

# Spring bean基础配置

## bean基础知识

bean本质上是对象

### 实例化bean的三种方法

1. 使用构造方法完成

   - 不管bean调用的对象定义的构造方法是私有/共有，bean都可以进行调用。

   - 调用的是无参的构造方法(不能有参数)。 

     ```xml
             <bean id="bookDao" class="com.itheima.dao.imp.BookDaoImpl1"/>
     ```

2. 使用静态工厂实例化bean (了解)

   - ```java
     // 工厂对象
     public class BookDaoFactory {
         public static BookDao getBookDao() {
             return new BookDaoImpl1();
         }
     }
     ```

   - ```xml
             <bean id="bookDao" class="com.itheima.Factory.BookDaoFactory" factory-method="getBookDao"/>
     ```

3. 使用实例工厂实例化bean ( 重要! )

   - ```java
     // 工厂对象
     public class BookDaoFactoryBean implements FactoryBean<BookDao> {
         // 代替原始实例工厂中创建对象的方法
         public BookDao getObject() throws Exception {
             return new BookDaoImpl1();
         }
         
         public Class<?> getObjectType() {
             return BookDao.class;
         }
         
         public boolean isSingleton() {
             // 是单例
             return true;
         }
     }
     ```

   - ```xml
         <bean id="bookDao" class="com.itheima.Factory.BookDaoFactory" />    
     ```

## 原代码

```java
// 业务层
public class BookService implements BookService {
    // 导致耦合度高
    private BookDao bookDao = new BookDaoImpl1();
    
    public void save() {
        bookDao.save();
    }
}

// 数据层
public class BookDaoImpl1 implements BookDao {
    public void save() {
        System.out.println("book dao save...")
    }
}
```

## Spring配置

### spring依赖引入

- 在`pom.xml`文件中加入

  ```xml
      <dependencies>
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-context</artifactId>
              <version>5.3.18</version>
          </dependency>
      </dependencies>
  ```

  引入spring依赖

### 配置项目resource目录

- 新建`applicationContext.xml`文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
          <!-- 1. 导入spring的坐标spring-context-->
  
          <!-- 2. 配置bean (配置实现类) -->
          <bean id="bookDao" class="com.itheima.dao.imp.BookDaoImpl1"/>
          <!-- 4. bean的作用范围分为singleton(单例)和 prototype(多例), 由scope属性配置，默认为singleton-->
          <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl" scope="singleton">
                  <!-- 3. 配置service与dao的关系
                       property标签表示配置当前bean的属性
                       name属性表示配置哪一个具体的属性
                       ref属性表示参照哪一个bean
                  -->
                  <property name="bookDao" ref="bookDao" />
          </bean>
  </beans>
  ```

### 业务层代码改写

```java
// 业务层
public class BookService implements BookService {
    // 改写所有由new方法新建的属性
    private BookDao bookDao;
    
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
    
    public void save() {
        bookDao.save();
    }
}

// 数据层
public class BookDaoImpl1 implements BookDao {
    public void save() {
        System.out.println("book dao save...")
    }
}
```

### 运行程序

```java
package com.itheima;

import com.itheima.service.BookService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App2 {
    public static void main(String[] args) {
        // 获取IoC容器
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 获取bean
        BookService bookService = (BookService) ctx.getBean("bookService");

        bookService.save();
    }
}
```

