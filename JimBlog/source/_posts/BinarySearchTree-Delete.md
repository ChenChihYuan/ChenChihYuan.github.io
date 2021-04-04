---
title: BinarySearchTree_Delete
date: 2021-02-04 00:59:56
tags:
- Algorithm
- BinaryTree
- BinarySearchTree
---

The original article:
[Link](https://www.geeksforgeeks.org/binary-search-tree-set-2-delete/)

When we delete a node, **three** possibilites arise.
1. Node to be deleted is leaf: Simply remove from the tree.
2. Node to be deleted has only one child: Copy the child to the node and delete the child.
3. Node to be deleted has two children: Find inorder successor of the node. Copy contents of the inorder successor to the node and delete the inorder successor. Note that inorder predecessor can also be used. 

The important thing to note is:
Inorder successor is needed only when right child is not empty.

Also, I learned from the visualization algorithm website[LINK](https://visualgo.net/en/bst).

```pseudo
search for v
if v is a leaf
    delete leaf v
else if v has 1 child
    bypass v
else
    replace v with successor

```

It is worth noting that when you delete nodes in a tree, deletion process will be in post-order. That is to say, when you delete a node, you will delete its left child and its right child before you delete the node itself.




```cpp
#include <bits/stdc++.h>
using namespace std;

struct node{
    int key;
    struct node *left, *right;
}

struct node* newNode(int item){
    struct node* temp = (struct node*)malloc(sizeof(struct node));
    temp->key = item;
    temp->left = temp->right = NULL;
    return temp;
}

void inorder(struct node* root){
    if(root != NULL){
        inorder(root->left);
        cout<<root->key;
        inorder(root->right);
    }
}

struct node* insert(struct node* node, int key){
    if(node == NULL)
        return newNode(key);
    if(key < node->key){
        node->left = insert(node->left, key);
    }
    else
        node->right = insert(node->right, key);

    // return the (unchanged) node pointer.
    return node;
}

struct node* minValueNode(struct node* node){
    struct node* current = node;

    while(current && current->left != NULL)
        current = current->left;

    return current;
}

struct node* deleteNode(struct node* root, int key){
    if(root == NULL)
        return root;

    if(key < root->key)
        root->left = deleteNode(root->left, key);
    else if(key > root->key)
        root->right = deleteNode(root->right, key);
    // if key is same as root's key, then this is the node
    // to be deleted
    else{ 
        // 1. node with only one child or no child
        if(root->left == NULL){
            struct node* temp = root->right;
            free(root);
            return temp;
        }
        else if(root->right == NULL){
            struct node* temp = root->left;
            free(root);
            return temp;
        }
        // 2. node with 2 children: Get the inorder successor(smallest in the right subtree)
        struct node* temp = minValueNode(root->right);
        root->key = temp->key;
        root->right = deleteNode(root->right, temp->key);
    }
    return root;
}


```

```cs
using System;
public class BinarySearchTree{
    class Node{
        public int key;
        public Node left, right;
        public Node(int item){
            key = item;
            left = right = null;
        }
    }
    // Root of BST
    Node root;
    BinarySearchTree(){root = null;}
    void deleteRec(Node root, int key){
        if(root == null)
            return root;
        if(key< root.key)
            root.left = deleteRec(root.left, key);
        else if(key > root.key)
            root.right = deleteRec(root.right,key);
        else{
            if(root.left == null)
                return root.right;
            else if(root.right == null)
                return root.left;

            root.key = minValue(root.right);
            root.right = deleteRec(root.right, root.key);
        }
        return root;
    }

    int minValue(Node root){
        int minv = root.key;
        while(root.left != null){
            minv = root.left.key;
            root = root.left;
        }
        return minv;
    }

    void insert(int key){root = insertRec(root, key);}

    Node insertRec(Node root, int key){
        if(root == NULL){
            root = new Node(key);
            return root;
        }
    }

    void inorder(){ inorderRec(root);}

    void inorderRec(Node root){
        if(root!=null){
            inorderRec(root.left);
            Console.Write(root.key + " ");
            inorderRec(root.right);
        }
    }
}
```