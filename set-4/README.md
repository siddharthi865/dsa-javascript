# Set 4

| S.No. | Question                                                                                                           |
| ----- | ------------------------------------------------------------------------------------------------------------------ |
| 1.    | [Implement selection sort](#question-1-implement-selection-sort)                                                   |
| 2.    | [Implement merge sort](#question-2-implement-merge-sort)                                                           |
| 3.    | [Implement quicksort](#question-3-implement-quicksort)                                                             |
| 4.    | [Count inversions in an array](#question-4-count-inversions-in-an-array)                                           |
| 5.    | [Find kth largest element in an array](#question-5-find-kth-largest-element-in-an-array)                           |
| 6.    | [Implement a HashMap from scratch](#question-6-implement-a-hashmap-from-scratch)                                   |
| 7.    | [Count frequency of elements using a hash map](#question-7-count-frequency-of-elements-using-a-hash-map)           |
| 8.    | [Find two numbers that sum to a target](#question-8-find-two-numbers-that-sum-to-a-target)                         |
| 9.    | [Find the longest consecutive sequence in an array](#question-9-find-the-longest-consecutive-sequence-in-an-array) |
| 10.   | [Check if a subarray with sum zero exists](#question-10-check-if-a-subarray-with-sum-zero-exists)                  |
| 11.   | [Fibonacci number (recursive and DP)](#question-11-fibonacci-number-recursive-and-dp)                              |
| 12.   | [Climbing stairs problem](#question-12-climbing-stairs-problem)                                                    |
| 13.   | [Coin change problem (minimum coins)](#question-13-coin-change-problem-minimum-coins)                              |
| 14.   | [Longest common subsequence (LCS)](#question-14-longest-common-subsequence-lcs)                                    |
| 15.   | [Longest increasing subsequence (LIS)](#question-15-longest-increasing-subsequence-lis)                            |
| 16.   | [Maximum sum subarray with no adjacent elements](#question-16-maximum-sum-subarray-with-no-adjacent-elements)      |
| 17.   | [0/1 Knapsack problem](#question-17-01-knapsack-problem)                                                           |
| 18.   | [Edit distance between two strings](#question-18-edit-distance-between-two-strings)                                |
| 19.   | [Matrix chain multiplication problem](#question-19-matrix-chain-multiplication-problem)                            |
| 20.   | [Minimum path sum in a grid](#question-20-minimum-path-sum-in-a-grid)                                              |

## Question 1. Implement selection sort

# Selection Sort

## Direct Answer

Selection Sort is a simple comparison-based sorting algorithm that repeatedly finds the smallest element from the unsorted portion of the array and places it at its correct position.

### Example

Input:

```js
[64, 25, 12, 22, 11];
```

Passes:

```text
[11, 25, 12, 22, 64]
[11, 12, 25, 22, 64]
[11, 12, 22, 25, 64]
[11, 12, 22, 25, 64]
```

Output:

```js
[11, 12, 22, 25, 64];
```

---

# 1. Problem Understanding

Given an array, sort it in ascending order using Selection Sort.

The key idea:

- Divide the array into:
  - Sorted portion (left side)
  - Unsorted portion (right side)

- Find the minimum element in the unsorted portion.
- Swap it with the first element of the unsorted portion.
- Repeat until the array is sorted.

---

# 2. Approach

### Algorithm

For each position `i`:

1. Assume `i` contains the minimum element.
2. Traverse the remaining array.
3. Find the actual minimum element.
4. Swap it with `arr[i]`.
5. Move to the next position.

---

# 3. JavaScript Implementation

```js
const selectionSort = (arr) => {
  const n = arr.length;

  for (let i = 0; i < n - 1; i++) {
    let minIndex = i;

    for (let j = i + 1; j < n; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }

    [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
  }

  return arr;
};

// Example
const arr = [64, 25, 12, 22, 11];
console.log(selectionSort(arr));
```

Output:

```js
[11, 12, 22, 25, 64];
```

---

# 4. Optimized Version (Avoid Unnecessary Swaps)

If the minimum element is already at the correct position, we can skip the swap.

```js
const selectionSort = (arr) => {
  const n = arr.length;

  for (let i = 0; i < n - 1; i++) {
    let minIndex = i;

    for (let j = i + 1; j < n; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }

    if (minIndex !== i) {
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
    }
  }

  return arr;
};
```

---

# 5. Complexity Analysis

| Operation        | Complexity |
| ---------------- | ---------- |
| Best Case        | O(n²)      |
| Average Case     | O(n²)      |
| Worst Case       | O(n²)      |
| Space Complexity | O(1)       |

### Why O(n²)?

Number of comparisons:

```text
(n-1) + (n-2) + ... + 1

= n(n-1)/2
= O(n²)
```

Unlike Bubble Sort, Selection Sort performs the same number of comparisons regardless of input order.

---

# 6. Edge Cases

### Empty Array

```js
[];
```

Output:

```js
[];
```

### Single Element

```js
[5];
```

Output:

```js
[5];
```

### Already Sorted

```js
[1, 2, 3, 4];
```

Still performs O(n²) comparisons.

### Duplicate Elements

```js
[4, 2, 4, 1];
```

Output:

```js
[1, 2, 4, 4];
```

---

# 7. Common Pitfalls

### Forgetting to Update `minIndex`

Incorrect:

```js
if (arr[j] < arr[i])
```

Correct:

```js
if (arr[j] < arr[minIndex])
```

---

### Swapping Inside Inner Loop

Wrong:

```js
for (...) {
    if (...) {
        swap();
    }
}
```

Selection Sort should swap **once per outer iteration**, after the minimum element is found.

---

# 8. Interview Tips

- Selection Sort is **in-place** (`O(1)` extra space).
- It minimizes the number of swaps:
  - At most `n - 1` swaps.

- It is **not stable** by default.
- It is generally used for educational purposes and small datasets.
- Compared to Bubble Sort:
  - Same time complexity: `O(n²)`
  - Fewer swaps
  - Often performs slightly better in practice.

### Interview Summary

> Selection Sort repeatedly selects the smallest element from the unsorted portion of the array and places it at its correct position. It runs in `O(n²)` time, uses `O(1)` extra space, performs at most `n−1` swaps, and is not stable in its standard implementation.

## Question 2. Implement merge sort

# Merge Sort

## Direct Answer

Merge Sort is a **divide-and-conquer sorting algorithm** that splits the array into halves, recursively sorts them, and then merges the sorted halves.

It has a guaranteed time complexity of **O(n log n)** and is stable.

---

# 1. Problem Understanding

Given an array, sort it in ascending order using Merge Sort.

Key idea:

- Divide array into two halves
- Recursively sort each half
- Merge the sorted halves into a single sorted array

---

# 2. Approach 1: Recursive Merge Sort (Standard)

### Idea

1. If array has 0 or 1 element → already sorted
2. Split array into two halves
3. Recursively sort both halves
4. Merge them using a helper function

---

## Merge Logic (Core Step)

We maintain two pointers:

- `i` → left array
- `j` → right array

Pick smaller element each time.

---

# 3. JavaScript Implementation

```js
const mergeSort = (arr) => {
  if (arr.length <= 1) return arr;

  const mid = Math.floor(arr.length / 2);

  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));

  return merge(left, right);
};

const merge = (left, right) => {
  let i = 0;
  let j = 0;
  const result = [];

  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      result.push(left[i]);
      i++;
    } else {
      result.push(right[j]);
      j++;
    }
  }

  // Add remaining elements
  while (i < left.length) result.push(left[i++]);
  while (j < right.length) result.push(right[j++]);

  return result;
};

// Example
const arr = [38, 27, 43, 3, 9, 82, 10];
console.log(mergeSort(arr));
```

---

# 4. Approach 2: Optimized (Using Auxiliary Array)

This avoids repeated slicing (which creates extra arrays).

### Idea:

- Use a single temporary array
- Work with index ranges

---

## JavaScript Implementation

```js
const mergeSort = (arr) => {
  const temp = new Array(arr.length);
  divide(arr, temp, 0, arr.length - 1);
  return arr;
};

const divide = (arr, temp, left, right) => {
  if (left >= right) return;

  const mid = Math.floor((left + right) / 2);

  divide(arr, temp, left, mid);
  divide(arr, temp, mid + 1, right);

  merge(arr, temp, left, mid, right);
};

const merge = (arr, temp, left, mid, right) => {
  let i = left;
  let j = mid + 1;
  let k = left;

  while (i <= mid && j <= right) {
    if (arr[i] <= arr[j]) {
      temp[k++] = arr[i++];
    } else {
      temp[k++] = arr[j++];
    }
  }

  while (i <= mid) temp[k++] = arr[i++];
  while (j <= right) temp[k++] = arr[j++];

  for (let idx = left; idx <= right; idx++) {
    arr[idx] = temp[idx];
  }
};

const arr2 = [38, 27, 43, 3, 9, 82, 10];
console.log(mergeSort(arr2));
```

---

# 5. Complexity Analysis

| Case         | Complexity |
| ------------ | ---------- |
| Best Case    | O(n log n) |
| Average Case | O(n log n) |
| Worst Case   | O(n log n) |

### Space Complexity

- O(n) (due to temporary arrays)

---

### Why O(n log n)?

- Array is divided into log n levels
- Each level takes O(n) time to merge

So:

```text
O(n) × O(log n) = O(n log n)
```

---

# 6. Edge Cases

### Empty Array

```js
[];
```

### Single Element

```js
[5];
```

### Already Sorted

```js
[1, 2, 3, 4];
```

### Reverse Sorted

```js
[9, 7, 5, 3, 1];
```

Merge sort handles all cases efficiently.

---

# 7. Common Pitfalls

### 1. Forgetting merge step correctness

- Must handle leftover elements

### 2. Using slice-heavy implementation in interviews

- Interviewers often prefer index-based approach

### 3. Incorrect mid calculation

```js
Math.floor((l + r) / 2);
```

---

# 8. Advantages & Disadvantages

## Advantages

- Stable sort
- Predictable O(n log n)
- Works well for linked lists
- Good for large datasets

## Disadvantages

- Requires O(n) extra space
- Slower than quicksort in practice due to memory overhead

---

# 9. Interview Tips

- Always mention both implementations:
  - Simple recursive (easy to explain)
  - Optimized index-based (preferred in interviews)

- Emphasize:
  - Divide and conquer
  - Stability
  - Guaranteed O(n log n)

---

## Interview Summary

> Merge Sort is a divide-and-conquer algorithm that splits the array into halves, recursively sorts them, and merges sorted halves. It runs in O(n log n) time in all cases and requires O(n) extra space, making it stable and predictable but memory-intensive.

## Question 3. Implement quicksort

## Question 4. Count inversions in an array

## Question 5. Find kth largest element in an array

## Question 6. Implement a HashMap from scratch

## Question 7. Count frequency of elements using a hash map

## Question 8. Find two numbers that sum to a target

## Question 9. Find the longest consecutive sequence in an array

## Question 10. Check if a subarray with sum zero exists

## Question 11. Fibonacci number (recursive and DP)

## Question 12. Climbing stairs problem

## Question 13. Coin change problem (minimum coins)

## Question 14. Longest common subsequence (LCS)

## Question 15. Longest increasing subsequence (LIS)

## Question 16. Maximum sum subarray with no adjacent elements

## Question 17. 0/1 Knapsack problem

## Question 18. Edit distance between two strings

## Question 19. Matrix chain multiplication problem

## Question 20. Minimum path sum in a grid
