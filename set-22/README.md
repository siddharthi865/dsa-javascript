# Set 22

| S.No. | Question                                                                                                                                          |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Merge two sorted linked lists in reverse order](#question-1-merge-two-sorted-linked-lists-in-reverse-order)                                      |
| 2.    | [Reverse every alternate K nodes in linked list](#question-2-reverse-every-alternate-k-nodes-in-linked-list)                                      |
| 3.    | [Find intersection point of Y-shaped linked lists](#question-3-find-intersection-point-of-y-shaped-linked-lists)                                  |
| 4.    | [Add two numbers when digits stored in forward order](#question-4-add-two-numbers-when-digits-stored-in-forward-order)                            |
| 5.    | [Clone linked list with next and random pointer using O(1) space](#question-5-clone-linked-list-with-next-and-random-pointer-using-o1-space)      |
| 6.    | [Detect loop and remove it in a linked list](#question-6-detect-loop-and-remove-it-in-a-linked-list)                                              |
| 7.    | [Reverse a doubly linked list in pairs](#question-7-reverse-a-doubly-linked-list-in-pairs)                                                        |
| 8.    | [Convert linked list to binary tree](#question-8-convert-linked-list-to-binary-tree)                                                              |
| 9.    | [Check if linked list is palindrome using recursion](#question-9-check-if-linked-list-is-palindrome-using-recursion)                              |
| 10.   | [Flatten linked list of linked lists](#question-10-flatten-linked-list-of-linked-lists)                                                           |
| 11.   | [Implement a queue using circular array](#question-11-implement-a-queue-using-circular-array)                                                     |
| 12.   | [Implement stack with getMin(), getMax(), and getMedian() efficiently](#question-12-implement-stack-with-getmin-getmax-and-getmedian-efficiently) |
| 13.   | [Next greater element in circular array](#question-13-next-greater-element-in-circular-array)                                                     |
| 14.   | [Largest rectangle in binary matrix using histogram trick](#question-14-largest-rectangle-in-binary-matrix-using-histogram-trick)                 |
| 15.   | [Sliding window maximum using deque](#question-15-sliding-window-maximum-using-deque)                                                             |
| 16.   | [Sort stack using another stack only](#question-16-sort-stack-using-another-stack-only)                                                           |
| 17.   | [Evaluate prefix expression](#question-17-evaluate-prefix-expression)                                                                             |
| 18.   | [Minimum cost to make valid parentheses using stack](#question-18-minimum-cost-to-make-valid-parentheses-using-stack)                             |
| 19.   | [Maximum of sliding window using segment tree](#question-19-maximum-of-sliding-window-using-segment-tree)                                         |
| 20.   | [Implement deque with O(1) getMax](#question-20-implement-deque-with-o1-getmax)                                                                   |

## Question 1. Merge two sorted linked lists in reverse order

# Merge Two Sorted Linked Lists in Reverse Order

## Direct Answer

Given two **sorted linked lists**, merge them into a **single linked list sorted in reverse (descending) order**.

A very efficient approach is:

- Traverse both lists like the standard merge step of Merge Sort.
- Instead of appending nodes at the tail, insert each selected node at the **head** of the result list.
- This automatically builds the merged list in reverse order.

---

# 1. Problem Understanding

### Example

```text
List1: 1 -> 3 -> 5
List2: 2 -> 4 -> 6

Merged (ascending):
1 -> 2 -> 3 -> 4 -> 5 -> 6

Required Output (reverse):
6 -> 5 -> 4 -> 3 -> 2 -> 1
```

---

# 2. Approach 1: Merge While Reversing (Optimal)

### Idea

Normally during merge:

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6
```

But instead of attaching nodes at the end, insert every chosen node at the beginning.

Example:

```text
Take 1 → Result: 1
Take 2 → Result: 2 -> 1
Take 3 → Result: 3 -> 2 -> 1
...
```

Final result automatically becomes:

```text
6 -> 5 -> 4 -> 3 -> 2 -> 1
```

---

## Algorithm

1. Initialize `result = null`.
2. Compare heads of both lists.
3. Pick smaller node.
4. Insert picked node at the front of `result`.
5. Move forward in that list.
6. Process remaining nodes similarly.
7. Return `result`.

---

## JavaScript Code

```javascript
class ListNode {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

const mergeReverse = (l1, l2) => {
  let result = null;

  while (l1 && l2) {
    let temp;

    if (l1.val <= l2.val) {
      temp = l1;
      l1 = l1.next;
    } else {
      temp = l2;
      l2 = l2.next;
    }

    temp.next = result;
    result = temp;
  }

  while (l1) {
    const temp = l1;
    l1 = l1.next;

    temp.next = result;
    result = temp;
  }

  while (l2) {
    const temp = l2;
    l2 = l2.next;

    temp.next = result;
    result = temp;
  }

  return result;
};
```

---

## Complexity Analysis

| Metric | Value    |
| ------ | -------- |
| Time   | O(m + n) |
| Space  | O(1)     |

where:

- `m` = length of first list
- `n` = length of second list

This is optimal because each node is visited exactly once.

---

# 3. Approach 2: Merge Normally Then Reverse

### Idea

1. Merge two sorted lists into ascending order.
2. Reverse the merged list.

---

## JavaScript Code

```javascript
const mergeSortedLists = (l1, l2) => {
  const dummy = new ListNode(-1);
  let tail = dummy;

  while (l1 && l2) {
    if (l1.val <= l2.val) {
      tail.next = l1;
      l1 = l1.next;
    } else {
      tail.next = l2;
      l2 = l2.next;
    }

    tail = tail.next;
  }

  tail.next = l1 || l2;

  return dummy.next;
};

const reverseList = (head) => {
  let prev = null;
  let curr = head;

  while (curr) {
    const next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
  }

  return prev;
};

const mergeReverse = (l1, l2) => {
  const merged = mergeSortedLists(l1, l2);
  return reverseList(merged);
};
```

---

## Complexity Analysis

| Metric | Value    |
| ------ | -------- |
| Time   | O(m + n) |
| Space  | O(1)     |

Still optimal, but requires two traversals.

---

# Edge Cases

### Empty Lists

```text
L1 = null
L2 = null

Output = null
```

---

### One Empty List

```text
L1 = 1 -> 3 -> 5
L2 = null

Output:
5 -> 3 -> 1
```

---

### Duplicate Values

```text
L1 = 1 -> 2 -> 2
L2 = 2 -> 3

Output:
3 -> 2 -> 2 -> 2 -> 1
```

---

# Interview Tips

If asked in an interview:

1. Mention the standard merge procedure.
2. Observe that inserting merged nodes at the **front** naturally reverses the order.
3. Highlight that this avoids a separate reversal pass.
4. State the optimal complexity:

```text
Time  : O(m + n)
Space : O(1)
```

A strong interview answer is:

> "I can merge the two sorted lists exactly like Merge Sort, but instead of appending nodes to the tail, I insert each selected node at the head of the result list. Since the smallest elements are processed first and continually pushed to the front, the final merged list is automatically in descending order."

## Question 2. Reverse every alternate K nodes in linked list

# Reverse Every Alternate K Nodes in a Linked List

## Direct Answer

Given a linked list and an integer `K`, reverse every alternate group of `K` nodes.

- Reverse the first `K` nodes.
- Skip (leave unchanged) the next `K` nodes.
- Reverse the following `K` nodes.
- Continue this pattern until the end of the list.

---

# 1. Problem Understanding

### Example

```text
Input:
1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10
K = 3

Reverse first 3:
3 -> 2 -> 1

Skip next 3:
4 -> 5 -> 6

Reverse next 3:
9 -> 8 -> 7

Remaining:
10

Output:
3 -> 2 -> 1 -> 4 -> 5 -> 6 -> 9 -> 8 -> 7 -> 10
```

---

# 2. Approach 1: Recursive Solution

### Idea

For each segment:

1. Reverse first `K` nodes.
2. Connect the last node of the reversed segment to the next node.
3. Skip the next `K` nodes.
4. Recursively process the remaining list.

---

## Visual Walkthrough

```text
Original:
1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8

K = 2

Reverse:
2 -> 1

Skip:
3 -> 4

Reverse:
6 -> 5

Skip:
7 -> 8

Result:
2 -> 1 -> 3 -> 4 -> 6 -> 5 -> 7 -> 8
```

---

## JavaScript Code (Recursive)

```javascript
class ListNode {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

const reverseAlternateKNodes = (head, k) => {
  if (!head || k <= 1) return head;

  let current = head;
  let prev = null;
  let next = null;
  let count = 0;

  // Reverse first K nodes
  while (current && count < k) {
    next = current.next;
    current.next = prev;
    prev = current;
    current = next;
    count++;
  }

  // Connect original head to remaining list
  head.next = current;

  // Skip next K nodes
  count = 0;
  let temp = current;

  while (temp && count < k - 1) {
    temp = temp.next;
    count++;
  }

  // Recur for remaining nodes
  if (temp) {
    temp.next = reverseAlternateKNodes(temp.next, k);
  }

  return prev;
};
```

---

# 3. Approach 2: Iterative Solution

### Idea

Use a flag:

- `shouldReverse = true`
- Reverse one block of `K`
- Skip one block of `K`
- Toggle the flag

Repeat until the list ends.

This avoids recursion stack usage.

---

## JavaScript Code (Iterative)

```javascript
const reverseAlternateKNodes = (head, k) => {
  if (!head || k <= 1) return head;

  let current = head;
  let prevTail = null;
  let newHead = null;
  let shouldReverse = true;

  while (current) {
    let count = 0;

    if (shouldReverse) {
      let blockHead = current;
      let prev = null;

      while (current && count < k) {
        const next = current.next;
        current.next = prev;
        prev = current;
        current = next;
        count++;
      }

      if (!newHead) newHead = prev;

      if (prevTail) {
        prevTail.next = prev;
      }

      blockHead.next = current;
      prevTail = blockHead;
    } else {
      let blockHead = current;

      while (current && count < k) {
        prevTail = current;
        current = current.next;
        count++;
      }

      if (!newHead) {
        newHead = blockHead;
      }
    }

    shouldReverse = !shouldReverse;
  }

  return newHead;
};
```

---

# 4. Complexity Analysis

Let:

- `n` = number of nodes

| Operation               | Complexity             |
| ----------------------- | ---------------------- |
| Traversal               | O(n)                   |
| Reversal Work           | O(n)                   |
| Total Time              | O(n)                   |
| Extra Space (Iterative) | O(1)                   |
| Extra Space (Recursive) | O(n/k) recursion stack |

---

# 5. Edge Cases

### Empty List

```text
Input:
null

Output:
null
```

---

### Single Node

```text
1
K = 3

Output:
1
```

---

### K = 1

Every block contains one node.

```text
1 -> 2 -> 3

Output:
1 -> 2 -> 3
```

No changes.

---

### K Greater Than Length

```text
1 -> 2 -> 3
K = 5

Output:
3 -> 2 -> 1
```

Entire list gets reversed because the first block has fewer than `K` nodes.

---

# 6. Common Pitfalls

### Forgetting to reconnect segments

After reversing:

```text
3 -> 2 -> 1
```

you must connect:

```text
1 -> nextPart
```

otherwise the rest of the list is lost.

---

### Incorrect skipping count

When skipping a block:

```javascript
while (current && count < k)
```

ensure exactly `K` nodes are skipped before the next reversal.

---

### Losing the next pointer

Always save:

```javascript
const next = current.next;
```

before changing links.

---

# Interview Tips

When explaining:

> "I process the list in blocks of size K. I alternate between reversing a block and leaving a block unchanged. By maintaining proper connections between block boundaries, I can solve the problem in O(n) time. The iterative solution achieves O(1) extra space, while the recursive version is often easier to explain and implement."

### Final Complexity

```text
Time  : O(n)
Space : O(1) iterative
Space : O(n/k) recursive
```

This is the expected optimal solution in interviews.

## Question 3. Find intersection point of Y-shaped linked lists

## Question 4. Add two numbers when digits stored in forward order

## Question 5. Clone linked list with next and random pointer using O(1) space

## Question 6. Detect loop and remove it in a linked list

## Question 7. Reverse a doubly linked list in pairs

## Question 8. Convert linked list to binary tree

## Question 9. Check if linked list is palindrome using recursion

## Question 10. Flatten linked list of linked lists

## Question 11. Implement a queue using circular array

## Question 12. Implement stack with getMin(), getMax(), and getMedian() efficiently

## Question 13. Next greater element in circular array

## Question 14. Largest rectangle in binary matrix using histogram trick

## Question 15. Sliding window maximum using deque

## Question 16. Sort stack using another stack only

## Question 17. Evaluate prefix expression

## Question 18. Minimum cost to make valid parentheses using stack

## Question 19. Maximum of sliding window using segment tree

## Question 20. Implement deque with O(1) getMax
