# 📌 Trie (Prefix Tree)

A Trie is a tree-like retrieval data structure used to store and search a dynamic set of strings, where keys are usually strings and edges represent characters.

---

## 📖 Overview & Core Concepts

In a Trie, all descendants of a node share a common string prefix associated with that node.

### Advantages over Hash Tables:
- Prefix searching (`startsWith`) in $O(L)$ time, where $L$ is key length.
- Predictable performance: avoids hash collisions and hash recalculations.
- Inorder traversal yields words in alphabetical sorted order.
- Supports bitwise prefix searches (Bitwise Trie for Maximum XOR queries).


---

## ⚡ Time & Space Complexity

| Operation | Time Complexity | Auxiliary Space |
|---|---|---|
| Insert word of length $L$ | $O(L)$ | $O(L \times \Sigma)$ worst case |
| Search word of length $L$ | $O(L)$ | $O(1)$ |
| StartsWith prefix of length $L$ | $O(L)$ | $O(1)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Standard Prefix Tree Implementation
```cpp
class Trie {
    struct TrieNode {
        TrieNode* children[26];
        bool isEndOfWord;
        TrieNode() : isEndOfWord(false) {
            fill(begin(children), end(children), nullptr);
        }
    };
    TrieNode* root;
public:
    Trie() { root = new TrieNode(); }

    void insert(const string& word) {
        TrieNode* curr = root;
        for (char c : word) {
            int idx = c - 'a';
            if (!curr->children[idx]) curr->children[idx] = new TrieNode();
            curr = curr->children[idx];
        }
        curr->isEndOfWord = true;
    }

    bool search(const string& word) {
        TrieNode* curr = root;
        for (char c : word) {
            int idx = c - 'a';
            if (!curr->children[idx]) return false;
            curr = curr->children[idx];
        }
        return curr->isEndOfWord;
    }

    bool startsWith(const string& prefix) {
        TrieNode* curr = root;
        for (char c : prefix) {
            int idx = c - 'a';
            if (!curr->children[idx]) return false;
            curr = curr->children[idx];
        }
        return true;
    }
};
```

### 2. Bitwise Trie (Maximum XOR of Two Numbers)
Insert numbers into a 32-bit binary trie and greedily choose opposite bits ($1 - b$) to maximize XOR.


---

## 🛠️ C++ STL & Implementations

No standard STL container. Implement with array pointer buffers or hash maps for non-alphabet keys.

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Implement Trie (Prefix Tree) | 🟡 Medium | LC 208 | Core Class Implementation |
| Design Add and Search Words Data Structure | 🟡 Medium | LC 211 | Trie DFS with Wildcard '.' |
| Word Search II | 🔴 Hard | LC 212 | 2D Grid DFS Backtracking + Trie |
| Maximum XOR of Two Numbers in an Array | 🟡 Medium | LC 421 | Bitwise 32-bit Trie |


---

## ⚠️ Common Pitfalls & C++ Tips

- Clean up allocated node memory in the destructor to prevent memory leaks in production code.
- If memory limit is tight, use `std::vector<array<int, 26>>` in a flat array rather than pointer-heavy nodes.

