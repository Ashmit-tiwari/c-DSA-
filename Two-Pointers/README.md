# 📌 Two Pointers Technique

Two Pointers is an algorithmic pattern where two pointer variables traverse an iterable data structure (array, string, or linked list) to optimize time complexity from $O(N^2)$ to $O(N)$.

---

## 📖 Overview & Core Concepts

The Two Pointers pattern is effective when working with sorted arrays or linear sequences. Instead of using nested loops to search pairs or triples, two pointers navigate the search space simultaneously by taking advantage of monotonic properties.

### Three Primary Variations:
1. **Opposite Ends (Left & Right)**: Pointers start at opposite ends (`0` and `N - 1`) and move toward each other (e.g., Two Sum II in sorted array, Valid Palindrome, Trapping Rain Water).
2. **Fast & Slow Pointers (Hare & Tortoise)**: Pointers move in the same direction at different speeds (e.g., Floyd's Cycle Detection in Linked List, Middle of Linked List, Remove Duplicates).
3. **Merging / Sliding Pointers**: Pointers traverse two different sorted arrays concurrently (e.g., Merge Sorted Array, Intersection of Two Arrays).


---

## ⚡ Time & Space Complexity

| Operation / Technique | Typical Time Complexity | Space Complexity |
|---|---|---|
| Opposite Direction (Two Sum II) | $O(N)$ | $O(1)$ |
| Fast & Slow (Cycle Detection) | $O(N)$ | $O(1)$ |
| 3Sum (Sort + Two Pointers) | $O(N^2)$ | $O(1)$ or $O(N)$ auxiliary |
| Trapping Rain Water | $O(N)$ | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Two Sum in Sorted Array
```cpp
vector<int> twoSumSorted(const vector<int>& numbers, int target) {
    int left = 0, right = (int)numbers.size() - 1;
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        if (sum == target) return {left + 1, right + 1};
        else if (sum < target) left++;
        else right--;
    }
    return {};
}
```

### 2. Fast & Slow (In-Place Array Mutation)
```cpp
int removeDuplicates(vector<int>& nums) {
    if (nums.empty()) return 0;
    int slow = 0;
    for (int fast = 1; fast < (int)nums.size(); ++fast) {
        if (nums[fast] != nums[slow]) {
            nums[++slow] = nums[fast];
        }
    }
    return slow + 1;
}
```

### 3. Container With Most Water
```cpp
int maxArea(const vector<int>& height) {
    int left = 0, right = (int)height.size() - 1;
    int maxWater = 0;
    while (left < right) {
        int h = min(height[left], height[right]);
        maxWater = max(maxWater, h * (right - left));
        if (height[left] < height[right]) left++;
        else right--;
    }
    return maxWater;
}
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <vector>
#include <algorithm>

std::vector<int> nums = {3, 1, 4, 1, 5, 9};
std::sort(nums.begin(), nums.end()); // Crucial prerequisite for many two-pointer problems
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Valid Palindrome | 🟢 Easy | LC 125 | Inward Two Pointers |
| Remove Duplicates from Sorted Array | 🟢 Easy | LC 26 | Fast & Slow Pointers |
| Move Zeroes | 🟢 Easy | LC 283 | Read & Write Pointers |
| Two Sum II - Input Array Is Sorted | 🟡 Medium | LC 167 | Opposite Direction Pointers |
| 3Sum | 🟡 Medium | LC 15 | Sort + Two Pointers + Duplicate Skip |
| Container With Most Water | 🟡 Medium | LC 11 | Greedy Width Reduction |
| 4Sum | 🟡 Medium | LC 18 | Sort + Nested Pointers |
| Trapping Rain Water | 🔴 Hard | LC 42 | Two Pointers with LeftMax/RightMax |


---

## ⚠️ Common Pitfalls & C++ Tips

- Make sure to handle duplicate elements properly (skip identical elements after finding a match in 3Sum).
- Ensure termination condition is clear: `left < right` vs `left <= right`.
- Verify sorting prerequisites: Does the problem allow reordering, or does it demand original indices?

