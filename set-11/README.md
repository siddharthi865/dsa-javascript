# Set 11

| S.No. | Question                                                                                                                                                 |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Find the maximum product of two elements in an array](#question-1-find-the-maximum-product-of-two-elements-in-an-array)                                 |
| 2.    | [Find the maximum sum of contiguous subarray of size k](#question-2-find-the-maximum-sum-of-contiguous-subarray-of-size-k)                               |
| 3.    | [Check if an array can be partitioned into two subsets with equal sum](#question-3-check-if-an-array-can-be-partitioned-into-two-subsets-with-equal-sum) |
| 4.    | [Rotate a matrix by 90 degrees](#question-4-rotate-a-matrix-by-90-degrees)                                                                               |
| 5.    | [Find the leader elements in an array](#question-5-find-the-leader-elements-in-an-array)                                                                 |
| 6.    | [Maximum circular subarray sum](#question-6-maximum-circular-subarray-sum)                                                                               |
| 7.    | [Count pairs with given sum](#question-7-count-pairs-with-given-sum)                                                                                     |
| 8.    | [Find all triplets with zero sum](#question-8-find-all-triplets-with-zero-sum)                                                                           |
| 9.    | [Subarray with sum divisible by n](#question-9-subarray-with-sum-divisible-by-n)                                                                         |
| 10.   | [Find the maximum difference (j > i) in an array](#question-10-find-the-maximum-difference-j--i-in-an-array)                                             |
| 11.   | [Longest repeating character replacement problem](#question-11-longest-repeating-character-replacement-problem)                                          |
| 12.   | [Longest prefix which is also a suffix](#question-12-longest-prefix-which-is-also-a-suffix)                                                              |
| 13.   | [Implement Rabin-Karp algorithm for substring search](#question-13-implement-rabin-karp-algorithm-for-substring-search)                                  |
| 14.   | [Find all anagrams of a string in another string](#question-14-find-all-anagrams-of-a-string-in-another-string)                                          |
| 15.   | [Minimum window substring](#question-15-minimum-window-substring)                                                                                        |
| 16.   | [Implement palindrome partitioning](#question-16-implement-palindrome-partitioning)                                                                      |
| 17.   | [Count binary substrings with equal 0s and 1s](#question-17-count-binary-substrings-with-equal-0s-and-1s)                                                |
| 18.   | [Check if string can be made palindrome by removing one character](#question-18-check-if-string-can-be-made-palindrome-by-removing-one-character)        |
| 19.   | [Find all subsequences of a string](#question-19-find-all-subsequences-of-a-string)                                                                      |
| 20.   | [String matching using KMP algorithm](#question-20-string-matching-using-kmp-algorithm)                                                                  |

## Question 1. Find the maximum product of two elements in an array

# Find the Maximum Product of Two Elements in an Array

## Direct Answer

Given an array of integers, the maximum product can come from:

1. The **two largest positive numbers**, or
2. The **two smallest (most negative) numbers**, because a negative × negative = positive.

So the answer is:

[
\text{max}(largest1 \times largest2,; smallest1 \times smallest2)
]

---

# 1. Problem Understanding

Given an array:

```js
const arr = [1, 4, 3, 6, 7, 0];
```

The maximum product is:

```js
7 * 6 = 42
```

However, consider:

```js
const arr = [-10, -20, 5, 2];
```

Here:

```js
(-10) * (-20) = 200
```

which is larger than:

```js
5 * 2 = 10
```

So we must consider both possibilities.

---

# Approach 1: Sort the Array

## Idea

After sorting:

- Largest two numbers are at the end.
- Smallest two numbers are at the beginning.

Compute both products and return the larger one.

### JavaScript Code

```js
const maxProduct = (arr) => {
  if (arr.length < 2) {
    throw new Error("Array must contain at least two elements");
  }

  arr.sort((a, b) => a - b);

  const n = arr.length;

  const product1 = arr[n - 1] * arr[n - 2];
  const product2 = arr[0] * arr[1];

  return Math.max(product1, product2);
};
```

### Complexity Analysis

- Sorting: **O(n log n)**
- Space: **O(1)** (ignoring sorting implementation details)

---

# Approach 2: Single Traversal (Optimal)

## Idea

Track:

- Largest number
- Second largest number
- Smallest number
- Second smallest number

Then compare:

```js
largest1 * largest2;
```

and

```js
smallest1 * smallest2;
```

This avoids sorting.

---

## Algorithm

For every element:

Update:

```js
(max1, max2);
(min1, min2);
```

At the end:

```js
return Math.max(max1 * max2, min1 * min2);
```

---

### JavaScript Code

```js
const maxProduct = (arr) => {
  if (arr.length < 2) {
    throw new Error("Array must contain at least two elements");
  }

  let max1 = -Infinity;
  let max2 = -Infinity;

  let min1 = Infinity;
  let min2 = Infinity;

  for (const num of arr) {
    // Update largest two
    if (num > max1) {
      max2 = max1;
      max1 = num;
    } else if (num > max2) {
      max2 = num;
    }

    // Update smallest two
    if (num < min1) {
      min2 = min1;
      min1 = num;
    } else if (num < min2) {
      min2 = num;
    }
  }

  return Math.max(max1 * max2, min1 * min2);
};
```

---

## Example Walkthrough

```js
arr = [-10, -3, 5, 6, -20];
```

After traversal:

```js
max1 = 6;
max2 = 5;

min1 = -20;
min2 = -10;
```

Products:

```js
6 * 5 = 30

(-20) * (-10) = 200
```

Answer:

```js
200;
```

---

# Approach 3: Return the Pair as Well

Interviewers sometimes ask for the actual elements.

```js
const maxProductPair = (arr) => {
  let max1 = -Infinity;
  let max2 = -Infinity;
  let min1 = Infinity;
  let min2 = Infinity;

  for (const num of arr) {
    if (num > max1) {
      max2 = max1;
      max1 = num;
    } else if (num > max2) {
      max2 = num;
    }

    if (num < min1) {
      min2 = min1;
      min1 = num;
    } else if (num < min2) {
      min2 = num;
    }
  }

  const positiveProduct = max1 * max2;
  const negativeProduct = min1 * min2;

  return positiveProduct >= negativeProduct
    ? { pair: [max1, max2], product: positiveProduct }
    : { pair: [min1, min2], product: negativeProduct };
};
```

Example:

```js
maxProductPair([-10, -20, 5, 4]);

// {
//   pair: [-20, -10],
//   product: 200
// }
```

---

# Edge Cases

### 1. Only Two Elements

```js
[3, 5];
```

Answer:

```js
15;
```

---

### 2. All Negative Numbers

```js
[-1, -2, -3, -4];
```

Answer:

```js
(-4) * (-3) = 12
```

---

### 3. Contains Zeros

```js
[-10, 0, 2, 3];
```

Answer:

```js
2 * 3 = 6
```

---

### 4. Duplicate Values

```js
[5, 5, 5];
```

Answer:

```js
25;
```

---

# Interview Tips

When asked this question:

1. Mention that the maximum product is **not always formed by the two largest numbers**.
2. Give the counterexample:

```js
[-10, -20, 5, 2];
```

3. Explain that the answer must consider:
   - Two largest values
   - Two smallest values

4. Present the **O(n)** single-pass solution as the optimal approach.

### Optimal Complexity

| Metric | Value    |
| ------ | -------- |
| Time   | **O(n)** |
| Space  | **O(1)** |

This is the solution most interviewers expect.

## Question 2. Find the maximum sum of contiguous subarray of size k

# Find the Maximum Sum of Contiguous Subarray of Size K

## Direct Answer

This is a classic **Sliding Window** problem.

Instead of calculating the sum of every subarray of size `k` separately (`O(n*k)`), maintain a window of size `k` and slide it across the array, updating the sum in **O(1)** time for each move.

The optimal solution runs in:

- **Time:** `O(n)`
- **Space:** `O(1)`

---

# 1. Problem Understanding

Given an array and an integer `k`, find the **maximum sum among all contiguous subarrays of size `k`**.

### Example

```js
arr = [2, 1, 5, 1, 3, 2];
k = 3;
```

Subarrays of size 3:

```js
[2, 1, 5] => 8
[1, 5, 1] => 7
[5, 1, 3] => 9
[1, 3, 2] => 6
```

Maximum sum:

```js
9;
```

---

# Approach 1: Brute Force

## Idea

Generate every subarray of size `k`, compute its sum, and keep track of the maximum.

### JavaScript Code

```js
const maxSumSubarray = (arr, k) => {
  let maxSum = -Infinity;

  for (let i = 0; i <= arr.length - k; i++) {
    let currentSum = 0;

    for (let j = i; j < i + k; j++) {
      currentSum += arr[j];
    }

    maxSum = Math.max(maxSum, currentSum);
  }

  return maxSum;
};
```

### Complexity Analysis

- Time: **O(n × k)**
- Space: **O(1)**

---

# Approach 2: Sliding Window (Optimal)

## Idea

1. Compute the sum of the first `k` elements.
2. Store it as the current maximum.
3. Slide the window one element at a time:
   - Remove the outgoing element.
   - Add the incoming element.

4. Update the maximum sum.

### Visualization

```js
arr = [2, 1, 5, 1, 3, 2];
k = 3;
```

Initial window:

```js
[2, 1, 5] => 8
```

Move right:

```js
8 - 2 + 1 = 7
```

Move right:

```js
7 - 1 + 3 = 9
```

Move right:

```js
9 - 5 + 2 = 6
```

Maximum = `9`

---

## JavaScript Code

```js
const maxSumSubarray = (arr, k) => {
  const n = arr.length;

  if (k > n) {
    throw new Error("k cannot be greater than array length");
  }

  let windowSum = 0;

  // First window
  for (let i = 0; i < k; i++) {
    windowSum += arr[i];
  }

  let maxSum = windowSum;

  // Slide the window
  for (let i = k; i < n; i++) {
    windowSum = windowSum - arr[i - k] + arr[i];
    maxSum = Math.max(maxSum, windowSum);
  }

  return maxSum;
};
```

---

# Example Walkthrough

```js
arr = [4, 2, 1, 7, 8, 1, 2, 8, 1, 0];
k = 3;
```

First window:

```js
4 + 2 + 1 = 7
```

Slide:

```js
7 - 4 + 7 = 10
10 - 2 + 8 = 16
16 - 1 + 1 = 16
16 - 7 + 2 = 11
11 - 8 + 8 = 11
11 - 1 + 1 = 11
11 - 2 + 0 = 9
```

Maximum:

```js
16;
```

---

# Complexity Analysis

### Sliding Window

| Metric | Complexity |
| ------ | ---------- |
| Time   | **O(n)**   |
| Space  | **O(1)**   |

---

# Edge Cases

### 1. k = 1

```js
arr = [3, 7, 2];
```

Answer:

```js
7;
```

---

### 2. k = n

```js
arr = [1, 2, 3];
k = 3;
```

Answer:

```js
6;
```

---

### 3. All Negative Numbers

```js
arr = [-4, -2, -7, -1];
k = 2;
```

Subarrays:

```js
[-4, -2] => -6
[-2, -7] => -9
[-7, -1] => -8
```

Answer:

```js
-6;
```

The sliding window approach works correctly even with negative numbers.

---

### 4. Invalid k

```js
k > arr.length;
```

Handle by throwing an error or returning a special value depending on requirements.

---

# Interview Tips

### Follow-up Questions Interviewers Ask

**1. Why use Sliding Window?**

Because consecutive subarrays overlap heavily. Instead of recomputing the entire sum, we reuse the previous window's sum.

---

**2. Can it be solved in O(n)?**

Yes, Sliding Window achieves `O(n)` time.

---

**3. What if the window size is variable instead of fixed?**

That becomes a Variable Sliding Window problem (e.g., longest substring with at most K distinct characters).

---

### Optimal Solution Summary

```js
const maxSumSubarray = (arr, k) => {
  let windowSum = 0;

  for (let i = 0; i < k; i++) {
    windowSum += arr[i];
  }

  let maxSum = windowSum;

  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k];
    maxSum = Math.max(maxSum, windowSum);
  }

  return maxSum;
};
```

**Time Complexity:** `O(n)`
**Space Complexity:** `O(1)`

This is the standard interview-expected solution.

## Question 3. Check if an array can be partitioned into two subsets with equal sum

## Question 4. Rotate a matrix by 90 degrees

## Question 5. Find the leader elements in an array

## Question 6. Maximum circular subarray sum

## Question 7. Count pairs with given sum

## Question 8. Find all triplets with zero sum

## Question 9. Subarray with sum divisible by n

## Question 10. Find the maximum difference (j > i) in an array

## Question 11. Longest repeating character replacement problem

## Question 12. Longest prefix which is also a suffix

## Question 13. Implement Rabin-Karp algorithm for substring search

## Question 14. Find all anagrams of a string in another string

## Question 15. Minimum window substring

## Question 16. Implement palindrome partitioning

## Question 17. Count binary substrings with equal 0s and 1s

## Question 18. Check if string can be made palindrome by removing one character

## Question 19. Find all subsequences of a string

## Question 20. String matching using KMP algorithm
