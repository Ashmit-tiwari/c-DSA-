# 📌 Advanced Data Structures

Advanced data structures solve dynamic range query and update problems in logarithmic time, powering high-performance systems and competitive programming.

---

## 📖 Overview & Core Concepts

When arrays undergo frequent dynamic updates and range queries (range minimum, range sum, range GCD), ordinary arrays take $O(N)$ for queries or updates. Advanced data structures optimize both to $O(\log N)$.

### 1. Segment Tree
A complete binary tree where each node represents an interval $[L, R]$.
- Point Update: $O(\log N)$
- Range Query: $O(\log N)$
- Range Update with **Lazy Propagation**: $O(\log N)$
- Space: $4N$ nodes.

### 2. Fenwick Tree (Binary Indexed Tree / BIT)
An array-based data structure where index $i$ is responsible for elements in $(i - \text{LSB}(i), i]$.
- Simpler to implement than Segment Tree.
- Lower constant factor.
- Space: Exactly $N + 1$.
- Range sum queries and point updates in $O(\log N)$.

### 3. Sparse Table
Static range query structure (no updates) computing Range Minimum Query (RMQ) in $O(1)$ time after $O(N \log N)$ preprocessing by precomputing $2^k$ jumps.


---

## ⚡ Time & Space Complexity

| Structure | Construction | Range Query | Point Update | Range Update | Space |
|---|---|---|---|---|---|
| Prefix Sum Array | $O(N)$ | $O(1)$ | $O(N)$ | $O(N)$ | $O(N)$ |
| Fenwick Tree (BIT) | $O(N)$ | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ (with diff) | $O(N)$ |
| Segment Tree | $O(N)$ | $O(\log N)$ | $O(\log N)$ | $O(\log N)$ (Lazy) | $O(4N)$ |
| Sparse Table | $O(N \log N)$ | $O(1)$ (Idempotent) | N/A | N/A | $O(N \log N)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Fenwick Tree (Binary Indexed Tree) Template
```cpp
class FenwickTree {
    vector<int> tree;
    int n;
public:
    FenwickTree(int n) : n(n), tree(n + 1, 0) {}

    void add(int i, int delta) { // 1-indexed
        for (; i <= n; i += i & -i) tree[i] += delta;
    }

    int query(int i) { // Prefix sum [1..i]
        int sum = 0;
        for (; i > 0; i -= i & -i) sum += tree[i];
        return sum;
    }

    int rangeQuery(int l, int r) {
        return query(r) - query(l - 1);
    }
};
```

### 2. Segment Tree (Point Update, Range Sum Query)
```cpp
class SegmentTree {
    int n;
    vector<int> tree;

    void build(const vector<int>& arr, int node, int start, int end) {
        if (start == end) { tree[node] = arr[start]; return; }
        int mid = start + (end - start) / 2;
        build(arr, 2 * node, start, mid);
        build(arr, 2 * node + 1, mid + 1, end);
        tree[node] = tree[2 * node] + tree[2 * node + 1];
    }
public:
    SegmentTree(const vector<int>& arr) : n(arr.size()), tree(4 * arr.size()) {
        if (n > 0) build(arr, 1, 0, n - 1);
    }

    int query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return 0;
        if (l <= start && end <= r) return tree[node];
        int mid = start + (end - start) / 2;
        return query(2 * node, start, mid, l, r) + query(2 * node + 1, mid + 1, end, l, r);
    }

    void update(int node, int start, int end, int idx, int val) {
        if (start == end) { tree[node] = val; return; }
        int mid = start + (end - start) / 2;
        if (idx <= mid) update(2 * node, start, mid, idx, val);
        else update(2 * node + 1, mid + 1, end, idx, val);
        tree[node] = tree[2 * node] + tree[2 * node + 1];
    }
};
```


---

## 🛠️ C++ STL & Implementations

Standard C++ does not provide Segment Tree or Fenwick Tree; keep robust templates ready.

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Range Sum Query - Mutable | 🟡 Medium | LC 307 | Segment Tree / BIT Point Update |
| Count of Smaller Numbers After Self | 🔴 Hard | LC 315 | BIT Coordinate Compression |
| Range Minimum Query | 🔴 Hard | Classic RMQ | Sparse Table / Segment Tree |
| Falling Squares | 🔴 Hard | LC 699 | Coordinate Compression + Segment Tree |


---

## ⚠️ Common Pitfalls & C++ Tips

- For Fenwick Tree, indices must be **1-indexed** because `0 & -0 = 0`, causing an infinite loop.
- Size of Segment Tree array must be allocated at least `4 * N` to prevent out-of-bounds on recursive child lookups.

