# 📌 Trees & Binary Trees

A tree is a hierarchical, non-linear data structure consisting of nodes connected by edges, with a designated root node and no cycles.

---

## 📖 Overview & Core Concepts

A binary tree is a tree data structure in which each node has at most two children: the left child and the right child.

### Fundamental Terminology:
- **Depth**: Number of edges from root to the node.
- **Height**: Number of edges from the node to the deepest leaf.
- **Full Binary Tree**: Every node has 0 or 2 children.
- **Complete Binary Tree**: All levels filled except possibly the last, which is filled from left to right.
- **Perfect Binary Tree**: All interior nodes have 2 children, all leaves at same depth ($2^{h+1}-1$ nodes).

### Tree Node Definition:
```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};
```


---

## ⚡ Time & Space Complexity

| Traversal | Time Complexity | Auxiliary Space |
|---|---|---|
| Depth-First Search (DFS) | $O(N)$ | $O(H)$ recursion stack ($O(\log N)$ balanced, $O(N)$ skewed) |
| Breadth-First Search (BFS) | $O(N)$ | $O(W)$ max tree width ($O(N)$ worst case) |
| Morris Traversal (Threaded) | $O(N)$ | $O(1)$ auxiliary space |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Depth First Traversals (Preorder, Inorder, Postorder)
- **Preorder** (Root, Left, Right): Cloning trees, serialization.
- **Inorder** (Left, Root, Right): Returns sorted sequence in BST.
- **Postorder** (Left, Right, Root): Deleting trees, bottom-up calculations (height, diameter).

```cpp
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}
```

### 2. Lowest Common Ancestor (LCA)
```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    if (left && right) return root;
    return left ? left : right;
}
```

### 3. Tree Diameter ($O(N)$ Bottom-Up)
```cpp
int diameterOfBinaryTree(TreeNode* root) {
    int maxDia = 0;
    auto height = [&](auto& self, TreeNode* node) -> int {
        if (!node) return 0;
        int lh = self(self, node->left);
        int rh = self(self, node->right);
        maxDia = max(maxDia, lh + rh);
        return 1 + max(lh, rh);
    };
    height(height, root);
    return maxDia;
}
```


---

## 🛠️ C++ STL & Implementations

Binary trees do not have a dedicated STL container; they are represented via node pointers (`TreeNode*`) or flattened vectors for heaps.

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Maximum Depth of Binary Tree | 🟢 Easy | LC 104 | DFS / BFS Height |
| Invert Binary Tree | 🟢 Easy | LC 226 | Recursive Swap |
| Same Tree | 🟢 Easy | LC 100 | Dual Tree Recursion |
| Subtree of Another Tree | 🟢 Easy | LC 572 | Tree Matching |
| Binary Tree Level Order Traversal | 🟡 Medium | LC 102 | BFS Queue |
| Lowest Common Ancestor of a Binary Tree | 🟡 Medium | LC 236 | Postorder Return |
| Construct Binary Tree from Preorder & Inorder | 🟡 Medium | LC 105 | Divide & Conquer Index Mapping |
| Binary Tree Maximum Path Sum | 🔴 Hard | LC 124 | Bottom-Up Postorder Max Contribution |
| Serialize and Deserialize Binary Tree | 🔴 Hard | LC 297 | Preorder BFS String Parsing |


---

## ⚠️ Common Pitfalls & C++ Tips

- Think inductively: Assume subtrees have solved the problem; how do you combine their results at the current root?
- Beware of stack overflow on skewed trees ($H = N$); use iterative DFS with `std::stack` for extreme depths.

