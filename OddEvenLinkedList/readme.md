# LeetCode 328: Odd Even Linked List

## Problem Description

**Difficulty:** Medium

Given the head of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return the reordered list.

The first node is considered odd, and the second node is even, and so on.

**Note:** The relative order inside both the even and odd groups should remain as it was in the input.

**Constraints:**
- You must solve the problem in O(1) extra space complexity and O(n) time complexity
- The number of nodes in the linked list is in the range [0, 10^4]
- -10^6 <= Node.val <= 10^6

## Examples

### Example 1:
```
Input: head = [1,2,3,4,5]
Output: [1,3,5,2,4]

Visualization:
Original: 1 -> 2 -> 3 -> 4 -> 5
Result:   1 -> 3 -> 5 -> 2 -> 4
          ^odd nodes^  ^even nodes^
```

### Example 2:
```
Input: head = [2,1,3,5,6,4,7]
Output: [2,3,6,7,1,5,4]

Visualization:
Original: 2 -> 1 -> 3 -> 5 -> 6 -> 4 -> 7
Result:   2 -> 3 -> 6 -> 7 -> 1 -> 5 -> 4
          ^odd nodes^      ^even nodes^
```

## Approach

The key insight is to maintain two separate chains:
1. **Odd chain**: Contains nodes at odd positions (1st, 3rd, 5th, ...)
2. **Even chain**: Contains nodes at even positions (2nd, 4th, 6th, ...)

### Algorithm Steps:
1. Handle edge cases (empty list or single node)
2. Initialize pointers:
   - `odd`: Points to the first node (odd position)
   - `even`: Points to the second node (even position)
   - `evenList`: Keeps track of the head of even chain
3. Traverse the list and separate odd and even positioned nodes
4. Connect the end of odd chain to the head of even chain

### Time & Space Complexity:
- **Time Complexity:** O(n) - We traverse the list once
- **Space Complexity:** O(1) - We only use a constant amount of extra space

## Solution

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        // Handle edge cases
        if (head == nullptr || head->next == nullptr) return head;
        
        ListNode* odd = head;           // Points to current odd node
        ListNode* even = head->next;    // Points to current even node
        ListNode* evenList = head->next; // Keep reference to even list head
        
        // Separate odd and even positioned nodes
        while (even != nullptr && even->next != nullptr) {
            odd->next = even->next;     // Connect odd to next odd
            odd = odd->next;            // Move odd pointer
            even->next = odd->next;     // Connect even to next even
            even = even->next;          // Move even pointer
        }
        
        // Connect odd list to even list
        odd->next = evenList;
        
        return head;
    }
};
```

## Step-by-Step Walkthrough

Let's trace through Example 1: `[1,2,3,4,5]`

**Initial State:**
```
1 -> 2 -> 3 -> 4 -> 5
^    ^
odd  even
     evenList
```

**Iteration 1:**
- `odd->next = even->next` → `1->next = 3`
- `odd = odd->next` → `odd` points to 3
- `even->next = odd->next` → `2->next = 4`
- `even = even->next` → `even` points to 4

```
1 -> 3 -> 4 -> 5
     ^    ^
   odd  even

2 -> 4 -> 5
^
evenList
```

**Iteration 2:**
- `odd->next = even->next` → `3->next = 5`
- `odd = odd->next` → `odd` points to 5
- `even->next = odd->next` → `4->next = null`
- `even = even->next` → `even` becomes null

```
1 -> 3 -> 5
          ^
        odd

2 -> 4
^
evenList
```

**Final Step:**
- `odd->next = evenList` → Connect odd chain to even chain

```
Final Result: 1 -> 3 -> 5 -> 2 -> 4
```

## Key Points

1. **Index vs Position:** The problem refers to "odd/even indices" but actually means positions (1st, 2nd, 3rd, etc.)
2. **Preserve Order:** The relative order within odd and even groups must be maintained
3. **In-Place Solution:** We rearrange pointers without creating new nodes
4. **Edge Cases:** Handle empty lists and single-node lists properly

## Related Topics
- Linked List
- Two Pointers
- Pointer Manipulation