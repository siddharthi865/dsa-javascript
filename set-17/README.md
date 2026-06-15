# Set 17

| S.No. | Question                                                                                                                                                   |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Reverse alternate k nodes in a linked list](#question-1-reverse-alternate-k-nodes-in-a-linked-list)                                                       |
| 2.    | [Rotate a linked list clockwise](#question-2-rotate-a-linked-list-clockwise)                                                                               |
| 3.    | [Merge two sorted circular linked lists](#question-3-merge-two-sorted-circular-linked-lists)                                                               |
| 4.    | [Split a circular linked list into two halves](#question-4-split-a-circular-linked-list-into-two-halves)                                                   |
| 5.    | [Reverse nodes between m and n](#question-5-reverse-nodes-between-m-and-n)                                                                                 |
| 6.    | [Remove duplicates from unsorted linked list](#question-6-remove-duplicates-from-unsorted-linked-list)                                                     |
| 7.    | [Add two numbers represented by linked lists (digits stored in reverse)](#question-7-add-two-numbers-represented-by-linked-lists-digits-stored-in-reverse) |
| 8.    | [Reorder linked list: L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...](#question-8-reorder-linked-list-l0--ln--l1--ln-1--l2--ln-2--)                                 |
| 9.    | [Check if linked list is a palindrome using stack](#question-9-check-if-linked-list-is-a-palindrome-using-stack)                                           |
| 10.   | [Intersection of two sorted linked lists](#question-10-intersection-of-two-sorted-linked-lists)                                                            |
| 11.   | [Maximum rectangle area in binary matrix using stack](#question-11-maximum-rectangle-area-in-binary-matrix-using-stack)                                    |
| 12.   | [Sliding window minimum using deque](#question-12-sliding-window-minimum-using-deque)                                                                      |
| 13.   | [Implement a queue with max() operation in O(1)](#question-13-implement-a-queue-with-max-operation-in-o1)                                                  |
| 14.   | [Sort a stack using recursion](#question-14-sort-a-stack-using-recursion)                                                                                  |
| 15.   | [Evaluate expression with parentheses and operators](#question-15-evaluate-expression-with-parentheses-and-operators)                                      |
| 16.   | [Next smaller element problem](#question-16-next-smaller-element-problem)                                                                                  |
| 17.   | [Implement two stacks in one array](#question-17-implement-two-stacks-in-one-array)                                                                        |
| 18.   | [Design circular buffer](#question-18-design-circular-buffer)                                                                                              |
| 19.   | [Reverse a queue using recursion](#question-19-reverse-a-queue-using-recursion)                                                                            |
| 20.   | [Sliding window sum of size k](#question-20-sliding-window-sum-of-size-k)                                                                                  |

## Question 1. Reverse alternate k nodes in a linked list

# Reverse Alternate K Nodes in a Linked List

## Direct Answer

Given a linked list and an integer `k`, reverse the first `k` nodes, skip the next `k` nodes, then reverse the following `k` nodes, and continue this pattern until the end of the list.

### Example

**Input:**

```
1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10
k = 3
```

**Output:**

```
3 → 2 → 1 → 4 → 5 → 6 → 9 → 8 → 7 → 10
```

- Reverse first 3 nodes: `1,2,3` → `3,2,1`
- Skip next 3 nodes: `4,5,6`
- Reverse next 3 nodes: `7,8,9` → `9,8,7`
- Remaining node `10` stays as is

---

# 1. Problem Understanding

We need to process the linked list in blocks of size `k`.

Pattern:

```
Reverse K nodes
Skip K nodes
Reverse K nodes
Skip K nodes
...
```

The challenge is maintaining proper links between:

- Reversed segments
- Skipped segments
- Remaining nodes

---

# 2. Approach 1: Recursive Solution

### Idea

For each recursive call:

1. Reverse first `k` nodes.
2. Connect the last node of the reversed block to the next section.
3. Skip next `k` nodes.
4. Recursively process the remaining list.

---

### Steps

For a given head:

```
Reverse first K nodes
|
Skip next K nodes
|
Recursively process remaining list
```

---

### JavaScript Code

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

const reverseAlternateKNodes = (head, k) => {
  if (!head || k <= 1) return head;

  let current = head;
  let prev = null;
  let next = null;
  let count = 0;

  // Reverse first k nodes
  while (current && count < k) {
    next = current.next;
    current.next = prev;
    prev = current;
    current = next;
    count++;
  }

  // Connect original head (now last node)
  head.next = current;

  // Skip next k nodes
  count = 0;
  let temp = current;

  while (temp && count < k - 1) {
    temp = temp.next;
    count++;
  }

  // Recur for remaining nodes
  if (temp && temp.next) {
    temp.next = reverseAlternateKNodes(temp.next, k);
  }

  return prev;
};
```

---

# 3. Approach 2: Iterative Solution

### Idea

Instead of recursion:

- Maintain pointers to previous segments.
- Alternate between:
  - Reversing `k` nodes
  - Skipping `k` nodes

This avoids recursion stack overhead.

---

### Algorithm

Use a boolean flag:

```javascript
shouldReverse = true;
```

For each block:

- If true → reverse k nodes.
- If false → move ahead k nodes.
- Toggle flag.

---

### JavaScript Code

```javascript
const reverseAlternateKNodesIterative = (head, k) => {
  if (!head || k <= 1) return head;

  let current = head;
  let prevTail = null;
  let newHead = null;
  let shouldReverse = true;

  while (current) {
    if (shouldReverse) {
      let lastNodeOfPrevPart = prevTail;
      let lastNodeOfSubList = current;

      let prev = null;
      let next = null;
      let i = 0;

      while (current && i < k) {
        next = current.next;
        current.next = prev;
        prev = current;
        current = next;
        i++;
      }

      if (!newHead) {
        newHead = prev;
      }

      if (lastNodeOfPrevPart) {
        lastNodeOfPrevPart.next = prev;
      }

      lastNodeOfSubList.next = current;
      prevTail = lastNodeOfSubList;
    } else {
      let i = 0;

      while (current && i < k) {
        prevTail = current;
        current = current.next;
        i++;
      }
    }

    shouldReverse = !shouldReverse;
  }

  return newHead;
};
```

---

# 4. Complexity Analysis

### Recursive Approach

| Metric | Complexity             |
| ------ | ---------------------- |
| Time   | O(n)                   |
| Space  | O(n/k) recursion stack |

---

### Iterative Approach

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

---

# 5. Example Walkthrough

List:

```
1 → 2 → 3 → 4 → 5 → 6 → 7 → 8
k = 2
```

### Reverse first 2

```
2 → 1 → 3 → 4 → 5 → 6 → 7 → 8
```

### Skip next 2

```
2 → 1 → 3 → 4 → 5 → 6 → 7 → 8
```

### Reverse next 2

```
2 → 1 → 3 → 4 → 6 → 5 → 7 → 8
```

### Skip next 2

Done.

Result:

```
2 → 1 → 3 → 4 → 6 → 5 → 7 → 8
```

---

# 6. Edge Cases

### Empty List

```javascript
head = null;
```

Output:

```javascript
null;
```

---

### Single Node

```javascript
1;
k = 3;
```

Output:

```javascript
1;
```

---

### k = 1

Every block contains one node.

```javascript
1 → 2 → 3
```

Output remains:

```javascript
1 → 2 → 3
```

---

### Remaining Nodes < k

```javascript
1 → 2 → 3 → 4 → 5
k = 3
```

The final incomplete group is handled naturally and reversed if it falls in a reverse segment.

---

# 7. Interview Tips

- Clarify whether the last group with fewer than `k` nodes should be reversed.
- Mention that the problem is an extension of **Reverse Nodes in K-Group**.
- Recursive solution is easier to explain.
- Iterative solution is preferred in production because it uses **O(1)** extra space.
- Be careful with reconnecting:
  - Previous segment
  - Reversed segment
  - Skipped segment
  - Remaining list

**Expected Interview Solution:** Iterative or Recursive traversal with **O(n) time**, where alternate groups of size `k` are reversed and skipped.

## Question 2. Rotate a linked list clockwise

# Rotate a Linked List Clockwise

## Direct Answer

To rotate a linked list clockwise by `k` positions:

1. Find the length of the list.
2. Connect the last node to the head, making it circular.
3. Find the new tail at position `n - (k % n) - 1`.
4. The next node becomes the new head.
5. Break the circular link.

### Example

**Input:**

```text
1 → 2 → 3 → 4 → 5
k = 2
```

**Output:**

```text
4 → 5 → 1 → 2 → 3
```

The last 2 nodes move to the front.

---

# 1. Problem Understanding

A clockwise (right) rotation by `k` means:

```text
Original:
1 → 2 → 3 → 4 → 5

Rotate by 1:
5 → 1 → 2 → 3 → 4

Rotate by 2:
4 → 5 → 1 → 2 → 3
```

Notice:

- Last `k` nodes become the first `k` nodes.
- Relative order remains unchanged.

---

# 2. Approach 1: Circular Linked List Technique (Optimal)

### Idea

Instead of moving nodes one by one:

1. Compute length `n`.
2. Reduce rotations:

```text
k = k % n
```

because rotating `n` times gives the original list.

3. Make the list circular.
4. Locate the new tail.
5. Break the circle.

---

### Visualization

```text
1 → 2 → 3 → 4 → 5
                ↓
                ↑
```

After connecting tail to head:

```text
1 → 2 → 3 → 4 → 5
↑               ↓
└───────────────┘
```

For `k = 2`:

```text
New Tail = 3
New Head = 4
```

Break:

```text
4 → 5 → 1 → 2 → 3
```

---

# 3. JavaScript Solution (Optimal)

```javascript
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

const rotateRight = (head, k) => {
  if (!head || !head.next || k === 0) {
    return head;
  }

  // Find length and tail
  let length = 1;
  let tail = head;

  while (tail.next) {
    tail = tail.next;
    length++;
  }

  k = k % length;

  if (k === 0) {
    return head;
  }

  // Make circular
  tail.next = head;

  let stepsToNewTail = length - k - 1;
  let newTail = head;

  while (stepsToNewTail > 0) {
    newTail = newTail.next;
    stepsToNewTail--;
  }

  let newHead = newTail.next;

  // Break circle
  newTail.next = null;

  return newHead;
};
```

---

# 4. Approach 2: Rotate One Position K Times

### Idea

Perform:

```text
Find last node
Move it to front
Repeat k times
```

Example:

```text
1 → 2 → 3 → 4 → 5

After 1 rotation:
5 → 1 → 2 → 3 → 4

After 2 rotations:
4 → 5 → 1 → 2 → 3
```

### Complexity

For each rotation:

```text
O(n)
```

Repeated `k` times:

```text
O(n × k)
```

Not efficient for large `k`.

---

### JavaScript Code

```javascript
const rotateRightBruteForce = (head, k) => {
  if (!head || !head.next) return head;

  while (k--) {
    let prev = null;
    let curr = head;

    while (curr.next) {
      prev = curr;
      curr = curr.next;
    }

    prev.next = null;
    curr.next = head;
    head = curr;
  }

  return head;
};
```

---

# 5. Complexity Analysis

## Optimal Circular Method

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(1)       |

---

## Brute Force

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n × k)   |
| Space  | O(1)       |

---

# 6. Example Walkthrough

### Input

```text
1 → 2 → 3 → 4 → 5
k = 2
```

### Step 1: Length

```text
length = 5
```

### Step 2

```text
k = 2 % 5 = 2
```

### Step 3: Circular

```text
1 → 2 → 3 → 4 → 5
↑               ↓
└───────────────┘
```

### Step 4

```text
newTail position = 5 - 2 - 1 = 2
```

Node:

```text
3
```

### Step 5

```text
newHead = 4
```

Break after 3:

```text
4 → 5 → 1 → 2 → 3
```

---

# 7. Edge Cases

### Empty List

```javascript
head = null;
```

Output:

```javascript
null;
```

---

### Single Node

```text
1
k = 100
```

Output:

```text
1
```

---

### k = 0

```text
1 → 2 → 3
```

Output unchanged.

---

### k > Length

```text
1 → 2 → 3 → 4 → 5
k = 12
```

```text
12 % 5 = 2
```

Same as rotating by 2.

---

### k = Length

```text
1 → 2 → 3 → 4
k = 4
```

```text
4 % 4 = 0
```

No change.

---

# Interview Tips

- The key optimization is recognizing that rotations repeat every `n` steps, so use `k % n`.
- Making the list circular is the cleanest and most common interview solution.
- Be careful with the formula:

```text
newTail = n - k - 1
newHead = newTail.next
```

- Mention that the optimal solution achieves **O(n) time** and **O(1) space**, which is the expected interview answer.

## Question 3. Merge two sorted circular linked lists

## Question 4. Split a circular linked list into two halves

## Question 5. Reverse nodes between m and n

## Question 6. Remove duplicates from unsorted linked list

## Question 7. Add two numbers represented by linked lists (digits stored in reverse)

## Question 8. Reorder linked list: L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...

## Question 9. Check if linked list is a palindrome using stack

## Question 10. Intersection of two sorted linked lists

## Question 11. Maximum rectangle area in binary matrix using stack

## Question 12. Sliding window minimum using deque

## Question 13. Implement a queue with max() operation in O(1)

## Question 14. Sort a stack using recursion

## Question 15. Evaluate expression with parentheses and operators

## Question 16. Next smaller element problem

## Question 17. Implement two stacks in one array

## Question 18. Design circular buffer

## Question 19. Reverse a queue using recursion

## Question 20. Sliding window sum of size k
