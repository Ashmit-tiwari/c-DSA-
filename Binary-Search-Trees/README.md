# 📌 Binary Search Trees (BST)

A Binary Search Tree is a binary tree where for every node, all values in its left subtree are strictly less than the node's value, and all values in its right subtree are strictly greater.

---

## 📖 Overview & Core Concepts

The BST invariant guarantees that an **Inorder Traversal visits keys in strictly sorted ascending order**.

### Properties:
- Inorder successor: Smallest node in the right subtree.
- Inorder predecessor: Largest node in the left subtree.
- Search, insertion, and deletion run in $O(H)$ time, where $H$ is tree height.
- On balanced BSTs (AVL, Red-Black Trees), $H = O(\log N)$.
- C++ `std::set` and `std::map` are implemented as self-balancing Red-Black Trees.


---

## ⚡ Time & Space Complexity

| Operation | Balanced BST | Skewed BST (Worst Case) |
|---|---|---|
| Search | $O(\log N)$ | $O(N)$ |
| Insertion | $O(\log N)$ | $O(N)$ |
| Deletion | $O(\log N)$ | $O(N)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Validate BST (Range Boundaries)
```cpp
bool isValidBST(TreeNode* root, long long minVal = LLONG_MIN, long long maxVal = LLONG_MAX) {
    if (!root) return true;
    if (root->val <= minVal || root->val >= maxVal) return false;
    return isValidBST(root->left, minVal, root->val) &&
           isValidBST(root->right, root->val, maxVal);
}
```

### 2. Search & Insert in BST
```cpp
TreeNode* searchBST(TreeNode* root, int val) {
    while (root && root->val != val) {
        root = (val < root->val) ? root->left : root->right;
    }
    return root;
}
```

### 3. Deletion in BST
```cpp
TreeNode* deleteNode(TreeNode* root, int key) {
    if (!root) return nullptr;
    if (key < root->val) root->left = deleteNode(root->left, key);
    else if (key > root->val) root->right = deleteNode(root->right, key);
    else {
        if (!root->left) return root->right;
        if (!root->right) return root->left;
        // Node with two children: replace with inorder successor
        TreeNode* succ = root->right;
        while (succ->left) succ = succ->left;
        root->val = succ->val;
        root->right = deleteNode(root->right, succ->val);
    }
    return root;
}
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <set>
#include <map>

// Self-balancing Red-Black Trees in STL
std::set<int> bst;
bst.insert(20);
bst.insert(10);
auto it = bst.lower_bound(15); // O(log N) - iterator to 20
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Search in a Binary Search Tree | 🟢 Easy | LC 700 | Recursive / Iterative Branching |
| Validate Binary Search Tree | 🟡 Medium | LC 98 | Valid Boundary Range |
| Kth Smallest Element in a BST | 🟡 Medium | LC 230 | Inorder Traversal Counter |
| Lowest Common Ancestor of a BST | 🟡 Medium | LC 235 | Value Comparison Split |
| Delete Node in a BST | 🟡 Medium | LC 450 | Successor Replacement |
| Convert Sorted Array to Binary Search Tree | 🟢 Easy | LC 108 | Divide & Conquer Midpoint |


---

## ⚠️ Common Pitfalls & C++ Tips

- When validating a BST, beware of 32-bit `INT_MIN` and `INT_MAX` boundary edge cases; use `long long` bounds.
- Remember: Checking only `node->left->val < node->val` is insufficient; the entire left subtree must be smaller than the node.

