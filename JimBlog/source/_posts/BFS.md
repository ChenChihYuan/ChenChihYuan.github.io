---
title: BFS
date: 2021-02-06 11:02:43
tags:
- Breath-first search
- BinarySearchTree
- Tree
- C++
- cpp
- Algorithm
---

# Breadth-first search
**Breath-first search**(BFS) is an algorithm for traversing or searching **tree** or **graph** data structures. It starts ar the **tree root**(or some arbitrary node of a graph), and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level. -[Wiki](https://en.wikipedia.org/wiki/Breadth-first_search#:~:text=Breadth%2Dfirst%20search%20(BFS),tree%20or%20graph%20data%20structures.&text=It%20uses%20the%20opposite%20strategy,backtrack%20and%20expand%20other%20nodes.)

# Queue Method 
## Pseudocode
---
Input:  A graph G and a starting vertex root of G
Output: Goal state. THe parent links trace the shortest path back to root.



```
procedure BFS(G, root) is
    let Q be a queue
    label root as discovered
    Q.enqueue(root)
    while Q is not empty do
        v := Q.dequeue()
        if c is the goal then
            return v
        for all edges from v to w in G.
```

The concept can refer to this article, though it is in Japanese.
[BFS_queue](https://qiita.com/drken/items/996d80bcae64649a6580)
[LeetCode_BFS](https://leetcode.com/explore/learn/card/data-structure-tree/134/traverse-a-tree/990/)

```cpp
vector<vector<int>> levelTravese(TreeNode* root){
        vector<vector<int>> LvTraverse;
        if(root == NULL)
            return LvTraverse;
        
        queue<TreeNode*> q;
        q.push(root);
        TreeNode* node = root;
        
        while(!q.empty()){
            int k = q.size();
            vector<int> tmp;
            while(k--){
                root = q.front();
                tmp.push_back(root->val);
                q.pop();
                
                if(root->left)
                    q.push(root->left);
                if(root->right)
                    q.push(root->right);
            }
            LvTraverse.push_back(tmp);
            
        }
        return LvTraverse;
        
    }
```

# Recursive Method

```cpp
int height(TreeNode* node){
        if(node == NULL)
            return 0;
        else{
            int lheight = height(node->left);
            int rheight = height(node->right);
            return lheight > rheight ? lheight+1 : rheight+1;
        }
    }
    
    
    
    vector<vector<int>> printlevelOrder(TreeNode* root){
        vector<vector<int>> vecs;
        for(int i = 1; i<= height(root);i++){
            vector<int> invec;
            AddGivenLevel(root, i, invec);
            vecs.push_back(invec);
        }
        return vecs;
    }
    
    
    void AddGivenLevel(TreeNode* root, int level, vector<int>& vec){
        if(root==NULL)
            return;
        if(level ==1)
            vec.push_back(root->val);
        else if(level>1){
            AddGivenLevel(root->left, level-1, vec);
            AddGivenLevel(root->right, level-1, vec);
        }
    }
```

Note:
1. height can be found recursively. height(1_based) = level(0_based) + 1


# Solve Tree Problems Recursively

Can refer to the LeetCode Page[LINK](https://leetcode.com/explore/learn/card/data-structure-tree/17/solve-problems-recursively/534/)

Typically, we can solve a tree problem recursively using a **top-down approach** or using a **bottom-up approach**.

## Top-down Solution

**Top-down** means that in each recursive call, we will **visit the node first** to come up with some values, and **pass** these values **to its children** when calling the function recursively.

So the top-down solution can be consideredd as a kind of **preorder** travesal. To be specific, the recursive function `top-down(root, params)` works like this:

1. return specific value for null node
2. update the answer if needed                              // answer <--- params
3. left_ans = top_down(root.left, left_params)              // left_params <--- root.val ,params
4. right_ans = top_down(root.right, right_params)           // right_params <--- root.val , params
5. return the answer if needed                              // answers <--- left_ans, right_ans

### For example - find height of a binary Tree

> We know that depth of the root node is `1`

1. For each node, if we know its depth, we will know the depth of its children.
2. Therefore, if we pass the depth of the node as a parameter when calling the function recursively, all the nodes will know their depth. 


---
1. return if root is `NULL`
2. if root is a leaf node:
3.     answer = max(answer, depth)      // update the answer if needed
4. maximum_depth(root.left, depth + 1)
5. maximum_depth(root.right, depth+ 1)
   
---
```cpp
int answer;		       // don't forget to initialize answer before call maximum_depth
void maximum_depth(TreeNode* root, int depth) {
    if (!root) {
        return;
    }
    if (!root->left && !root->right) {
        answer = max(answer, depth);
    }
    maximum_depth(root->left, depth + 1);
    maximum_depth(root->right, depth + 1);
}
```

---

## Button-up Solution
`Button-up` is another recursive solution. In each recursive call, we will firstly call the function recursively for **all the children nodes** and then come up with the answer according to the returned valued and the value of the current node itself. This process can be regraded as a kind of **postordered** traversal. Typically, a "button-up" recursive function `bottom_up(root)` will be something like this:

1. return specific value for null node
2. left_ans = bottom_up(root.left)          // call function recursively for left child
3. right_ans = bottom_up(root.right)        // call function recursively for right child
4. return answers   // answer <-- left_ans, right_ans, root.val
   
---
### For example - find height of a binary Tree
> Same exmaple, different perspective.

1. If we know the maximum depth `l` of the subtree rooted at its **left** child and the maximum depth `r` of the subtree rooted at its **right** child, can we answer the previous question? 
2. Of course yes, we can choose the maximum between them and add 1 to get the maximum depth of the subtree rooted at the current node. That is `x = max(l,r) + 1`

1. return 0 if root is null
2. left_depth = maximum_depth(root.left)
3. right_depth = maximum_depth(root.right)
4. return max(left_depth, right_depth) + 1

```cpp
int maximum_depth(TreeNode* root) {
	if (!root) {
		return 0;                                 // return 0 for null node
	}
	int left_depth = maximum_depth(root->left);
	int right_depth = maximum_depth(root->right);
	return max(left_depth, right_depth) + 1;	  // return depth of the subtree rooted at root
}
```