---
title: BinarySearchTree_Basic
date: 2021-02-03 22:06:11
tags:
- Algorthm
- BinarySearchTree
- BinaryTree
- Cpp
---
This post is for the notes for me re-learning 'binary search', somehow I feel like I have been learned this algorithm couple times. But shame to say, I still do not feel I totally get the idea behind, as for how to implement this. I hope I can spend some quailty time for once again learn and master binary Tree.

The note here, and as code are from the GeeksforGeeks[Link](https://www.geeksforgeeks.org/binary-search-tree-data-structure/#basic).

# Binary Search Tree
**Binary Search Tree ** is a 'node-based' binary tree data structure which has the following properties.
- The left subtree of a node contains only nodes with keys lesser than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- The left and right subtree each must also be a binary search tree.


## Binary Search Tree| Set1(Search and Insertion)

Operations like **search**, **minimum** and **maximum** can be done fast.

### Searching a key
If we had a **sorted array**  we could performed a binary search.

```cpp
#include <iostream>
using namespace std;

class BST{
        int data;
        BST *left, *right;
    public:
        BST();
        BST(int);
        BST* Insert(BST*, int);
        void Inorder(BST*); // Inorder traversal.
};

// default constructor definition
BST:: BST() : data(0), left(NULL),right(NULL){

}
// Parameterized constructor definition
BST::BST(int value){
    data = value;
    left = right = NULL;
}

// Insert function definition.
BST* BST :: Insert(BST* root, int value){
    if(!root){
        // insert the first node, if root is NULL
        return new BST(value);
    }
    // Insert data
    if(value > root->data){
        root->right = Insert(root->right,value);
    }
    else{
        root->left = Insert(root->left,value);
    }
    return root;
}
// Inorder traversal function
void BST::Inorder(BST* root){
    if(!root){
        return;
    }
    Inorder(root->left);
    cout<<root->data<<endl;
    Inorder(root->right);
}

// Driver code 
int main() 
{ 
    BST b, *root = NULL; 
    root = b.Insert(root, 50); 
    b.Insert(root, 30); 
    b.Insert(root, 20); 
    b.Insert(root, 40); 
    b.Insert(root, 70); 
    b.Insert(root, 60); 
    b.Insert(root, 80); 
  
    b.Inorder(root); 
    return 0; 
} 
```

```cs
using System;
class BinarySearchTree{
    public class Node{
        public int key;
        public Node left, right;
        public Node(int item){
            key = item;
            left = right = NULL;
        }
    }

Node root;
BinarySearchTree(){
    root = null;
}

void insert(int key){
    root = insertRec(root, key);
}

Node insertRec(Node root, int key){
    if(root == null){
        root = new Node(key);
        return root;
    }

    if(key < root.key)
        root.left = insertRec(root.left, key);
    else if( key> root.key)
        root.right = insertRec(root.right, key);

    //return the (unchanged) node pointer
    return root;
}

void inorder(){
    inorderRec(root);
}

void inorderRec(Node root){
    if(root != null){
        inorderRec(root.left);
        Console.WriteLine(root.key);
        inorderRec(root.right);
    }
}


public static void Main(String[] args) 
{ 
    BinarySearchTree tree = new BinarySearchTree(); 
  
    /* Let us create following BST 
          50 
       /     \ 
      30      70 
     /  \    /  \ 
   20   40  60   80 */
    tree.insert(50); 
    tree.insert(30); 
    tree.insert(20); 
    tree.insert(40); 
    tree.insert(70); 
    tree.insert(60); 
    tree.insert(80); 
      
    // Print inorder traversal of the BST 
    tree.inorder(); 
} 

}



```
