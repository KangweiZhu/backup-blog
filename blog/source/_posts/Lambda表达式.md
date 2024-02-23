---
title: 函数式编程
date: 2023-10-12 02:23:00
tags:
    - Java基础  
    - Java
    - Lambda表达式

categories:
    - Java基础
---

## 1.概述
Lambda是JDK8中的一个**语法糖**。可以对匿名内部类的写法进行简化。是函数式编程思想的体现。
类型可以推导->省略类型
方法可以推导->省略方法

## 2.基本格式
(参数列表) -> 代码

## 3. 实例
之前在多线程的学习中提到过，线程Thread的创建方式比较知名的有三种：**实现Thread、 new Thread(new Runnable())、实现Callable接口**. 
在第二种方法: 实现Runnable接口时，我们可以用一个**匿名内部类**，来创建实现Runnable接口的对象。
```java
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("匿名内部类");
    }
})
thread.start();
```
通过Lambda，我们可以简化代码成：
```java
new Thread(()->System.out.println("匿名内部类")).start;
```

