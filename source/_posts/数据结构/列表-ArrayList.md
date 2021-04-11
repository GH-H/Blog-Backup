---
title: 列表 ArrayList
date: 2020-04-14 15:51:35
toc: true
thumbnail: /thumbnails/data.png
categories:
	- [数据结构,线性结构]
tags: 
	- 数据结构
---
## 概述

## 重构思路
1. size 表示表中数据数
2. last数据位置为size-1
3. 删除last时，返回last，只改动size
4. 如果size等于容量，扩容并使用arraycopy
<!--more-->
以下为generic构造
**造轮子**
```JAVA
public class AList<Item> {
    private Item[] items;
    private int size;

    public AList(){
        //cannot creat generic array objects,so we use object class and cast
        items = (Item[]) new Object[100];
        size = 0;
    }
    public void resize(int capacity){
        Item[] a = (Item[]) new Object[capacity];
        System.arraycopy(items,0,a,0,size);
        items = a;
    }
    public void  addLast(Item x){
        if(size == items.length){
            resize(size+1);
        }

        items[size] = x;
        size+=1;
    }

    public Item getLast(){
        return items[size-1];
    }

    public Item get(int i){
        return items[i];
    }

    public int getSize(){
        return size;
    }

    public Item removeLast(){
        Item x = getLast();
        size-=1;
        return x;
    }

}
```