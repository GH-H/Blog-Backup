---
title: 链表 Linked List
date: 2020-03-21 21:29:44
toc: true
thumbnail: /thumbnails/data.png
categories:
	- [数据结构,线性结构]
tags: 
	- 数据结构
---
## 概述
理论上长度可以无限拓展
1. 以节点的方式储存
2. 每个节点包含data，next：指向下一个节点
3. 各个节点不一定是在连续的储存位置
<!--more-->
### 特性
读取数据时需要遍历，而数组可以直接读取

## 不简洁的list优化
改list的大小可以通过迭代和递归得到。如果有一个变量记录大小则可以省略不必要的计算。接下来的链表都会以此优化

**造轮子**
```java
public class IntList {
    public int first;
    public IntList rest;

    public IntList(int first,IntList rest){
        this.first = first;
        this.rest = rest;
    }

    // return size of the list using recursion
    public int size(){
        if(rest == null){
            return 1;
        }
        return 1 + this.rest.size();
    }

    //return size of the list no recursion
    public int iterativeSize(){
        IntList p = this;
        int count = 0;

        while(p!= null){
            count ++;
            p = p.rest;
        }
        return count;
    }

}
```

## 单向链表

### 思路
1. 内部节点,addFront,addEnd，getFirst
2. 内置head变量，避免空指针
3. 内置size变量，避免不必要的计算

### 缺点
1. 末尾删除数据需要遍历一遍，如果用一个指针指向末尾，删除后还需要指向前一位，还是得遍历一遍
2. 双向链表解决了这个问题

**为什么要用static node？**
这个node不会用到任何实列或者函数，可以用static节省内存使用 

**造轮子**
```java
public class SLList {
    //节点
    public static class IntNode {
        public int item;
        public IntNode next;

        public IntNode(int item, IntNode next) {
            this.item = item;
            this.next = next;
        }
    }

    private IntNode head;
    private int size;

    public SLList(){
        this.head = new IntNode(666,null);
        size = 0;
    }
    //initialize
    public SLList(int x) {
        this.head = new IntNode(666,null);
        this.head.next = new IntNode(x,null);
        size = 1;
    }

    //add to the front of the list
    public void addFirst(int x){
        head.next = new IntNode(x, head.next);
        size+=1;
    }

    //add element to the end
    public void addLast(int x){
        IntNode p = head;//相当于指针

        while(p.next != null){
            p = p.next;
        }
        p.next = new IntNode(x,null);
        size+=1;
    }

    public int size(){
        return size;
    }

    //returns first item int the list
    public int getHead(){
        return head.next.item;
    }
}
```


## 双向列表
加入head 和 tail指向前和后， 可以快速从尾部插入

###思路

