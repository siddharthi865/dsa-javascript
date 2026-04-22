# Set 1

## Questions Table

| S. No. | Question                                                                                                                              |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | [What is an array? How is it stored in memory?](#question-1-what-is-an-array-how-is-it-stored-in-memory)                              |
| 2      | [What is the difference between static and dynamic arrays?](#question-2-what-is-the-difference-between-static-and-dynamic-arrays)     |
| 3      | [How to find the maximum and minimum element in an array?](#question-3-how-to-find-the-maximum-and-minimum-element-in-an-array)       |
| 4      | [How to reverse an array in-place?](#question-4-how-to-reverse-an-array-in-place)                                                     |
| 5      | [How to rotate an array by k positions?](#question-5-how-to-rotate-an-array-by-k-positions)                                           |
| 6      | [How to find the sum of all elements in an array?](#question-6-how-to-find-the-sum-of-all-elements-in-an-array)                       |
| 7      | [How to find the second largest element in an array?](#question-7-how-to-find-the-second-largest-element-in-an-array)                 |
| 8      | [How to remove duplicates from a sorted array?](#question-8-how-to-remove-duplicates-from-a-sorted-array)                             |
| 9      | [How to count the frequency of elements in an array?](#question-9-how-to-count-the-frequency-of-elements-in-an-array)                 |
| 10     | [Find the subarray with the maximum sum (Kadane’s algorithm).](#question-10-find-the-subarray-with-the-maximum-sum-kadanes-algorithm) |
| 11     | [Reverse a string without using library functions.](#question-11-reverse-a-string-without-using-library-functions)                    |
| 12     | [Check if a string is a palindrome.](#question-12-check-if-a-string-is-a-palindrome)                                                  |
| 13     | [Find the first non-repeating character in a string.](#question-13-find-the-first-non-repeating-character-in-a-string)                |
| 14     | [Count the number of vowels and consonants in a string.](#question-14-count-the-number-of-vowels-and-consonants-in-a-string)          |
| 15     | [Implement strStr() (substring search).](#question-15-implement-strstr-substring-search)                                              |
| 16     | [What is a linked list? Difference from arrays.](#question-16-what-is-a-linked-list-difference-from-arrays)                           |
| 17     | [Implement a singly linked list.](#question-17-implement-a-singly-linked-list)                                                        |
| 18     | [Implement a doubly linked list.](#question-18-implement-a-doubly-linked-list)                                                        |
| 19     | [Reverse a linked list iteratively.](#question-19-reverse-a-linked-list-iteratively)                                                  |
| 20     | [Reverse a linked list recursively.](#question-20-reverse-a-linked-list-recursively)                                                  |

---

## Question 1. What is an array? How is it stored in memory?

## Question 2. What is the difference between static and dynamic arrays?

## Question 3. How to find the maximum and minimum element in an array?

## Question 4. How to reverse an array in-place?

## Question 5. How to rotate an array by k positions?

## Question 6. How to find the sum of all elements in an array?

## Question 7. How to find the second largest element in an array?

## Question 8. How to remove duplicates from a sorted array?

## Question 9. How to count the frequency of elements in an array?

## Question 10. Find the subarray with the maximum sum (Kadane’s algorithm).

## Question 11. Reverse a string without using library functions.

## Question 12. Check if a string is a palindrome.

## Question 13. Find the first non-repeating character in a string.

## Question 14. Count the number of vowels and consonants in a string.

## Question 15. Implement strStr() (substring search).

## Question 16. What is a linked list? Difference from arrays.

## Question 17. Implement a singly linked list.

## Question 18. Implement a doubly linked list.

## Question 19. Reverse a linked list iteratively.

## Question 20. Reverse a linked list recursively.

```js
// The Node Class
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class SinglyLinkedList {
  constructor() {
    this.head = null;
  }

  insertAtEnd(data) {
    const newNode = new Node(data);
    if (!this.head) {
      this.head = newNode;
      return;
    }

    let current = this.head;
    while (current.next) {
      current = current.next;
    }

    current.next = newNode;
  }

  // Utility to print linked list
  printList() {
    let current = this.head;
    let str = "";
    while (current) {
      str += current.data + "->";
      current = current.next;
    }

    str += "null";
    console.log("str");
  }

  reverseRecursive(node = this.head) {
    // Base case: empty list or last node
    if (!node || !node.next) {
      this.head = node;
      return node;
    }

    // Recursively reverse the rest of the list
    let newHead = this.reverseRecursive(node.next);

    // Reverse current node's pointer
    node.next.next = node;
    node.next = null;
    return newHead;
  }
}

const list = new SinglyLinkedList();
list.insertAtEnd(1);
list.insertAtEnd(2);
list.insertAtEnd(3);
list.insertAtEnd(4);

console.log("Original List:");
list.printList();
// Output: 1 -> 2 -> 3 -> 4 -> null

list.reverseRecursive();
console.log("Reversed List (Recursive):");
list.printList();
// Output: 4 -> 3 -> 2 -> 1 -> null
```
