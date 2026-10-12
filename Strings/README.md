# 📌 Strings & Pattern Matching

A string is a sequence of characters stored in contiguous memory, terminated by a null character `\0` in C-style strings, or dynamically managed via `std::string` in C++.

---

## 📖 Overview & Core Concepts

Strings are fundamental to data representation, text processing, parsers, and compilers. In C++, `std::string` is mutable, providing dynamic resizing and a rich set of manipulation member functions.

### Key Aspects:
- **Mutability**: Unlike Java/Python where strings are immutable, `std::string` in C++ is **mutable**, allowing in-place modifications without memory reallocation if capacity suffices.
- **Small String Optimization (SSO)**: Modern C++ implementations store short strings (typically $\le 15$ or $23$ chars) directly inside the `std::string` object buffer on the stack, bypassing heap allocation.


---

## ⚡ Time & Space Complexity

| Operation | Time Complexity | Space Complexity |
|---|---|---|
| Access by Index (`s[i]`) | $O(1)$ | $O(1)$ |
| Append Character (`push_back`) | Amortized $O(1)$ | $O(1)$ |
| Concatenation (`s1 + s2`) | $O(N + M)$ | $O(N + M)$ |
| Substring (`substr(pos, len)`) | $O(len)$ | $O(len)$ |
| Find Substring (Naive `find`) | $O(N \times M)$ | $O(1)$ |
| KMP Pattern Search | $O(N + M)$ | $O(M)$ |
| Rabin-Karp Search | Average $O(N + M)$ | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Two-Pointer Symmetry & Palindromes
Expanding around centers or comparing from boundary inward:
```cpp
bool isPalindrome(const string& s) {
    int left = 0, right = (int)s.size() - 1;
    while (left < right) {
        while (left < right && !isalnum(s[left])) left++;
        while (left < right && !isalnum(s[right])) right--;
        if (tolower(s[left]) != tolower(s[right])) return false;
        left++; right--;
    }
    return true;
}
```

### 2. Frequency Counting & Anagrams
Using fixed-size arrays (e.g. `int count[26]`) instead of hash maps for $O(1)$ space:
```cpp
bool isAnagram(const string& s, const string& t) {
    if (s.size() != t.size()) return false;
    int freq[26] = {0};
    for (char c : s) freq[c - 'a']++;
    for (char c : t) if (--freq[c - 'a'] < 0) return false;
    return true;
}
```

### 3. Knuth-Morris-Pratt (KMP) Algorithm
Computes the Longest Proper Prefix which is also Suffix ($\pi$ / LPS array) in $O(M)$ time to search patterns in $O(N)$ without backtracking.

### 4. Rolling Hash (Rabin-Karp)
Polynomial rolling hash to compare string windows in $O(1)$ time:
$$H = \sum_{i=0}^{k-1} s[i] \cdot P^{k-1-i} \pmod M$$


---

## 🛠️ C++ STL & Implementations

```cpp
#include <string>
#include <string_view> // C++17 zero-copy string slice

std::string s = "leetcode";
s.push_back('s');
std::string sub = s.substr(0, 4); // "leet"
size_t idx = s.find("code");      // returns 4

// C++17 string_view avoids memory allocations
std::string_view sv = s;
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Valid Palindrome | 🟢 Easy | LC 125 | Two Pointers |
| Valid Anagram | 🟢 Easy | LC 242 | Frequency Array |
| Longest Common Prefix | 🟢 Easy | LC 14 | Horizontal / Vertical Scanning |
| Longest Palindromic Substring | 🟡 Medium | LC 5 | Expand Around Center / DP |
| Group Anagrams | 🟡 Medium | LC 49 | Hash Map + Sorted Key |
| String to Integer (atoi) | 🟡 Medium | LC 8 | State Machine / Parsing |
| Longest Substring Without Repeating Characters | 🟡 Medium | LC 3 | Sliding Window + Hash Set |
| Minimum Window Substring | 🔴 Hard | LC 76 | Sliding Window + Frequency Match |
| Find the Index of the First Occurrence in a String | 🟡 Medium | LC 28 | KMP / Rolling Hash |


---

## ⚠️ Common Pitfalls & C++ Tips

- Use `std::string_view` (C++17) for read-only function parameters to prevent heap allocation.
- Avoid repeated string concatenation in loops with `+`; use `append()` or `+=` to avoid allocating intermediate strings.
- Always check string bounds before calling `s[i]`.

