# 📌 Disjoint Set Union (DSU / Union-Find)

Disjoint Set Union (also known as Union-Find) is a data structure that tracks a partition of a set into disjoint subsets, supporting near-constant time union and find operations.

---

## 📖 Overview & Core Concepts

DSU manages dynamic connectivity: checking whether two elements belong to the same component and merging two components.

### Crucial Optimizations:
1. **Path Compression**: During `find(i)`, make every visited node point directly to the root:
   ```cpp
   parent[i] = find(parent[i]);
   ```
2. **Union by Rank / Size**: Attach the shorter tree beneath the root of the taller tree to keep tree height bounded.

### Ackerman Complexity:
Combining Path Compression and Union by Rank reduces the amortized time per operation to:
$$O(\alpha(N))$$
Where $\alpha(N)$ is the **Inverse Ackermann function**, which is $< 5$ for any conceivable universe size ($N \le 10^{80}$). For all practical intents, operations take **amortized $O(1)$** time.


---

## ⚡ Time & Space Complexity

| Operation | Naive Array | Path Compression Alone | Path Compression + Union by Rank |
|---|---|---|---|
| Find | $O(N)$ | $O(\log N)$ | $O(\alpha(N)) \approx O(1)$ |
| Union | $O(N)$ | $O(\log N)$ | $O(\alpha(N)) \approx O(1)$ |
| Space Complexity | $O(N)$ | $O(N)$ | $O(N)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### Production C++ DSU Implementation Template
```cpp
class DSU {
    vector<int> parent;
    vector<int> size;
    int numComponents;
public:
    DSU(int n) : parent(n), size(n, 1), numComponents(n) {
        iota(parent.begin(), parent.end(), 0);
    }

    int find(int i) {
        if (parent[i] == i) return i;
        return parent[i] = find(parent[i]); // Path compression
    }

    bool unite(int i, int j) {
        int rootI = find(i);
        int rootJ = find(j);
        if (rootI == rootJ) return false; // Already in same component

        // Union by size
        if (size[rootI] < size[rootJ]) swap(rootI, rootJ);
        parent[rootJ] = rootI;
        size[rootI] += size[rootJ];
        numComponents--;
        return true;
    }

    bool connected(int i, int j) {
        return find(i) == find(j);
    }

    int getComponents() const {
        return numComponents;
    }
};
```

### Kruskal's Minimum Spanning Tree (MST)
```cpp
struct Edge {
    int u, v, weight;
    bool operator<(const Edge& other) const { return weight < other.weight; }
};

int kruskal(int n, vector<Edge>& edges) {
    sort(edges.begin(), edges.end());
    DSU dsu(n);
    int mstWeight = 0, edgesCount = 0;
    for (const auto& edge : edges) {
        if (dsu.unite(edge.u, edge.v)) {
            mstWeight += edge.weight;
            if (++edgesCount == n - 1) break;
        }
    }
    return (edgesCount == n - 1) ? mstWeight : -1;
}
```


---

## 🛠️ C++ STL & Implementations

C++ STL does not have a built-in DSU container, so you must implement it (keep this standard snippet ready).

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Number of Connected Components in an Undirected Graph | 🟡 Medium | LC 323 | Basic DSU Union & Count |
| Redundant Connection | 🟡 Medium | LC 684 | Cycle Detection with DSU |
| Accounts Merge | 🟡 Medium | LC 721 | Email Index DSU Mapping |
| Number of Provinces | 🟡 Medium | LC 547 | Adjacency Matrix DSU |
| Satisfiability of Equality Equations | 🟡 Medium | LC 990 | Bipartite DSU / Equivalence |
| Min Cost to Connect All Points | 🟡 Medium | LC 1584 | Kruskal's MST + DSU |
| Swim in Rising Water | 🔴 Hard | LC 778 | DSU Grid Percolation |


---

## ⚠️ Common Pitfalls & C++ Tips

- Always use both Path Compression AND Union by Rank/Size to guarantee $O(\alpha(N))$.
- Remember that DSU supports only adding connections (unions); standard DSU cannot delete connections dynamically without rollback logs.

