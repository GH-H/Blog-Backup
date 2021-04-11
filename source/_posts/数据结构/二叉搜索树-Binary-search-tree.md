---
title: 二叉搜索树 Binary search tree
date: 2020-06-012 17:47:22
toc: true
thumbnail: /thumbnails/data.png
categories:
	- [数据结构,非线性结构]
tags: 
	- 数据结构
---
## 概述 
1.改善了二叉树节点查找的效率
2.左子节点小于当前节点
3.右子节点大于当前节点
4.可将重复数值放在左或右子节点
<!--more-->
### 优点
相比于其他数据结构，查找、插入的时间复杂度较低,均为O(log n)。

先写node
```java
class Node{
    int key;
    Node left;
    Node right;

    public Node(int key) {
        this.key = key;
    }

    @Override
    public String toString() {
        return "key=" + key ;
    }
}

```

删除节点为叶子
删除节点有一颗子树
删除节点有两棵子树
```JAVA
public class BinarySearchTree {
    Node root;

    // This method mainly calls insertRec()
    void add(int key) { root = addNode(root, key); }

    /* A recursive function to
       insert a new key in BST */
    Node addNode(Node root, int key)
    {

        /* If the tree is empty,
          return a new node */
        if (root == null) {
            root = new Node(key);
            return root;
        }

        /* Otherwise, recur down the tree */
        if (key < root.key)
            root.left = addNode(root.left, key);
        else if (key > root.key)
            root.right = addNode(root.right, key);

        /* return the (unchanged) node pointer */
        return root;
    }

    public void preOrderTraverse(Node current){
        if(current != null){
            System.out.println(current);
            inOrderTraverse(current.left);
            inOrderTraverse(current.right);
        }
    }
    public void inOrderTraverse(Node current){
        if(current != null){
            inOrderTraverse(current.left);
            System.out.println(current);
            inOrderTraverse(current.right);
        }
    }
    public void postOrderTraverse(Node current){
        if(current != null){
            inOrderTraverse(current.left);
            inOrderTraverse(current.right);
            System.out.println(current);
        }
    }

    public Node findNode(int key){
        Node current = root;
        while(current.key != key){
            if(key < current.key){
                current = current.left;
            }else{
                current = current.right;
            }
            if (current == null){
                return null;
            }
        }
        return current;
    }

    // This method mainly calls deleteRec()
    void deleteKey(int key) { root = deleteNode(root, key); }

    /* A recursive function to
      delete an existing key in BST
     */
    Node deleteNode(Node root, int key)
    {
        /* Base Case: If the tree is empty */
        if (root == null)
            return root;

        /* Otherwise, recur down the tree */
        if (key < root.key)
            root.left = deleteNode(root.left, key);
        else if (key > root.key)
            root.right = deleteNode(root.right, key);

            // if key is same as root's
            // key, then This is the
            // node to be deleted
        else {
            // node with only one child or no child
            if (root.left == null)
                return root.right;
            else if (root.right == null)
                return root.left;

            // node with two children: Get the inorder
            // successor (smallest in the right subtree)
            root.key = minValue(root.right);

            // Delete the inorder successor
            root.right = deleteNode(root.right, root.key);
        }

        return root;
    }

    int minValue(Node root)
    {
        int minv = root.key;
        while (root.left != null)
        {
            minv = root.left.key;
            root = root.left;
        }
        return minv;
    }

    public static void main(String[] args) {
        BinarySearchTree tree = new BinarySearchTree();

        tree.add(4);
        tree.add(5);
        tree.add(3);
        tree.add(1);
        tree.add(2);
        tree.inOrderTraverse(tree.root);
        //tree.preOrderTraverse(tree.root);
        //tree.postOrderTraverse(tree.root);
    }

}
```