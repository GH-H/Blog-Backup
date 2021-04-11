---
title: 集合 set
date: 2020-06-07 17:47:22
toc: true
thumbnail: /thumbnails/data.png
categories:
	- [数据结构,非线性结构]
tags: 
	- 数据结构
---
## 概述 
用于存储无序元素，值不能重复。
<!--more-->
## 重构思路
1. 添加时忽略重复元素
2. 添加内部类，iterator
3. implement iterable 后可以使用for each
4. 重写equal，toString
5. hashcode 暂时忽略


以下为generic构造
**造轮子**
```java
//implement iterable 后可以使用for each
public class ArraySet<T> implements Iterable<T>{
    private T[] items;
    private int size;

    //返回通用iterator
    public Iterator<T> iterator(){
        return new ArraySetIterator();
    }
    //构建通用iterator
    private class ArraySetIterator implements Iterator<T>{
        private int index;
        public ArraySetIterator(){
            index = 0;
        }

        public boolean hasNext(){
            return index < size;
        }

        public T next(){
            T returnItem = items[index];
            index++;
            return returnItem;
        }

    }

    public ArraySet(){
        items = (T[]) new Object[100];
        size = 0;
    }

    public boolean contains(T x){
        for(int i = 0;i<size;i++){
            if(x.equals(items[i])){
                return true;
            }
        }
        return false;
    }

    public void add(T x){
        if(x == null){
            throw new IllegalArgumentException("cannot add null yo the set");
        }
        if(contains(x)){
            return;
        }
        items[size] = x;
        size++;
    }

    public int size(){
        return size;
    }
    //toString 重写
    @Override
    public String toString(){
        String returnString = "{";
        for(T item: this){
            returnString += item.toString();
            returnString += ", ";
            return returnString;
        }
        returnString += items[size-1];
        returnString += "}";
        return returnString;
    }

    //Equals 重写
    @Override
    public boolean equals(Object other){
        if(other == this){
            return true;
        }
        if(other == null){
            return false;
        }

        if(other.getClass() != this.getClass()){
            return false;
        }
        ArraySet<T> o = (ArraySet<T>) other;
        if(o.size() == this.size()){
            return false;
        }
        for(T item: this){
            if(!o.contains(item)){
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        ArraySet<Integer> a = new ArraySet<Integer>();
        a.add(5);
        a.add(10);
        a.add(15);

        Iterator<Integer> iterator = a.iterator();
        while(iterator.hasNext()){
            int i = iterator.next();
            System.out.println(i);
        }

        for(int i : a){
            System.out.println(i);
        }
    }
}
```