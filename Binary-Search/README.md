# 📌 Binary Search

Binary Search is a divide-and-conquer algorithm that finds the position of a target value within a sorted or monotonic space in logarithmic time $O(\log N)$.

---

## 📖 Overview & Core Concepts

Binary search operates by comparing the target with the middle element. If they match, search finishes; otherwise, half of the search space is eliminated. Beyond searching arrays, binary search applies to any monotonic predicate function $f(x) \in \{0, 1\}$.

### Search Space Monotonicity:
If a property satisfies:
$$f(x) = \begin{cases} \text{false} & x < \text{ans} \\ \text{true} & x \ge \text{ans} \end{cases}$$
Binary search can find the threshold $\text{ans}$ in $O(\log(\text{search\_space})) \times \text{cost}(f)$.


---

## ⚡ Time & Space Complexity

| Variant | Time Complexity | Space Complexity |
|---|---|---|
| Classical Binary Search | $O(\log N)$ | $O(1)$ Iterative / $O(\log N)$ Recursive |
| `std::lower_bound` / `std::upper_bound` | $O(\log N)$ | $O(1)$ |
| Binary Search on Answer | $O(\text{cost}(f) \times \log(\text{range}))$ | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Standard Template (Closed Interval `[low, high]`)
```cpp
int binarySearch(const vector<int>& nums, int target) {
    int low = 0, high = (int)nums.size() - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2; // Prevents integer overflow
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```

### 2. Lower Bound (First Element $\ge$ Target)
```cpp
int lowerBound(const vector<int>& nums, int target) {
    int low = 0, high = (int)nums.size();
    while (low < high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] < target) low = mid + 1;
        else high = mid;
    }
    return low;
}
```

### 3. Binary Search on Answer
```cpp
bool isValid(int mid, const vector<int>& weights, int days);

int shipWithinDays(vector<int>& weights, int days) {
    int low = *max_element(weights.begin(), weights.end());
    int high = accumulate(weights.begin(), weights.end(), 0);
    int ans = high;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (isValid(mid, weights, days)) {
            ans = mid;
            high = mid - 1; // Try smaller capacity
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <algorithm>
#include <vector>

std::vector<int> nums = {1, 3, 3, 5, 7, 9};

// First element >= 3 (index 1)
auto it1 = std::lower_bound(nums.begin(), nums.end(), 3);

// First element > 3 (index 3, value 5)
auto it2 = std::upper_bound(nums.begin(), nums.end(), 3);

// Check if element exists
bool exists = std::binary_search(nums.begin(), nums.end(), 5);
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Binary Search | 🟢 Easy | LC 704 | Standard Template |
| First Bad Version | 🟢 Easy | LC 278 | Lower Bound Monotonic Predicate |
| Search in Rotated Sorted Array | 🟡 Medium | LC 33 | Modified Mid Comparison |
| Find Minimum in Rotated Sorted Array | 🟡 Medium | LC 153 | Inflection Point Search |
| Search a 2D Matrix | 🟡 Medium | LC 74 | Flattened 1D Coordinate Mapping |
| Koko Eating Bananas | 🟡 Medium | LC 875 | Binary Search on Answer |
| Capacity To Ship Packages Within D Days | 🟡 Medium | LC 1011 | Binary Search on Answer |
| Median of Two Sorted Arrays | 🔴 Hard | LC 4 | Binary Search on Partition Index |
| Split Array Largest Sum | 🔴 Hard | LC 410 | Binary Search on Answer |


---

## ⚠️ Common Pitfalls & C++ Tips

- Never calculate `mid` as `(low + high) / 2` to prevent signed 32-bit integer overflow; always use `low + (high - low) / 2`.
- Identify the search space: Does the answer lie within array indices or value range?
- Pay attention to termination conditions (`low <= high` with `low = mid + 1, high = mid - 1` vs `low < high` with `high = mid`).

