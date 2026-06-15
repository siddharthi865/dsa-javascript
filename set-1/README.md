# Set 1

| S.No. | Question                                                                                                                                 |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [What is an array? How is it stored in memory?](#question-1-what-is-an-array-how-is-it-stored-in-memory)                                 |
| 2.    | [Difference between static and dynamic arrays](#question-2-difference-between-static-and-dynamic-arrays)                                 |
| 3.    | [How to find the maximum and minimum element in an array?](#question-3-how-to-find-the-maximum-and-minimum-element-in-an-array)          |
| 4.    | [Reverse an array in-place](#question-4-reverse-an-array-in-place)                                                                       |
| 5.    | [Rotate an array by k positions](#question-5-rotate-an-array-by-k-positions)                                                             |
| 6.    | [Find the sum of all elements in an array](#question-6-find-the-sum-of-all-elements-in-an-array)                                         |
| 7.    | [Find the second largest element in an array](#question-7-find-the-second-largest-element-in-an-array)                                   |
| 8.    | [How to remove duplicates from a sorted array?](#question-8-how-to-remove-duplicates-from-a-sorted-array)                                |
| 9.    | [Count the frequency of elements in an array](#question-9-count-the-frequency-of-elements-in-an-array)                                   |
| 10.   | [Find the subarray with the maximum sum (Kadane's algorithm)](#question-10-find-the-subarray-with-the-maximum-sum-kadanes-algorithm)     |
| 11.   | [How do you reverse a string without using library functions?](#question-11-how-do-you-reverse-a-string-without-using-library-functions) |
| 12.   | [Check if a string is a palindrome](#question-12-check-if-a-string-is-a-palindrome)                                                      |
| 13.   | [Find the first non-repeating character in a string](#question-13-find-the-first-non-repeating-character-in-a-string)                    |
| 14.   | [Count the number of vowels and consonants in a string](#question-14-count-the-number-of-vowels-and-consonants-in-a-string)              |
| 15.   | [How do you implement strStr() (substring search)?](#question-15-how-do-you-implement-strstr-substring-search)                           |
| 16.   | [What is a linked list? Difference from arrays](#question-16-what-is-a-linked-list-difference-from-arrays)                               |
| 17.   | [Implement a singly linked list](#question-17-implement-a-singly-linked-list)                                                            |
| 18.   | [Implement a doubly linked list](#question-18-implement-a-doubly-linked-list)                                                            |
| 19.   | [Reverse a linked list iteratively](#question-19-reverse-a-linked-list-iteratively)                                                      |
| 20.   | [Reverse a linked list recursively](#question-20-reverse-a-linked-list-recursively)                                                      |

## Question 1. What is an array? How is it stored in memory?

An **array** is a linear data structure that stores a collection of elements of the **same type** in **contiguous memory locations**.

It allows fast access to elements using an index.

Example in JavaScript:

```js
const arr = [10, 20, 30, 40];
```

Here:

- `arr[0] = 10`
- `arr[1] = 20`
- `arr[2] = 30`
- `arr[3] = 40`

---

# 1. What is an Array?

An array is:

- **Ordered** → elements maintain insertion order
- **Indexed** → each element has a position/index
- **Contiguous in memory** (in low-level implementations)
- Typically stores **homogeneous data** (same data type)

Arrays are one of the most fundamental data structures because they provide:

- O(1) random access
- Efficient iteration
- Good cache locality

---

# 2. How Arrays Are Stored in Memory

The key idea is:

> Array elements are stored in **continuous (contiguous) memory locations**.

Suppose we have:

```text
Index:   0    1    2    3
Array: [10,  20,  30,  40]
```

Assume:

- Each integer takes **4 bytes**
- Base address = `1000`

Memory layout:

```text
Address    Value
1000       10
1004       20
1008       30
1012       40
```

Because elements are contiguous, the computer can calculate any element’s address directly.

---

# 3. Address Calculation Formula

The address of an array element is computed using:

\text{Address}(A[i]) = \text{Base Address} + (i \times \text{Size of Each Element})

Example:

Find address of `arr[2]`

- Base address = `1000`
- Element size = `4 bytes`
- Index = `2`

Calculation:

```text
1000 + (2 × 4)
= 1008
```

So `arr[2]` is stored at address `1008`.

---

# 4. Why Arrays Provide O(1) Access

Since the address is computed mathematically, accessing any index is direct.

Example:

```js
arr[500];
```

The computer does NOT traverse from the beginning.

It directly computes:

```text
base + (500 × element_size)
```

That is why array access is:

```text
Time Complexity: O(1)
```

---

# 5. Visual Representation

```text
arr = [10, 20, 30, 40]

        Contiguous Memory
      ┌────┬────┬────┬────┐
      │ 10 │ 20 │ 30 │ 40 │
      └────┴────┴────┴────┘
Index    0    1    2    3
Addr   1000 1004 1008 1012
```

---

# 6. Advantages of Contiguous Storage

## Fast Random Access

```js
arr[i];
```

works in O(1).

---

## Cache Friendly

Because data is stored together:

- CPU cache performs better
- Iteration becomes very fast

This is one reason arrays are heavily optimized.

---

# 7. Disadvantages of Contiguous Storage

## Fixed Size (in low-level languages)

In languages like C/C++:

```c
int arr[5];
```

size cannot grow dynamically.

---

## Expensive Insertions/Deletions

If inserting in the middle:

```text
[10, 20, 30, 40]
```

Insert `25` at index 2:

```text
[10, 20, 25, 30, 40]
```

Elements must shift.

Time complexity:

```text
O(n)
```

---

# 8. Arrays in JavaScript

JavaScript arrays are more flexible than traditional arrays.

They are actually dynamic and can store mixed types:

```js
const arr = [1, "hello", true];
```

Internally, modern JS engines optimize arrays heavily and may use:

- contiguous memory for dense numeric arrays
- hash-map-like structures for sparse arrays

So conceptually arrays behave like arrays, but internally engines are more sophisticated.

---

# 9. Static vs Dynamic Arrays

| Feature       | Static Array | Dynamic Array            |
| ------------- | ------------ | ------------------------ |
| Size          | Fixed        | Resizable                |
| Memory        | Contiguous   | Usually contiguous       |
| Insert at end | Limited      | Amortized O(1)           |
| Example       | C array      | Java ArrayList, JS Array |

Dynamic arrays resize by allocating a larger block and copying elements.

---

# 10. Interview Tips

Interviewers usually expect these key points:

✅ Arrays store elements in contiguous memory
✅ Direct indexing is possible via address calculation
✅ Random access is O(1)
✅ Insertions/deletions in middle are O(n)
✅ Arrays are cache friendly
✅ Dynamic arrays resize by copying to larger memory blocks

---

# Short Interview Answer

> An array is a linear data structure that stores elements in contiguous memory locations. Each element is accessed using an index, and the memory address is calculated using:
>
> `base_address + index × size_of_element`
>
> Because of direct address calculation, accessing elements is O(1). However, insertions and deletions in the middle are expensive because elements must be shifted.

## Question 2. Difference between static and dynamic arrays

A **static array** has a **fixed size** decided at allocation time, while a **dynamic array** can **grow or shrink automatically** during runtime.

---

# 1. Quick Comparison

| Feature           | Static Array                    | Dynamic Array                                             |
| ----------------- | ------------------------------- | --------------------------------------------------------- |
| Size              | Fixed                           | Resizable                                                 |
| Memory Allocation | Compile time / fixed allocation | Runtime                                                   |
| Memory Layout     | Contiguous                      | Usually contiguous                                        |
| Resize Possible   | ❌ No                           | ✅ Yes                                                    |
| Insert at End     | Limited                         | Amortized O(1)                                            |
| Memory Efficiency | Can waste memory                | Better flexibility                                        |
| Performance       | Slightly faster                 | Slight overhead                                           |
| Examples          | C arrays                        | JavaScript Array, Java ArrayList, Python list, C++ vector |

---

# 2. Static Array

A static array has a predefined size.

Example in C:

```c id="g2dc0c"
int arr[5];
```

This creates space for exactly 5 integers.

---

## Memory Representation

```text id="c1fl7i"
arr = [10, 20, 30, 40, 50]

┌────┬────┬────┬────┬────┐
│ 10 │ 20 │ 30 │ 40 │ 50 │
└────┴────┴────┴────┴────┘
```

The size cannot change after allocation.

---

## Characteristics

### Advantages

- Very fast access
- No resizing overhead
- Better performance predictability
- Cache friendly

### Disadvantages

- Fixed capacity
- Memory wastage possible
- Cannot grow when full

---

## Example Problem

Suppose size is 5:

```text id="wuw2sp"
[10, 20, 30, _, _]
```

If more than 5 elements are needed:

```text id="v8p4cx"
❌ Cannot expand
```

You must create a new array manually.

---

# 3. Dynamic Array

A dynamic array automatically resizes when capacity is exceeded.

Examples:

- JavaScript `Array`
- Python `list`
- Java `ArrayList`
- C++ `vector`

Example in JavaScript:

```js id="6r0p3d"
const arr = [];

arr.push(10);
arr.push(20);
arr.push(30);
```

The array grows automatically.

---

# 4. How Dynamic Arrays Work Internally

Dynamic arrays still try to maintain:

```text id="o4p7ca"
Contiguous memory
```

When full:

1. Allocate a larger block
2. Copy old elements
3. Insert new element
4. Delete old block

---

## Example Resize

Initial capacity = 4

```text id="nh7rvx"
[10, 20, 30, 40]
```

Insert another element:

```text id="m5n0w4"
Need bigger memory
```

New capacity might become 8:

```text id="2pxq6g"
[10, 20, 30, 40, 50]
```

Elements are copied into the new block.

---

# 5. Dynamic Array Growth Strategy

Most implementations grow by:

```text id="ih3r8u"
New Capacity = Old Capacity × 2
```

Example:

```text id="ykh2ks"
1 → 2 → 4 → 8 → 16
```

This keeps append operations efficient.

---

# 6. Time Complexity Comparison

| Operation            | Static Array         | Dynamic Array      |
| -------------------- | -------------------- | ------------------ |
| Access               | O(1)                 | O(1)               |
| Update               | O(1)                 | O(1)               |
| Append               | O(1) if space exists | Amortized O(1)     |
| Insert/Delete Middle | O(n)                 | O(n)               |
| Resize               | ❌ Not possible      | O(n) during resize |

---

# 7. Why Dynamic Array Append Is Amortized O(1)

Most insertions do NOT trigger resizing.

Occasional resize takes O(n), but it happens infrequently.

So average insertion cost becomes:

```text id="7qgj8v"
Amortized O(1)
```

---

# 8. Memory Allocation Difference

## Static Array

Memory allocated once:

```text id="im8u4w"
┌────┬────┬────┬────┐
│    Fixed Block   │
└────┴────┴────┴────┘
```

---

## Dynamic Array

May relocate during resizing:

```text id="o4yv0w"
Old Block → New Larger Block
```

This relocation is invisible to the programmer.

---

# 9. JavaScript Perspective

JavaScript arrays are dynamic arrays.

Example:

```js id="9w4p0w"
const arr = [];

for (let i = 0; i < 1000; i++) {
  arr.push(i);
}
```

The engine automatically resizes internally.

However, JS arrays are more advanced than traditional dynamic arrays because they can also:

- store mixed types
- become sparse
- optimize differently internally

---

# 10. When to Use Which

## Use Static Arrays When

- Size is known beforehand
- Performance is critical
- Memory usage must be predictable
- Working in low-level systems

Examples:

- Embedded systems
- Fixed buffers

---

## Use Dynamic Arrays When

- Size changes frequently
- Ease of use matters
- Building general applications

Examples:

- Most application development
- Data collections
- APIs

---

# 11. Common Interview Follow-Ups

Interviewers often ask:

### Why is dynamic array append amortized O(1)?

Because resizing happens occasionally, not on every insertion.

---

### Why are arrays cache friendly?

Because elements are stored contiguously.

---

### Why are insertions expensive?

Because elements must shift.

---

### What happens during resizing?

A new larger contiguous block is allocated and elements are copied.

---

# 12. Interview-Friendly Summary

> A static array has a fixed size and cannot grow after allocation, while a dynamic array resizes automatically during runtime. Both usually store elements contiguously in memory and provide O(1) indexing. Dynamic arrays achieve flexible growth by allocating a larger block and copying elements when capacity is exceeded, making append operations amortized O(1).

## Question 3. How to find the maximum and minimum element in an array?

## Question 4. Reverse an array in-place

## Question 5. Rotate an array by k positions

## Question 6. Find the sum of all elements in an array

## Question 7. Find the second largest element in an array

## Question 8. How to remove duplicates from a sorted array?

## Question 9. Count the frequency of elements in an array

## Question 10. Find the subarray with the maximum sum (Kadane's algorithm)

## Question 11. How do you reverse a string without using library functions?

## Question 12. Check if a string is a palindrome

## Question 13. Find the first non-repeating character in a string

## Question 14. Count the number of vowels and consonants in a string

## Question 15. How do you implement strStr() (substring search)?

## Question 16. What is a linked list? Difference from arrays

## Question 17. Implement a singly linked list

## Question 18. Implement a doubly linked list

## Question 19. Reverse a linked list iteratively

## Question 20. Reverse a linked list recursively
