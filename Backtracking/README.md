# 📌 Backtracking

Backtracking is an algorithmic paradigm that systematically searches the entire solution space by building candidates incrementally and abandoning ('backtracking') a candidate as soon as it cannot lead to a valid solution.

---

## 📖 Overview & Core Concepts

Backtracking can be visualized as a Depth-First Search (DFS) on an implicit decision tree.

### Core Three-Step Blueprint:
1. **Choose**: Make a decision, advance state, mark choice as used.
2. **Explore**: Recurse down the decision tree.
3. **Un-choose (Backtrack)**: Revert state and unmark choice before trying alternatives.

### Pruning:
Bounding functions eliminate branches that violate constraints early, dramatically reducing search time from brute-force factorial/exponential bounds.


---

## ⚡ Time & Space Complexity

| Problem Class | Typical Time Complexity | Space Complexity |
|---|---|---|
| Subsets / Power Set | $O(2^N)$ | $O(N)$ recursion depth |
| Permutations | $O(N!)$ | $O(N)$ |
| N-Queens | $O(N!)$ with pruning | $O(N)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Subsets Template (Include / Exclude)
```cpp
void generateSubsets(int idx, vector<int>& nums, vector<int>& curr, vector<vector<int>>& res) {
    res.push_back(curr);
    for (int i = idx; i < (int)nums.size(); ++i) {
        curr.push_back(nums[i]);            // Choose
        generateSubsets(i + 1, nums, curr, res); // Explore
        curr.pop_back();                     // Un-choose (Backtrack)
    }
}
```

### 2. Permutations Template
```cpp
void permuteHelper(vector<int>& nums, vector<bool>& used, vector<int>& curr, vector<vector<int>>& res) {
    if (curr.size() == nums.size()) {
        res.push_back(curr);
        return;
    }
    for (size_t i = 0; i < nums.size(); ++i) {
        if (used[i]) continue;
        used[i] = true;
        curr.push_back(nums[i]);
        permuteHelper(nums, used, curr, res);
        curr.pop_back();
        used[i] = false;
    }
}
```

### 3. N-Queens (Pruning with Bitsets/Hash Sets)
```cpp
bool isSafe(int row, int col, int n, const vector<string>& board);
void solveNQueens(int row, int n, vector<string>& board, vector<vector<string>>& res);
```


---

## 🛠️ C++ STL & Implementations

Standard STL utilities for permutations:
```cpp
#include <algorithm>
#include <vector>

std::vector<int> nums = {1, 2, 3};
// std::next_permutation generates permutations lexicographically
do {
    // process nums
} while (std::next_permutation(nums.begin(), nums.end()));
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Subsets | 🟡 Medium | LC 78 | Power Set Backtracking |
| Subsets II | 🟡 Medium | LC 90 | Sort + Duplicate Pruning |
| Combinations | 🟡 Medium | LC 77 | Bounded Depth Exploration |
| Combination Sum | 🟡 Medium | LC 39 | Reusable Elements Backtracking |
| Permutations | 🟡 Medium | LC 46 | Visited Array / Swap |
| Word Search | 🟡 Medium | LC 79 | Grid DFS with In-Place Visited |
| Letter Combinations of a Phone Number | 🟡 Medium | LC 17 | Multi-branch Recursion |
| N-Queens | 🔴 Hard | LC 51 | Diagonal & Column Constraint Pruning |
| Sudoku Solver | 🔴 Hard | LC 37 | Constraint Satisfaction Search |


---

## ⚠️ Common Pitfalls & C++ Tips

- Sort arrays upfront whenever handling duplicate elements to prune branches easily (`if (i > idx && nums[i] == nums[i-1]) continue;`).
- Always revert state modifications in the exact reverse order of making them.

