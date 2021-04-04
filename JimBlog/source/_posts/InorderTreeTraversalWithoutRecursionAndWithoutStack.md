---
title: InorderTreeTraversalWithoutRecursionAndWithoutStack
date: 2021-02-09 22:38:46
tags:
- BST
- BinarySearchTree
- Inorder
- Traversal
- Morris Traversal
- Threaded Binary Tree
---

[Reference_GeeksforGeeks](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion-and-without-stack/?ref=rp)
[Thread Binary Tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)

# Inorder Tree Traversal without recursion and without stack
Using `Morris Travesal`, we can traverse the tree without using stack and recursion. The idea of Morris Traversal is based on `Threaded Binary Tree`.

In this traveral, we first create links to Inorder successor and print the data using these links, and finally revert the changes to restore original tree.

1. Initalize current as root
2. While current **is not NULL**
    **If** the current does not have left child
        1. Print current's data
        2. Go to the right, i.e., current = current->right
    **Else**
        Find rightmost node in current left subtree `OR` node whose right child == current
        *If*
            we found right child == current
            Go to the right, i.e. current = current->right
        *Else*
            1. Make current as the right child of that rightmost node we found; and
            2. Go to this left child, i.e., current = current -> current->left


Although the tree **is modified through the traversal**, it is reverted back to its original shape after the completion. *No extra space is required*

```cpp
struct tNode{
    int data;
    struct tNode* left;
    struct tNode* right;
};

void MorrisTraversal(struct tNode* root){
    struct tNode* current;
    struct tNode* pre;

    if(root == NULL)
        return;
    
    current = root;
    while(current != NULL){
        if(current->left == NULL){
            printf("%d", current->data);
            current = current->right;
        }
        else{
            // Find the inorder predecessor of current
            pre = current->left;
            while(pre->right != NULL && pre->right != current)
                pre = pre->right;

            // Make current as the right child of its inorder predecessor
            if(pre->right == NULL){
                pre->right = current;
                current = current->left;
            }

            // Revert the changes made in the `if` part to restore the original tree i.e., fix the right child of predecessor
            else{
                pre->right = NULL;
                printf("%d", current->data);
                current = current->right;
            }
        }
    }

}


struct tNode* newtNode(int data)
{
    struct tNode* node = new tNode;
    node->data = data;
    node->left = NULL;
    node->right = NULL;
 
    return (node);
}

int main()
{
 
    /* Constructed binary tree is
            1
          /   \
         2     3
       /   \
      4     5
  */
    struct tNode* root = newtNode(1);
    root->left = newtNode(2);
    root->right = newtNode(3);
    root->left->left = newtNode(4);
    root->left->right = newtNode(5);
 
    MorrisTraversal(root);
 
    return 0;
}

```