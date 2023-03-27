---
title: spring引论
date: 2022-10-03 13:05:35
tags: "SSM"
---

# Spring引论

![image-20221003133303223.png](https://img1.imgtp.com/2022/10/03/qTPYca1w.png)

##  核心概念

- 代码书写现状

  - 耦合度太高(应该是低耦合)

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

    **如果数据层发生改变(换成别的)，则业务层也需要重新变动、编译、测试、部署、发布。**

  - 解决方案

    - 使用对象时，在程序中不要主动使用new 产生对象，转换为由**外部提供对象**。

    - 这种方法被称为IoC( Inversion of control )

      - 对象的创建控制权由程序转移到外部
      - Spring技术对IoC思想进行了实现
        - Spring 提供了一个容器，称为**IoC容器**，用来充当IoC思想中的外部
        - IoC容器**负责对象的创建、初始化**等一系列工作，被创建或者被管理的对象再在IoC容器中统称为**Bean**
      - DI ( Dependency Injection )依赖注入
        - 在容器中建立bean和bean之间依赖关系的整个过程，称为依赖注入

      ~~~java
      // 业务层
      public class BookService implements BookService {
          // 导致耦合度高
          private BookDao bookDao;
          
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
      
      
      // IoC容器 (管理大量对象的创建、初始化过程)
      `````````````````````````````````````````````````````````````````````````````````````````
      `																				`			
      `		service------>dao														  `
      `																				`
      `````````````````````````````````````````````````````````````````````````````````````````
      ~~~

      

      

