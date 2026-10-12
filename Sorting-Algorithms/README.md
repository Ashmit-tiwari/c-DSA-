# 📌 Sorting Algorithms

Sorting arranges elements of a list in a specified order (numerical or lexicographical). Mastering sorting provides insights into algorithmic complexity, recursion, and stability.

---

## 📖 Overview & Core Concepts

Sorting algorithms fall into two broad categories:
1. **Comparison-Based Sorts**: Compare elements pairwise. The theoretical lower bound for comparison sort is $\Omega(N \log N)$.
   - Merge Sort, Quick Sort, Heap Sort, Insertion Sort, Bubble Sort, Selection Sort.
2. **Non-Comparison Sorts**: Exploit keys/numeric structures. Achieve $O(N)$ or $O(N + K)$.
   - Counting Sort, Radix Sort, Bucket Sort.

### Stability:
A sorting algorithm is **stable** if elements with identical keys retain their relative order. Merge Sort and Insertion Sort are stable; Quick Sort and Heap Sort are unstable by default.


---

## ⚡ Time & Space Complexity

| Algorithm | Best Time | Average Time | Worst Time | Space | Stable? |
|---|---|---|---|---|---|
| Selection Sort | $O(N^2)$ | $O(N^2)$ | $O(N^2)$ | $O(1)$ | No |
| Bubble Sort | $O(N)$ | $O(N^2)$ | $O(N^2)$ | $O(1)$ | Yes |
| Insertion Sort | $O(N)$ | $O(N^2)$ | $O(N^2)$ | $O(1)$ | Yes |
| Merge Sort | $O(N \log N)$ | $O(N \log N)$ | $O(N \log N)$ | $O(N)$ | Yes |
| Quick Sort | $O(N \log N)$ | $O(N \log N)$ | $O(N^2)$ | $O(\log N)$ | No |
| Heap Sort | $O(N \log N)$ | $O(N \log N)$ | $O(N \log N)$ | $O(1)$ | No |
| Counting Sort | $O(N + K)$ | $O(N + K)$ | $O(N + K)$ | $O(K)$ | Yes |
| Radix Sort | $O(D(N + K))$ | $O(D(N + K))$ | $O(D(N + K))$ | $O(N + K)$ | Yes |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Merge Sort (Divide & Conquer)
```cpp
void merge(vector<int>& arr, int l, int m, int r) {
    vector<int> left(arr.begin() + l, arr.begin() + m + 1);
    vector<int> right(arr.begin() + m + 1, arr.begin() + r + 1);
    int i = 0, j = 0, k = l;
    while (i < (int)left.size() && j < (int)right.size()) {
        if (left[i] <= right[j]) arr[k++] = left[i++];
        else arr[k++] = right[j++];
    }
    while (i < (int)left.size()) arr[k++] = left[i++];
    while (j < (int)right.size()) arr[k++] = right[j++];
}
void mergeSort(vector<int>& arr, int l, int r) {
    if (l >= r) return;
    int m = l + (r - l) / 2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}
```

### 2. Quick Sort with Randomized Pivot
```cpp
int partition(vector<int>& arr, int low, int high) {
    int pivotIdx = low + rand() % (high - low + 1);
    swap(arr[pivotIdx], arr[high]);
    int pivot = arr[high], i = low - 1;
    for (int j = low; j < high; ++j) {
        if (arr[j] <= pivot) swap(arr[++i], arr[j]);
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}
```

### 3. Custom Comparator in C++
```cpp
// Sort pairs by first ascending, second descending
sort(pairs.begin(), pairs.end(), [](const pair<int, int>& a, const pair<int, int>& b) {
    if (a.first != b.first) return a.first < b.first;
    return a.second > b.second;
});
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <algorithm>
#include <vector>

std::vector<int> nums = {5, 2, 8, 1, 9};

// Introsort (Quick Sort + Heap Sort + Insertion Sort): O(N log N)
std::sort(nums.begin(), nums.end());

// Stable Sort (Merge Sort variant): O(N log N)
std::stable_sort(nums.begin(), nums.end());

// Partial Sort: sort only the top K elements in O(N log K)
std::partial_sort(nums.begin(), nums.begin() + 3, nums.end());

// Nth element: quickselect in O(N) average time
std::nth_element(nums.begin(), nums.begin() + 2, nums.end());
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Sort an Array | 🟡 Medium | LC 912 | Merge / Quick / Heap Sort |
| Sort Colors | 🟡 Medium | LC 75 | Dutch National Flag (Counting) |
| Kth Largest Element in an Array | 🟡 Medium | LC 215 | Quickselect / Heap |
| Merge Intervals | 🟡 Medium | LC 56 | Sort by Start Time |
| Non-overlapping Intervals | 🟡 Medium | LC 435 | Greedy Sort by End Time |
| Largest Number | 🟡 Medium | LC 179 | Custom Comparator ($a+b > b+a$) |
| Count of Smaller Numbers After Self | 🔴 Hard | LC 315 | Merge Sort Inversion Count |


---

## ⚠️ Common Pitfalls & C++ Tips

- `std::sort` in C++ is **IntroSort** (a hybrid of Quick Sort, Heap Sort, and Insertion Sort), running in $O(N \log N)$ worst-case.
- When writing custom comparators, enforce **Strict Weak Ordering**: `comp(x, x)` must evaluate to `false`. Returning `true` for equals causes undefined behavior / crashes.

