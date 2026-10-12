# 📌 Hashing & Hash Tables

Hashing maps keys of arbitrary size to fixed-size integers using a hash function, supporting average $O(1)$ search, insertion, and deletion.

---

## 📖 Overview & Core Concepts

A Hash Table provides associative array capabilities. A hash function transforms a key into an index within a bucket array.

### Collision Resolution Strategies:
1. **Chaining (Separate Chaining)**: Each bucket stores a linked list of elements sharing the same hash. Used by `std::unordered_map` in C++.
2. **Open Addressing**: Elements reside directly in the table. Collisions are handled via:
   - Linear Probing: $h(k, i) = (h'(k) + i) \pmod M$
   - Quadratic Probing: $h(k, i) = (h'(k) + c_1 i + c_2 i^2) \pmod M$
   - Double Hashing: $h(k, i) = (h_1(k) + i \cdot h_2(k)) \pmod M$

### Load Factor & Rehashing:
$$\alpha = \frac{N}{\text{bucket count}}$$
When $\alpha$ exceeds a threshold (typically $1.0$ in C++), the table doubles buckets and rehashes all elements.


---

## ⚡ Time & Space Complexity

| Operation | `std::unordered_map` (Hash Table) | `std::map` (Red-Black Tree) |
|---|---|---|
| Search | Average $O(1)$, Worst $O(N)$ | $O(\log N)$ guaranteed |
| Insertion | Average $O(1)$, Worst $O(N)$ | $O(\log N)$ guaranteed |
| Deletion | Average $O(1)$, Worst $O(N)$ | $O(\log N)$ guaranteed |
| Order Preserved? | No | Yes (Strict Weak Ordering) |
| Key Requirement | Hash function + Equality (`==`) | Strict Weak Ordering (`<`) |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Complement Search (Two Sum)
```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> seen;
    for (int i = 0; i < (int)nums.size(); ++i) {
        int comp = target - nums[i];
        if (seen.count(comp)) return {seen[comp], i};
        seen[nums[i]] = i;
    }
    return {};
}
```

### 2. Prefix Sum with Hash Map
Used to find the count of subarrays summing to $K$:
```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> prefixFreq;
    prefixFreq[0] = 1; // Base case for subarrays starting at index 0
    int sum = 0, count = 0;
    for (int x : nums) {
        sum += x;
        if (prefixFreq.count(sum - k)) count += prefixFreq[sum - k];
        prefixFreq[sum]++;
    }
    return count;
}
```

### 3. Custom Hash Function for `pair<int, int>`
C++ does not provide a default hash for `std::pair`. You must supply one:
```cpp
struct PairHash {
    size_t operator()(const pair<int, int>& p) const {
        return hash<int>()(p.first) ^ (hash<int>()(p.second) << 16);
    }
};
unordered_set<pair<int, int>, PairHash> visited;
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <unordered_map>
#include <unordered_set>

std::unordered_map<std::string, int> freq;
freq["apple"]++;
if (freq.find("apple") != freq.end()) { /* Found */ }
freq.reserve(1024); // Prevents rehashing
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Two Sum | 🟢 Easy | LC 1 | Value $\to$ Index Map |
| Contains Duplicate | 🟢 Easy | LC 217 | Hash Set Existence |
| Valid Anagram | 🟢 Easy | LC 242 | Character Frequency |
| Group Anagrams | 🟡 Medium | LC 49 | Sorted String as Key |
| Subarray Sum Equals K | 🟡 Medium | LC 560 | Prefix Sum + Frequency Map |
| Longest Consecutive Sequence | 🟡 Medium | LC 128 | Hash Set + Sequence Start Check |
| LRU Cache | 🟡 Medium | LC 146 | Hash Map + Doubly Linked List |
| Design Twitter | 🟡 Medium | LC 355 | Hash Map + Max-Heap |


---

## ⚠️ Common Pitfalls & C++ Tips

- In competitive programming, `std::unordered_map` with integer keys can be hacked to $O(N^2)$ using custom anti-hash tests. Use a custom randomized splitmix64 hash function.
- Prefer `map.count(key)` or `map.find(key)` over `map[key]` when checking existence to avoid inserting default values inadvertently.

