# 📌 Heap & Priority Queue

A Heap is a complete binary tree satisfying the heap property: in a max-heap, the parent key is $\ge$ its children; in a min-heap, the parent key is $\le$ its children.

---

## 📖 Overview & Core Concepts

Heaps provide efficient retrieval of the extremum (minimum or maximum) in $O(1)$ time, with $O(\log N)$ insertion and deletion.

### Array Representation:
For a 0-indexed complete binary tree stored in an array:
- Parent of node $i$: $\lfloor(i - 1) / 2\rfloor$
- Left child of node $i$: $2i + 1$
- Right child of node $i$: $2i + 2$

### Core Algorithms:
- **Sift-Up (Heapify-Up)**: Restore heap property after inserting at the end ($O(\log N)$).
- **Sift-Down (Heapify-Down)**: Restore heap property after removing root ($O(\log N)$).
- **Build Heap**: Bottom-up construction from an unordered array runs in **$O(N)$** time (not $O(N \log N)$).


---

## ⚡ Time & Space Complexity

| Operation | Time Complexity |
|---|---|
| Find Min/Max (`top`) | $O(1)$ |
| Insert (`push`) | $O(\log N)$ |
| Remove Min/Max (`pop`) | $O(\log N)$ |
| Build Heap (`make_heap`) | $O(N)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Top-K Elements Pattern
- To find **K Largest Elements**: Use a **Min-Heap** of size $K$. When size exceeds $K$, pop the minimum. Elements remaining in the heap are the $K$ largest ($O(N \log K)$).
- To find **K Smallest Elements**: Use a **Max-Heap** of size $K$.

```cpp
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    for (int x : nums) {
        minHeap.push(x);
        if ((int)minHeap.size() > k) minHeap.pop();
    }
    return minHeap.top();
}
```

### 2. Two Heaps Pattern (Find Median from Data Stream)
Maintains two balanced heaps:
- `maxHeap` stores smaller half.
- `minHeap` stores larger half.
```cpp
class MedianFinder {
    priority_queue<int> maxHeap; // smaller half
    priority_queue<int, vector<int>, greater<int>> minHeap; // larger half
public:
    void addNum(int num) {
        maxHeap.push(num);
        minHeap.push(maxHeap.top());
        maxHeap.pop();
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.push(minHeap.top());
            minHeap.pop();
        }
    }
    double findMedian() {
        if (maxHeap.size() > minHeap.size()) return maxHeap.top();
        return (maxHeap.top() + minHeap.top()) / 2.0;
    }
};
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <queue>
#include <vector>

// Default: Max-Heap
std::priority_queue<int> maxH;

// Min-Heap
std::priority_queue<int, std::vector<int>, std::greater<int>> minH;

// Custom Comparator
auto comp = [](const pair<int, int>& a, const pair<int, int>& b) {
    return a.first > b.first; // min-heap by first element
};
std::priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(comp)> customPQ(comp);
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Kth Largest Element in an Array | 🟡 Medium | LC 215 | Min-Heap of Size $K$ |
| Top K Frequent Elements | 🟡 Medium | LC 347 | Hash Map + Min-Heap / Bucket Sort |
| Merge k Sorted Lists | 🔴 Hard | LC 23 | Min-Heap with Node Pointers |
| Find Median from Data Stream | 🔴 Hard | LC 295 | Dual Balanced Heaps |
| Task Scheduler | 🟡 Medium | LC 621 | Max-Heap Frequency Greedy |
| Reorganize String | 🟡 Medium | LC 767 | Max-Heap Adjacent Separation |


---

## ⚠️ Common Pitfalls & C++ Tips

- Remember: Default `std::priority_queue<int>` is a **Max-Heap**, not a Min-Heap.
- Building a heap using `std::priority_queue(vec.begin(), vec.end())` takes $O(N)$ time, whereas pushing elements one by one takes $O(N \log N)$.

