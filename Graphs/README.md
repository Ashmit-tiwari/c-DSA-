# 📌 Graphs & Graph Algorithms

A graph $G = (V, E)$ consists of a set of vertices $V$ and edges $E$ that connect pairs of vertices, representing relationships and networks.

---

## 📖 Overview & Core Concepts

Graphs may be directed or undirected, weighted or unweighted, cyclic or acyclic (DAG).

### Representations:
1. **Adjacency List**: `vector<vector<int>> adj` or `vector<vector<pair<int, int>>> adj`. Space: $O(V + E)$. Ideal for sparse graphs.
2. **Adjacency Matrix**: `int adj[V][V]`. Space: $O(V^2)$. Ideal for dense graphs and fast $O(1)$ edge existence checks.

### Essential Algorithms:
- **BFS**: Shortest path in unweighted graphs ($O(V + E)$).
- **DFS**: Cycle detection, connected components, path finding ($O(V + E)$).
- **Topological Sort**: Kahn's BFS algorithm or DFS post-order on DAG ($O(V + E)$).
- **Dijkstra's Algorithm**: Single-source shortest path with non-negative weights ($O((V + E) \log V)$).
- **Bellman-Ford Algorithm**: Shortest path with negative weights & negative cycle detection ($O(V \cdot E)$).
- **Floyd-Warshall Algorithm**: All-pairs shortest path ($O(V^3)$).
- **Prim's & Kruskal's**: Minimum Spanning Tree (MST) ($O(E \log V)$).
- **Bridges & Articulation Points**: Tarjan's low-link values ($O(V + E)$).


---

## ⚡ Time & Space Complexity

| Algorithm | Time Complexity | Space Complexity | Use Case |
|---|---|---|---|
| BFS / DFS | $O(V + E)$ | $O(V)$ | Traversal, Connected Components |
| Kahn's Topological Sort | $O(V + E)$ | $O(V)$ | Dependency Resolution |
| Dijkstra | $O((V + E) \log V)$ | $O(V)$ | Shortest Path (Non-negative edges) |
| Bellman-Ford | $O(V \cdot E)$ | $O(V)$ | Shortest Path (Negative weights allowed) |
| Floyd-Warshall | $O(V^3)$ | $O(V^2)$ | All-Pairs Shortest Path |
| Kruskal's MST | $O(E \log E)$ | $O(V)$ | Minimum Spanning Tree |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. BFS Shortest Path (Unweighted)
```cpp
int shortestPath(int start, int target, int n, const vector<vector<int>>& adj) {
    vector<int> dist(n, -1);
    queue<int> q;
    dist[start] = 0;
    q.push(start);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        if (u == target) return dist[u];
        for (int v : adj[u]) {
            if (dist[v] == -1) {
                dist[v] = dist[u] + 1;
                q.push(v);
            }
        }
    }
    return -1;
}
```

### 2. Kahn's Algorithm for Topological Sort
```cpp
vector<int> topoSort(int n, const vector<vector<int>>& adj) {
    vector<int> inDegree(n, 0);
    for (int u = 0; u < n; ++u)
        for (int v : adj[u]) inDegree[v]++;
    queue<int> q;
    for (int i = 0; i < n; ++i)
        if (inDegree[i] == 0) q.push(i);
    vector<int> order;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        order.push_back(u);
        for (int v : adj[u])
            if (--inDegree[v] == 0) q.push(v);
    }
    return (order.size() == n) ? order : vector<int>{}; // Empty if cycle detected
}
```

### 3. Dijkstra's Algorithm
```cpp
vector<int> dijkstra(int src, int n, const vector<vector<pair<int, int>>>& adj) {
    vector<int> dist(n, INT_MAX);
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    dist[src] = 0;
    pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;
        for (auto& [v, weight] : adj[u]) {
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```


---

## 🛠️ C++ STL & Implementations

Adjacency lists are constructed using `std::vector<std::vector<int>>` and `std::priority_queue`.

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Number of Islands | 🟡 Medium | LC 200 | Grid DFS / BFS Flood Fill |
| Clone Graph | 🟡 Medium | LC 133 | Hash Map + DFS/BFS |
| Course Schedule | 🟡 Medium | LC 207 | Cycle Detection / Kahn's Topo Sort |
| Course Schedule II | 🟡 Medium | LC 210 | Topological Sort Ordering |
| Network Delay Time | 🟡 Medium | LC 743 | Dijkstra's Algorithm |
| Rotting Oranges | 🟡 Medium | LC 994 | Multi-Source BFS |
| Word Ladder | 🔴 Hard | LC 127 | Bidirectional BFS |
| Alien Dictionary | 🔴 Hard | LC 269 | Directed Graph Topological Sort |


---

## ⚠️ Common Pitfalls & C++ Tips

- Avoid visited cycle infinite loops by marking nodes as visited before pushing to BFS queue, not when popping.
- In Dijkstra, always verify `if (d > dist[u]) continue;` to discard outdated entries in the priority queue.

