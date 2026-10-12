# 📌 Arrays & Vectors

An array is a contiguous collection of elements of the same data type stored at contiguous memory locations.

---

## 📖 Overview & Core Concepts

An array is one of the most fundamental data structures in computer science. Elements are placed sequentially in memory, enabling constant time $O(1)$ random access using index arithmetic. In C++, dynamic arrays are represented by `std::vector` from the Standard Template Library (STL).

### Memory Layout & Characteristics
- **Contiguous Allocation**: Fast cache locality (L1/L2 cache prefetching).
- **Fixed vs Dynamic**: Static arrays (`int arr[N]`, `std::array<int, N>`) have fixed compile-time size. `std::vector<int>` grows dynamically by doubling capacity when full (amortized $O(1)$ insertion).
- **Bounds Checking**: `arr[i]` does not perform bounds checking (faster), while `arr.at(i)` throws `std::out_of_range` exception.


---

## ⚡ Time & Space Complexity

| Operation | Time Complexity (Static Array) | Time Complexity (std::vector) | Space Complexity |
|---|---|---|---|
| Access by Index (`arr[i]`) | $O(1)$ | $O(1)$ | $O(1)$ |
| Search (Unsorted) | $O(N)$ | $O(N)$ | $O(1)$ |
| Search (Sorted - Binary) | $O(\log N)$ | $O(\log N)$ | $O(1)$ |
| Insertion at End | N/A | Amortized $O(1)$ | $O(1)$ |
| Insertion at Beginning/Middle | $O(N)$ | $O(N)$ | $O(1)$ |
| Deletion at End | N/A | $O(1)$ | $O(1)$ |
| Deletion at Middle | $O(N)$ | $O(N)$ | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Prefix Sum & Difference Arrays
Precomputing running sums allows querying subarray sum in $O(1)$ time:
$$\text{prefix}[i] = \text{prefix}[i-1] + \text{nums}[i]$$
$$\text{sum}(L, R) = \text{prefix}[R] - \text{prefix}[L-1]$$

### 2. Kadane's Algorithm (Maximum Subarray Sum)
Greedy dynamic programming technique running in $O(N)$ time and $O(1)$ space:
```cpp
int maxSubArray(const vector<int>& nums) {
    int maxSoFar = nums[0], currMax = nums[0];
    for (size_t i = 1; i < nums.size(); ++i) {
        currMax = max(nums[i], currMax + nums[i]);
        maxSoFar = max(maxSoFar, currMax);
    }
    return maxSoFar;
}
```

### 3. Dutch National Flag (3-Way Partitioning)
Partitioning elements around pivot values (e.g., sorting 0s, 1s, and 2s) in a single pass with low, mid, high pointers.

### 4. 2D Arrays / Matrix Traversal
- Spiral Traversal
- Matrix Rotation (Transpose + Reverse rows)
- Prefix sum on 2D matrices (2D prefix sum grid)


---

## 🛠️ C++ STL & Implementations

```cpp
#include <vector>
#include <numeric>
#include <algorithm>

std::vector<int> v = {1, 2, 3, 4, 5};
v.push_back(6);         // Amortized O(1)
v.pop_back();          // O(1)
v.reserve(100);        // Avoid reallocations
int sum = std::accumulate(v.begin(), v.end(), 0);
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Two Sum | 🟢 Easy | LC 1 | Hash Map / Complement |
| Best Time to Buy and Sell Stock | 🟢 Easy | LC 121 | Running Min / Kadane's |
| Contains Duplicate | 🟢 Easy | LC 217 | Hash Set / Sorting |
| Maximum Subarray | 🟡 Medium | LC 53 | Kadane's Algorithm |
| Product of Array Except Self | 🟡 Medium | LC 238 | Prefix & Suffix Products |
| Subarray Sum Equals K | 🟡 Medium | LC 560 | Prefix Sum + Hash Map |
| Rotate Image | 🟡 Medium | LC 48 | Transpose & Reverse |
| Spiral Matrix | 🟡 Medium | LC 54 | Boundary Simulation |
| First Missing Positive | 🔴 Hard | LC 41 | Index Hash / Cycle Sort |
| Trapping Rain Water | 🔴 Hard | LC 42 | Two Pointers / Precomputed Heights |


---

## ⚠️ Common Pitfalls & C++ Tips

- Always use `v.reserve(N)` if the maximum size is known beforehand to prevent costly dynamic reallocations.
- Prefer passing vectors by `const std::vector<T>&` to avoid deep copying.
- Beware of integer overflow when summing vector elements; use `long long`.
- In C++17/20, use `std::size()` and range-based for loops: `for (const auto& x : v)`.

