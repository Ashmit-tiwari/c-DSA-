# 📌 Dynamic Programming (DP)

Dynamic Programming is a method for solving complex optimization and counting problems by breaking them down into overlapping subproblems and optimal substructures, caching subproblem solutions to prevent recomputation.

---

## 📖 Overview & Core Concepts

Dynamic Programming transforms exponential $O(2^N)$ recursion into polynomial time $O(N)$ or $O(N^2)$ by caching previously computed states.

### Approaches:
1. **Top-Down with Memoization**: Write recursive relations, cache state in a table or hash map (`memo`).
2. **Bottom-Up with Tabulation**: Identify topological dependency order and fill an iterative DP table from base cases up.
3. **Space Optimization**: If state at step $i$ depends only on $i-1$ or $i-2$, reduce space from $O(N)$ to $O(1)$.

### Classic DP Categories:
- 1D DP (Fibonacci, Climbing Stairs, House Robber)
- 2D / Grid DP (Unique Paths, Minimum Path Sum)
- Knapsack Variants (0/1 Knapsack, Unbounded Knapsack, Subset Sum)
- String DP (Longest Common Subsequence, Edit Distance)
- Longest Increasing Subsequence (LIS)
- Matrix Chain Multiplication / Interval DP
- Bitmask DP


---

## ⚡ Time & Space Complexity

| Problem | Time Complexity | Tabulation Space | Optimized Space |
|---|---|---|---|
| Climbing Stairs | $O(N)$ | $O(N)$ | $O(1)$ |
| 0/1 Knapsack | $O(N \times W)$ | $O(N \times W)$ | $O(W)$ |
| Longest Common Subsequence | $O(N \times M)$ | $O(N \times M)$ | $O(\min(N, M))$ |
| Edit Distance | $O(N \times M)$ | $O(N \times M)$ | $O(M)$ |
| LIS (Patience Sorting) | $O(N \log N)$ | $O(N)$ | $O(N)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. 0/1 Knapsack (Space-Optimized 1D Array)
Traverse capacities **backwards** to use values from the previous item:
```cpp
int knapSack(int W, const vector<int>& wt, const vector<int>& val) {
    int n = wt.size();
    vector<int> dp(W + 1, 0);
    for (int i = 0; i < n; ++i) {
        for (int w = W; w >= wt[i]; --w) {
            dp[w] = max(dp[w], val[i] + dp[w - wt[i]]);
        }
    }
    return dp[W];
}
```

### 2. Longest Common Subsequence (LCS)
$$\text{dp}[i][j] = \begin{cases} 1 + \text{dp}[i-1][j-1] & s_1[i-1] == s_2[j-1] \\ \max(\text{dp}[i-1][j], \text{dp}[i][j-1]) & \text{otherwise} \end{cases}$$
```cpp
int longestCommonSubsequence(string text1, string text2) {
    int m = text1.size(), n = text2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (text1[i - 1] == text2[j - 1]) dp[i][j] = 1 + dp[i - 1][j - 1];
            else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    return dp[m][n];
}
```

### 3. Longest Increasing Subsequence ($O(N \log N)$ Binary Search)
```cpp
int lengthOfLIS(const vector<int>& nums) {
    vector<int> tails;
    for (int x : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}
```


---

## 🛠️ C++ STL & Implementations

Standard arrays `std::vector<int>` or `std::vector<vector<int>>` are standard DP tables in C++.

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Climbing Stairs | 🟢 Easy | LC 70 | 1D Fibonacci DP |
| House Robber | 🟡 Medium | LC 198 | Choice DP ($O(1)$ space) |
| Coin Change | 🟡 Medium | LC 322 | Unbounded Knapsack |
| Longest Increasing Subsequence | 🟡 Medium | LC 300 | DP / Binary Search |
| Longest Common Subsequence | 🟡 Medium | LC 1143 | 2D Grid Alignment |
| Word Break | 🟡 Medium | LC 139 | 1D Substring DP |
| Unique Paths | 🟡 Medium | LC 62 | Grid Combinatorics / DP |
| Edit Distance | 🟡 Medium | LC 72 | 2D Insert/Delete/Replace |
| Trapping Rain Water | 🔴 Hard | LC 42 | DP Prefix & Suffix |
| Burst Balloons | 🔴 Hard | LC 312 | Interval DP / MCM |


---

## ⚠️ Common Pitfalls & C++ Tips

- Clearly define the state: What does `dp[i]` or `dp[i][j]` represent in plain English?
- When optimizing knapsack to 1D: Iterate backwards for 0/1 Knapsack (each item used at most once), iterate forward for Unbounded Knapsack (items can be reused).

