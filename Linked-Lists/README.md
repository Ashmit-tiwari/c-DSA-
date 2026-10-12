# 📌 Linked Lists

A linked list is a linear data structure where elements are not stored contiguously; each node contains a value and a pointer to the next (and optionally previous) node.

---

## 📖 Overview & Core Concepts

Linked lists offer $O(1)$ insertion and deletion once a reference to the node is obtained, but require $O(N)$ sequential traversal for random access.

### Types of Linked Lists:
1. **Singly Linked List**: Each node contains `val` and `next`.
2. **Doubly Linked List**: Each node contains `val`, `next`, and `prev`. Enables $O(1)$ bidirectional deletion and traversal.
3. **Circular Linked List**: The last node points back to the head node.

### Core Structure:
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```


---

## ⚡ Time & Space Complexity

| Operation | Singly Linked List | Doubly Linked List | Array / Vector |
|---|---|---|---|
| Access by Index | $O(N)$ | $O(N)$ | $O(1)$ |
| Search Value | $O(N)$ | $O(N)$ | $O(N)$ (or $O(\log N)$ if sorted) |
| Insert / Delete at Head | $O(1)$ | $O(1)$ | $O(N)$ |
| Insert / Delete at Tail | $O(N)$ (or $O(1)$ with tail ptr) | $O(1)$ with tail ptr | Amortized $O(1)$ |
| Insert / Delete given Node | $O(N)$ (need prev) | $O(1)$ | $O(N)$ |


---

## 💡 Common Algorithmic Patterns & Techniques

### 1. Dummy Node Technique
Avoids special handling for edge cases at the head (e.g., removing the head):
```cpp
ListNode* removeElements(ListNode* head, int val) {
    ListNode dummy(0, head);
    ListNode* curr = &dummy;
    while (curr->next) {
        if (curr->next->val == val) {
            ListNode* toDelete = curr->next;
            curr->next = curr->next->next;
            delete toDelete;
        } else {
            curr = curr->next;
        }
    }
    return dummy.next;
}
```

### 2. Fast & Slow Pointers (Floyd's Cycle Finding)
```cpp
bool hasCycle(ListNode *head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
```

### 3. In-Place Reversal of Singly Linked List
```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* nextTemp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```


---

## 🛠️ C++ STL & Implementations

```cpp
#include <forward_list> // Singly linked list in STL
#include <list>         // Doubly linked list in STL

std::list<int> l = {1, 2, 3};
l.push_front(0);
l.push_back(4);
l.erase(l.begin());
```

---

## 🎯 Curated Problem Roadmap

| Problem | Difficulty | LeetCode # | Key Pattern |
|---|---|---|---|
| Reverse Linked List | 🟢 Easy | LC 206 | 3-Pointer Iteration / Recursion |
| Merge Two Sorted Lists | 🟢 Easy | LC 21 | Dummy Node + Two Pointers |
| Linked List Cycle | 🟢 Easy | LC 141 | Floyd's Tortoise and Hare |
| Remove Nth Node From End of List | 🟡 Medium | LC 19 | Two Pointers separated by $N$ |
| Reorder List | 🟡 Medium | LC 143 | Find Mid + Reverse Half + Merge |
| Add Two Numbers | 🟡 Medium | LC 2 | Dummy Node + Carry simulation |
| Copy List with Random Pointer | 🟡 Medium | LC 138 | Interweaving Nodes / Hash Map |
| LRU Cache | 🟡 Medium | LC 146 | Hash Map + Doubly Linked List |
| Reverse Nodes in k-Group | 🔴 Hard | LC 25 | Sub-segment Reversal Loop |
| Merge k Sorted Lists | 🔴 Hard | LC 23 | Min-Heap / Divide & Conquer |


---

## ⚠️ Common Pitfalls & C++ Tips

- Always initialize dummy nodes on stack (`ListNode dummy(0)`) to simplify boundary logic without manual memory management.
- Guard against null pointer dereference (`fast && fast->next`).
- Remember to prevent memory leaks in real applications by calling `delete` on unlinked nodes.

