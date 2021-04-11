---
title: 单列模式 Singleton
date: 2021-03-24 18:43:43
toc: true
thumbnail: /thumbnails/design.png
categories:
	- 设计模式
tags: 
	- 设计模式
---

## 饿汉式
 类加载到内存后实列化一个单例，JVM保证线程安全
 缺点：不论是否用到，都会在类装载时完成实例化
 实用但不完美
<!--more-->
```java
public class T1 {
    private static final T1 INSTANCE = new T1();
    private T1(){}

    public static T1 getInstance(){return INSTANCE;}

    public void m(){
        System.out.println("test");
    }

    public static void main(String[] args) {
        T1 e1 = T1.getInstance();
        T1 e2 = T1.getInstance();
        System.out.println(e1 == e2);
    }
}
```
在其它class里新建一个实列会报错
```java
public class Main {
    public static void main(String[] args) {
        T1 test = new T1();//error
        T1 test2 = T1.getInstance();
    }
}
```
## 静态内部类方式
 JVM单例
 加载外部类时不会加载内部类，可以实现懒加载
 最完美

```java
public class T3 {
    private T3(){}

    private static class T3Holder{
        private final static T3 INSTANCE = new T3();
    }

    public void m(){
        System.out.println("test");
    }

    public static T3 getInstance(){
        return T3Holder.INSTANCE;
    }
}
```

## 懒汉式
 按需求初始化，加synchronized保证线程安全，但效率下降
 相对完美但不实用
```java
public class T2 {
    private static volatile T2 INSTANCE;

    private T2(){}

    public void m(){
        System.out.println("test");
    }

    public static T2 getInstance(){
        if(INSTANCE == null){
            synchronized (T2.class){
                if(INSTANCE == null){
                    INSTANCE = new T2();
                }
            }
        }
        return INSTANCE;
    }
}
```
## 枚举
完美中的完美，可以解决线程同步，防止反序列化
```java
public enum T4 {

    INSTANCE;

    public void m(){
        System.out.println("test");
    }
}
```