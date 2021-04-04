---
title: InorderTreeTraverseWithoutRecursion
date: 2021-02-09 21:57:46
tags:
- BFS
- BinaryTree
- Inorder
- Traverse
- Stack
- Recursion
- c++
- cpp
---

# Inorder Tree Traversal without Recursion

[Reference](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion/?ref=lbp)

**Time Complexity:** O(n2)

Using `Stack` is the obvious way to travese tree without recursion.

1. create an empty stack S 
2. Initialize current node as root 
3. Push the current node to S and set current = current->left until current is **NULL**
4. If current is **NULL** and stack is **not empty** then 
    1. Pop the top item from stack
    2. Print the popped item, set current = popped_item->right
    3. Go to step 3.
5. If current is **NULL** and stack is **empty**
   


```cpp
#include<bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    struct Node* left;
    struct Node* right;
    Node(int data){
        this->data = data;
        left = right = null;
    }
};

void inOrder(struct Node* root){
    stack<Node*> s;
    Node* curr = root;
    while(curr!= NULL || s.empty()==false){
        while(curr!= NULL){
            s.push(curr);
            curr = curr->left;
        }
        curr = s.top();
        s.pop();
        cout<<curr->data<<" ";
        curr =  curr->right;
    }
}

int main(){
    struct Node* root = new Node(1);
    root->left = new Node(2);
    root->right = new Node(3);
    root->left->left = new Node(4);
    root->left->right = new Node(5);

    inOrder(root);
    return 0;
}

```
