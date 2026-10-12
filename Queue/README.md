# 📌 Queue & Deque Data Structures

A queue is a linear collection operating under the First-In, First-Out (FIFO) principle. A double-ended queue (deque) allows insertions and deletions at both ends in $O(1)$.

---

## 📖 Overview & Core Concepts

Queues are essential for breadth-first traversals (BFS), task scheduling, buffering, and message pipelines.

### Variations:
1. **Standard Queue (FIFO)**: Enqueue at back, dequeue from front. Implemented via `std::queue`.
2. **Double-Ended Queue (Deque)**: Insertion/removal at both ends in $O(1)$. Implemented via `std::deque`.
3. **Circular Queue**: Fixed-size array where the end wraps around to the beginning using modulo arithmetic (`(tail + 1) % capacity`).
4. **Monotonic Deque**: Maintains monotonic elements to compute sliding window minimum/maximum in amortized $O(1)$ per step.


---

## ⚡ Time & Space Complexity

| Operation | `std::queue` | `std::deque` |
|---|---|---|
| Push Front | N/A | $O(1)$ |
| Push Back (`push`) | $O(1)$ | $O(1)$ |
| Pop Front (`pop`) | $O(1)$ | $O(1)$ |
| Pop Back | N/A | $O(1)$ |
| Random Access (`[i]`) | N/A | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. BFS Level-Order Traversal
```cpp
void bfs(TreeNode* root) {
    if (!root) return;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int levelSize = q.size();
        for (int i = 0; i < levelSize; ++i) {
            TreeNode* node = q.front();
            q.pop();
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
}
```

### 2. Monotonic Deque (Sliding Window Maximum)
```cpp
vector<int> maxSlidingWindow(const vector<int>& nums, int k) {
    deque<int> dq; // stores indices
    vector<int> res;
    for (int i = 0; i < (int)nums.size(); ++i) {
        if (!dq.empty() && dq.front() <= i - k) dq.pop_front();
        while (!dq.empty() && nums[dq.back()] <= nums[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) res.push_back(nums[dq.front()]);
    }
    return res;
}
```

### 3. Implementing Queue using Stacks
```cpp
class MyQueue {
    stack<int> in, out;
    void transfer() {
        if (out.empty()) {
            while (!in.empty()) {
                out.push(in.top());
                in.pop();
            }
        }
    }
public:
    void push(int x) { in.push(x); }
    int pop() { transfer(); int x = out.top(); out.pop(); return x; }
    int peek() { transfer(); return out.top(); }
    bool empty() { return in.empty() && out.empty(); }
};
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <queue>
#include <deque>

std::queue<int> q;
q.push(1);
q.pop();
int f = q.front();

std::deque<int> dq;
dq.push_front(10);
dq.push_back(20);
dq.pop_front();
dq.pop_back();
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Implement Queue using Stacks | 🟢 Easy | LC 232 | Two Stacks Amortized |
| Design Circular Queue | 🟡 Medium | LC 622 | Array + Modulo Indexing |
| Sliding Window Maximum | 🔴 Hard | LC 239 | Monotonic Deque |
| Shortest Subarray with Sum at Least K | 🔴 Hard | LC 862 | Monotonic Deque on Prefix Sums |
| Rotting Oranges | 🟡 Medium | LC 994 | Multi-source BFS Queue |
| Open the Lock | 🟡 Medium | LC 752 | BFS Shortest Path |


---

## ⚠️ Common Pitfalls & C++ Tips

- Always verify `!q.empty()` before accessing `q.front()` or `q.back()`.
- Capture `int sz = q.size()` at the start of each level iteration in BFS to process nodes level by level.

