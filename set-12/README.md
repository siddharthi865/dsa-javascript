# Set 12

| S.No. | Question                                                                                                                         |
| ----- | -------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Swap pairs of nodes in a linked list](#question-1-swap-pairs-of-nodes-in-a-linked-list)                                         |
| 2.    | [Reverse nodes in k-group](#question-2-reverse-nodes-in-k-group)                                                                 |
| 3.    | [Copy a linked list with random pointer](#question-3-copy-a-linked-list-with-random-pointer)                                     |
| 4.    | [Segregate even and odd nodes in linked list](#question-4-segregate-even-and-odd-nodes-in-linked-list)                           |
| 5.    | [Remove all occurrences of a given key from a linked list](#question-5-remove-all-occurrences-of-a-given-key-from-a-linked-list) |
| 6.    | [Intersection point of two linked lists](#question-6-intersection-point-of-two-linked-lists)                                     |
| 7.    | [Add one to a number represented by a linked list](#question-7-add-one-to-a-number-represented-by-a-linked-list)                 |
| 8.    | [Flatten a binary tree to a linked list](#question-8-flatten-a-binary-tree-to-a-linked-list)                                     |
| 9.    | [Find loop length in linked list](#question-9-find-loop-length-in-linked-list)                                                   |
| 10.   | [Convert sorted linked list to BST](#question-10-convert-sorted-linked-list-to-bst)                                              |
| 11.   | [Implement a stack with getMin and getMax in O(1)](#question-11-implement-a-stack-with-getmin-and-getmax-in-o1)                  |
| 12.   | [Largest rectangle area in histogram using stack](#question-12-largest-rectangle-area-in-histogram-using-stack)                  |
| 13.   | [Maximum of sliding window using deque](#question-13-maximum-of-sliding-window-using-deque)                                      |
| 14.   | [Implement circular deque](#question-14-implement-circular-deque)                                                                |
| 15.   | [Evaluate expression with +, -, \*, / operators using stack](#question-15-evaluate-expression-with------operators-using-stack)   |
| 16.   | [Find all nodes at distance k from a given node](#question-16-find-all-nodes-at-distance-k-from-a-given-node)                    |
| 17.   | [Check if two trees are mirror images](#question-17-check-if-two-trees-are-mirror-images)                                        |
| 18.   | [Count leaf nodes in a binary tree](#question-18-count-leaf-nodes-in-a-binary-tree)                                              |
| 19.   | [Construct binary tree from preorder and inorder](#question-19-construct-binary-tree-from-preorder-and-inorder)                  |
| 20.   | [Construct binary tree from postorder and inorder](#question-20-construct-binary-tree-from-postorder-and-inorder)                |

## Question 1. Swap pairs of nodes in a linked list

# Swap Pairs of Nodes in a Linked List

## Direct Answer

Given a linked list, swap every two adjacent nodes and return the head of the modified list.

**Example:**

```
Input:  1 -> 2 -> 3 -> 4
Output: 2 -> 1 -> 4 -> 3
```

**Important:** Swap the actual nodes, not just their values.

---

# 1. Problem Understanding

Given:

```
1 -> 2 -> 3 -> 4
```

Swap nodes in pairs:

```
(1,2) => 2 -> 1
(3,4) => 4 -> 3
```

Result:

```
2 -> 1 -> 4 -> 3
```

If the list has an odd number of nodes:

```
1 -> 2 -> 3
```

Output:

```
2 -> 1 -> 3
```

The last node remains unchanged.

---

# 2. Approach 1: Iterative (Optimal)

## Idea

Use a dummy node before the head.

For every pair:

- `first` = current node
- `second` = next node

Adjust pointers:

Before:

```
prev -> first -> second -> nextPair
```

After:

```
prev -> second -> first -> nextPair
```

Move `prev` forward and continue.

---

## Dry Run

### Input

```
1 -> 2 -> 3 -> 4
```

### First Swap

```
dummy -> 1 -> 2 -> 3 -> 4

first = 1
second = 2
```

After swapping:

```
dummy -> 2 -> 1 -> 3 -> 4
```

### Second Swap

```
dummy -> 2 -> 1 -> 3 -> 4

first = 3
second = 4
```

After swapping:

```
dummy -> 2 -> 1 -> 4 -> 3
```

Return:

```
2 -> 1 -> 4 -> 3
```

---

## JavaScript Code (Iterative)

```javascript
const swapPairs = (head) => {
  const dummy = { val: 0, next: head };

  let prev = dummy;

  while (prev.next && prev.next.next) {
    const first = prev.next;
    const second = first.next;

    // Swap
    first.next = second.next;
    second.next = first;
    prev.next = second;

    // Move to next pair
    prev = first;
  }

  return dummy.next;
};
```

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

Each node is visited once.

### Space Complexity

```
O(1)
```

Only a few pointers are used.

---

# 3. Approach 2: Recursive

## Idea

For every pair:

1. Swap first two nodes.
2. Recursively solve the remaining list.
3. Connect the swapped pair with the result.

---

## Recursive Relation

Given:

```
1 -> 2 -> 3 -> 4
```

Swap first pair:

```
2 -> 1
```

Recursively solve:

```
3 -> 4
```

Result:

```
4 -> 3
```

Connect:

```
2 -> 1 -> 4 -> 3
```

---

## JavaScript Code (Recursive)

```javascript
const swapPairs = (head) => {
  if (!head || !head.next) {
    return head;
  }

  const first = head;
  const second = head.next;

  first.next = swapPairs(second.next);
  second.next = first;

  return second;
};
```

---

## Complexity Analysis

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

Recursive call stack.

---

# 4. Edge Cases

### Empty List

```
Input: null
Output: null
```

---

### Single Node

```
Input: 1
Output: 1
```

---

### Two Nodes

```
Input: 1 -> 2
Output: 2 -> 1
```

---

### Odd Length

```
Input: 1 -> 2 -> 3 -> 4 -> 5
Output: 2 -> 1 -> 4 -> 3 -> 5
```

Last node remains unchanged.

---

# 5. Common Pitfalls

### ❌ Swapping Values Instead of Nodes

Many interviewers specifically require swapping nodes.

Bad:

```javascript
[first.val, second.val] = [second.val, first.val];
```

Good:

```javascript
first.next = second.next;
second.next = first;
```

---

### ❌ Losing References

Always save pointers before modifying links.

Incorrect pointer updates can disconnect the list.

---

### ❌ Forgetting Dummy Node

Without a dummy node, handling the head swap becomes more complicated.

The dummy node simplifies edge cases significantly.

---

# 6. Interview Tips

When explaining your solution:

1. Mention that values should not be swapped.
2. Use a dummy node to simplify head handling.
3. Explain the pointer transformation:

```
prev -> first -> second -> next

becomes

prev -> second -> first -> next
```

4. State complexities clearly:

| Approach  | Time | Space |
| --------- | ---- | ----- |
| Iterative | O(n) | O(1)  |
| Recursive | O(n) | O(n)  |

For interviews, the **iterative dummy-node solution** is generally considered the optimal and most expected answer.

## Question 2. Reverse nodes in k-group

# Reverse Nodes in k-Group

## Direct Answer

Given a linked list, reverse the nodes of the list **k at a time** and return the modified list.

- If the number of nodes remaining is less than `k`, leave them as they are.
- Reverse the **nodes**, not the values.

### Example

```text
Input:  1 -> 2 -> 3 -> 4 -> 5
k = 2

Output: 2 -> 1 -> 4 -> 3 -> 5
```

```text
Input:  1 -> 2 -> 3 -> 4 -> 5
k = 3

Output: 3 -> 2 -> 1 -> 4 -> 5
```

---

# 1. Problem Understanding

For every group of `k` nodes:

1. Check if `k` nodes exist.
2. Reverse those `k` nodes.
3. Connect the reversed group to the previous part.
4. Continue with the next group.

Example:

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6
k = 3
```

Groups:

```text
(1 2 3) (4 5 6)
```

After reversal:

```text
3 -> 2 -> 1 -> 6 -> 5 -> 4
```

---

# 2. Approach 1: Iterative (Optimal)

## Idea

Use a dummy node and process one group at a time.

For each group:

1. Find the kth node.
2. Reverse nodes between current position and kth node.
3. Reconnect the reversed group.
4. Move to the next group.

---

## Visualization

Before reversal:

```text
dummy -> 1 -> 2 -> 3 -> 4 -> 5
          ↑         ↑
       groupPrev   kth
```

Reverse:

```text
dummy -> 3 -> 2 -> 1 -> 4 -> 5
```

Then move to the next group.

---

## JavaScript Code (Iterative)

```javascript
const reverseKGroup = (head, k) => {
  if (!head || k <= 1) return head;

  const dummy = { val: 0, next: head };
  let groupPrev = dummy;

  while (true) {
    let kth = groupPrev;

    // Find kth node
    for (let i = 0; i < k && kth; i++) {
      kth = kth.next;
    }

    if (!kth) break;

    const groupNext = kth.next;

    // Reverse current group
    let prev = groupNext;
    let curr = groupPrev.next;

    while (curr !== groupNext) {
      const temp = curr.next;
      curr.next = prev;
      prev = curr;
      curr = temp;
    }

    const temp = groupPrev.next;

    groupPrev.next = kth;
    groupPrev = temp;
  }

  return dummy.next;
};
```

---

# 3. Dry Run

### Input

```text
1 -> 2 -> 3 -> 4 -> 5
k = 2
```

### Group 1

Reverse:

```text
1 -> 2
```

Becomes:

```text
2 -> 1
```

List:

```text
2 -> 1 -> 3 -> 4 -> 5
```

---

### Group 2

Reverse:

```text
3 -> 4
```

Becomes:

```text
4 -> 3
```

List:

```text
2 -> 1 -> 4 -> 3 -> 5
```

---

### Remaining

Only one node left:

```text
5
```

Less than `k`, so keep it unchanged.

Final:

```text
2 -> 1 -> 4 -> 3 -> 5
```

---

# 4. Approach 2: Recursive

## Idea

1. Verify that at least `k` nodes exist.
2. Reverse first `k` nodes.
3. Recursively process the remaining list.
4. Connect them.

---

## JavaScript Code (Recursive)

```javascript
const reverseKGroup = (head, k) => {
  let count = 0;
  let curr = head;

  while (curr && count < k) {
    curr = curr.next;
    count++;
  }

  if (count < k) return head;

  let prev = null;
  curr = head;
  count = 0;

  while (curr && count < k) {
    const next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
    count++;
  }

  head.next = reverseKGroup(curr, k);

  return prev;
};
```

---

# 5. Complexity Analysis

## Iterative Approach

### Time Complexity

```text
O(n)
```

Each node is processed once.

### Space Complexity

```text
O(1)
```

No extra data structure.

---

## Recursive Approach

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(n/k)
```

Due to recursion stack.

---

# 6. Edge Cases

### Empty List

```text
head = null
k = 3

Output: null
```

---

### k = 1

```text
1 -> 2 -> 3

Output:
1 -> 2 -> 3
```

No changes.

---

### k Greater Than Length

```text
1 -> 2
k = 3
```

Output:

```text
1 -> 2
```

No reversal.

---

### Length Multiple of k

```text
1 -> 2 -> 3 -> 4
k = 2
```

Output:

```text
2 -> 1 -> 4 -> 3
```

All nodes reversed in groups.

---

# 7. Common Pitfalls

### ❌ Reversing the Last Incomplete Group

Incorrect:

```text
1 -> 2 -> 3 -> 4 -> 5
k = 3

Wrong:
3 -> 2 -> 1 -> 5 -> 4
```

Correct:

```text
3 -> 2 -> 1 -> 4 -> 5
```

---

### ❌ Losing the Next Group

Always store:

```javascript
const groupNext = kth.next;
```

before reversing.

---

### ❌ Forgetting to Reconnect Groups

After reversal:

```javascript
groupPrev.next = kth;
```

and update:

```javascript
groupPrev = oldGroupStart;
```

---

# Interview Tips

When asked this problem:

1. Clarify whether the last group with fewer than `k` nodes should be reversed (usually **No**, as in LeetCode 25).
2. Explain that you first locate the kth node to ensure a complete group exists.
3. Reverse the group in-place.
4. Reconnect the group to the previous and next parts of the list.
5. Mention the optimal complexity:

```text
Time  : O(n)
Space : O(1)
```

The **iterative in-place solution using a dummy node** is the most expected and interview-preferred approach.

## Question 3. Copy a linked list with random pointer

## Question 4. Segregate even and odd nodes in linked list

## Question 5. Remove all occurrences of a given key from a linked list

## Question 6. Intersection point of two linked lists

## Question 7. Add one to a number represented by a linked list

## Question 8. Flatten a binary tree to a linked list

## Question 9. Find loop length in linked list

## Question 10. Convert sorted linked list to BST

## Question 11. Implement a stack with getMin and getMax in O(1)

## Question 12. Largest rectangle area in histogram using stack

## Question 13. Maximum of sliding window using deque

## Question 14. Implement circular deque

## Question 15. Evaluate expression with +, -, \*, / operators using stack

## Question 16. Find all nodes at distance k from a given node

## Question 17. Check if two trees are mirror images

## Question 18. Count leaf nodes in a binary tree

## Question 19. Construct binary tree from preorder and inorder

## Question 20. Construct binary tree from postorder and inorder
