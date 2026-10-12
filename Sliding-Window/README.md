# 📌 Sliding Window Technique

The Sliding Window technique solves contiguous subarray or substring problems by maintaining a window defined by two boundaries $[L, R]$, adjusting window size to avoid redundant recalculations.

---

## 📖 Overview & Core Concepts

The Sliding Window pattern is used for problems asking for the longest, shortest, or optimal contiguous subarray/substring that satisfies a condition.

### Window Classification:
1. **Fixed Size Window ($K$)**:
   - The window length is constant ($R - L + 1 = K$).
   - Slide the window by adding element at $R$ and dropping element at $L$.
   - Time Complexity: $O(N)$ instead of $O(N \times K)$.

2. **Dynamic / Variable Size Window**:
   - Expand the right pointer $R$ to explore and satisfy criteria.
   - Once invalid (or valid, depending on objective), contract the left pointer $L$ until the condition is satisfied.
   - Each element enters and exits the window at most once: $2N$ operations $\Rightarrow O(N)$ time.


---

## ⚡ Time & Space Complexity

| Technique | Time Complexity | Auxiliary Space |
|---|---|---|
| Fixed Window (Max sum of size K) | $O(N)$ | $O(1)$ |
| Dynamic Window (Longest substring) | $O(N)$ | $O(\Sigma)$ (Character alphabet size) |
| Monotonic Queue Sliding Window (LC 239) | $O(N)$ | $O(K)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Fixed Window Template
```cpp
int maxSubarraySumK(const vector<int>& nums, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; ++i) windowSum += nums[i];
    int maxSum = windowSum;
    for (int i = k; i < (int)nums.size(); ++i) {
        windowSum += nums[i] - nums[i - k];
        maxSum = max(maxSum, windowSum);
    }
    return maxSum;
}
```

### 2. Dynamic Window Template (Find Longest Valid)
```cpp
int lengthOfLongestSubstring(const string& s) {
    int lastSeen[256];
    fill(begin(lastSeen), end(lastSeen), -1);
    int maxLen = 0, left = 0;
    for (int right = 0; right < (int)s.size(); ++right) {
        if (lastSeen[(unsigned char)s[right]] >= left) {
            left = lastSeen[(unsigned char)s[right]] + 1;
        }
        lastSeen[(unsigned char)s[right]] = right;
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

### 3. Dynamic Window Template (Find Shortest Valid)
```cpp
int minSubArrayLen(int target, const vector<int>& nums) {
    int left = 0, sum = 0, minLen = INT_MAX;
    for (int right = 0; right < (int)nums.size(); ++right) {
        sum += nums[right];
        while (sum >= target) {
            minLen = min(minLen, right - left + 1);
            sum -= nums[left++];
        }
    }
    return minLen == INT_MAX ? 0 : minLen;
}
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <deque>
#include <unordered_map>
// std::deque is essential for monotonic sliding window maximum/minimum
std::deque<int> dq;
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Maximum Average Subarray I | 🟢 Easy | LC 643 | Fixed Window Size $K$ |
| Longest Substring Without Repeating Characters | 🟡 Medium | LC 3 | Dynamic Window + Last Index Map |
| Minimum Size Subarray Sum | 🟡 Medium | LC 209 | Dynamic Window (Shortest) |
| Longest Repeating Character Replacement | 🟡 Medium | LC 424 | Window with Max Count |
| Fruit Into Baskets | 🟡 Medium | LC 904 | At Most 2 Distinct Elements |
| Permutation in String | 🟡 Medium | LC 567 | Fixed Window Frequency Map |
| Sliding Window Maximum | 🔴 Hard | LC 239 | Monotonic Deque |
| Minimum Window Substring | 🔴 Hard | LC 76 | Dynamic Window + Match Counter |


---

## ⚠️ Common Pitfalls & C++ Tips

- Verify if negative numbers are allowed; if array contains negatives, sliding window may fail for subarray sum (use Prefix Sum + Hash Map instead).
- Ensure window variables (frequencies, sums) are updated when contracting left boundary.
- Pay attention to 0-indexed vs 1-indexed length calculation: length is always `right - left + 1`.

