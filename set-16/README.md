# Set 16

| S.No. | Question                                                                                                                                                                                 |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Find all pairs in an array whose sum is a perfect square](#question-1-find-all-pairs-in-an-array-whose-sum-is-a-perfect-square)                                                         |
| 2.    | [Count number of subarrays with sum less than K](#question-2-count-number-of-subarrays-with-sum-less-than-k)                                                                             |
| 3.    | [Find equilibrium index of an array](#question-3-find-equilibrium-index-of-an-array)                                                                                                     |
| 4.    | [Maximum difference between two elements such that larger element comes after smaller](#question-4-maximum-difference-between-two-elements-such-that-larger-element-comes-after-smaller) |
| 5.    | [Find subarray with given XOR](#question-5-find-subarray-with-given-xor)                                                                                                                 |
| 6.    | [Maximum sum of two non-overlapping subarrays](#question-6-maximum-sum-of-two-non-overlapping-subarrays)                                                                                 |
| 7.    | [Maximum product subarray (including negative numbers)](#question-7-maximum-product-subarray-including-negative-numbers)                                                                 |
| 8.    | [Smallest subarray with sum greater than S](#question-8-smallest-subarray-with-sum-greater-than-s)                                                                                       |
| 9.    | [Count subarrays with exactly K distinct integers](#question-9-count-subarrays-with-exactly-k-distinct-integers)                                                                         |
| 10.   | [Find duplicate numbers in an array of size n+1 using Floyd's algorithm](#question-10-find-duplicate-numbers-in-an-array-of-size-n1-using-floyds-algorithm)                              |
| 11.   | [Check if a string can be segmented into words from dictionary](#question-11-check-if-a-string-can-be-segmented-into-words-from-dictionary)                                              |
| 12.   | [Minimum number of deletions to make two strings anagrams](#question-12-minimum-number-of-deletions-to-make-two-strings-anagrams)                                                        |
| 13.   | [Implement shortest palindrome by adding characters at front](#question-13-implement-shortest-palindrome-by-adding-characters-at-front)                                                  |
| 14.   | [Remove adjacent duplicates in a string repeatedly](#question-14-remove-adjacent-duplicates-in-a-string-repeatedly)                                                                      |
| 15.   | [Check if two strings are isomorphic](#question-15-check-if-two-strings-are-isomorphic)                                                                                                  |
| 16.   | [Encode and decode a string (like "aaabbbccc" → "a3b3c3")](#question-16-encode-and-decode-a-string-like-aaabbbccc--a3b3c3)                                                               |
| 17.   | [Longest substring with at most K distinct characters](#question-17-longest-substring-with-at-most-k-distinct-characters)                                                                |
| 18.   | [Find all palindromic substrings](#question-18-find-all-palindromic-substrings)                                                                                                          |
| 19.   | [Longest common prefix among strings](#question-19-longest-common-prefix-among-strings)                                                                                                  |
| 20.   | [Count number of substrings containing exactly K distinct characters](#question-20-count-number-of-substrings-containing-exactly-k-distinct-characters)                                  |

## Question 1. Find all pairs in an array whose sum is a perfect square

# Find All Pairs in an Array Whose Sum Is a Perfect Square

## Direct Answer

Given an array, find all pairs `(arr[i], arr[j])` where `i < j` and the sum `arr[i] + arr[j]` is a perfect square.

Example:

```js
Input: [1, 3, 5, 6, 7]

Pairs:
(1, 3) => 4  ✓
(3, 6) => 9  ✓
(1, 8) => 9  ✓ (if 8 existed)

Output: [[1, 3], [3, 6]]
```

---

# 1. Problem Understanding

A number is a **perfect square** if it can be written as:

```text
1, 4, 9, 16, 25, 36, ...
```

For every pair in the array:

1. Compute the sum.
2. Check if the sum is a perfect square.
3. If yes, include the pair in the result.

---

# 2. Approach 1: Brute Force (Nested Loops)

### Idea

Check every possible pair.

For each pair:

```js
sum = arr[i] + arr[j];
```

Determine whether `sum` is a perfect square.

### Perfect Square Check

```js
const isPerfectSquare = (num) => {
  const root = Math.sqrt(num);
  return root === Math.floor(root);
};
```

### JavaScript Code

```js
const isPerfectSquare = (num) => {
  const root = Math.sqrt(num);
  return root === Math.floor(root);
};

const findPerfectSquarePairs = (arr) => {
  const result = [];

  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      const sum = arr[i] + arr[j];

      if (isPerfectSquare(sum)) {
        result.push([arr[i], arr[j]]);
      }
    }
  }

  return result;
};

// Example
console.log(findPerfectSquarePairs([1, 3, 5, 6, 7]));
```

### Output

```js
[
  [1, 3],
  [3, 6],
];
```

### Complexity

- Time: **O(n²)**
- Space: **O(1)** (excluding output)

---

# 3. Approach 2: Using Precomputed Perfect Squares

### Idea

Instead of calling `Math.sqrt()` for every pair:

1. Find the maximum possible pair sum.
2. Generate all perfect squares up to that value.
3. Store them in a `Set`.
4. Check pair sums using O(1) lookup.

---

### JavaScript Code

```js
const findPerfectSquarePairs = (arr) => {
  const result = [];

  const maxVal = Math.max(...arr);
  const maxSum = maxVal * 2;

  const squares = new Set();

  for (let i = 0; i * i <= maxSum; i++) {
    squares.add(i * i);
  }

  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      const sum = arr[i] + arr[j];

      if (squares.has(sum)) {
        result.push([arr[i], arr[j]]);
      }
    }
  }

  return result;
};

console.log(findPerfectSquarePairs([1, 3, 5, 6, 7]));
```

### Complexity

- Time: **O(n²)**
- Space: **O(√M)**

where `M` is the maximum possible pair sum.

---

# 4. Optimized Approach (Hashing)

If the goal is to avoid checking every pair explicitly:

### Idea

For each element `x`:

1. Generate possible perfect squares.
2. Compute:

```text
needed = square - x
```

3. If `needed` already exists in the hash map, a valid pair is found.

---

### JavaScript Code

```js
const findPerfectSquarePairs = (arr) => {
  const result = [];
  const seen = new Map();

  const maxVal = Math.max(...arr);
  const maxSum = maxVal * 2;

  const squares = [];

  for (let i = 0; i * i <= maxSum; i++) {
    squares.push(i * i);
  }

  for (const num of arr) {
    for (const square of squares) {
      const needed = square - num;

      if (seen.has(needed)) {
        const count = seen.get(needed);

        for (let k = 0; k < count; k++) {
          result.push([needed, num]);
        }
      }
    }

    seen.set(num, (seen.get(num) || 0) + 1);
  }

  return result;
};

console.log(findPerfectSquarePairs([1, 3, 5, 6, 7]));
```

### Complexity

- Time: **O(n√M)**
- Space: **O(n)**

This can be better than O(n²) when the range of values is relatively small compared to `n`.

---

# Example Walkthrough

```js
arr = [1, 3, 5, 6, 7];
```

Pairs:

| Pair  | Sum | Perfect Square? |
| ----- | --- | --------------- |
| (1,3) | 4   | ✓               |
| (1,5) | 6   | ✗               |
| (1,6) | 7   | ✗               |
| (1,7) | 8   | ✗               |
| (3,5) | 8   | ✗               |
| (3,6) | 9   | ✓               |
| (3,7) | 10  | ✗               |
| (5,6) | 11  | ✗               |
| (5,7) | 12  | ✗               |
| (6,7) | 13  | ✗               |

Result:

```js
[
  [1, 3],
  [3, 6],
];
```

---

# Edge Cases

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
[];
```

### Duplicate Elements

```js
[2, 2, 2];
```

Pairs:

```text
2 + 2 = 4
```

Output:

```js
[
  [2, 2],
  [2, 2],
  [2, 2],
];
```

### Negative Numbers

```js
[-1, 2];
```

Sum:

```text
1
```

which is a perfect square.

---

# Interview Tips

- Start with the **O(n²)** brute-force solution since every pair may need to be reported.
- Mention that if only **counting** pairs is required, hashing can improve performance.
- Use a helper function to check perfect squares cleanly.
- Clarify whether duplicate pairs and repeated values should be included.
- If the array size is small, the brute-force solution is often the most practical and easiest to explain in interviews.

## Question 2. Count number of subarrays with sum less than K

# Count Number of Subarrays with Sum Less Than K

## Direct Answer

Given an array and an integer `K`, count the number of contiguous subarrays whose sum is **strictly less than `K`**.

Example:

```js
arr = [1, 2, 3]
K = 5

Valid subarrays:
[1]      => 1
[1,2]    => 3
[2]      => 2
[2,3]    => 5 ❌
[3]      => 3
[1,2,3]  => 6 ❌

Answer = 4
```

---

# 1. Problem Understanding

We need to count all contiguous subarrays:

```text
arr[i...j]
```

such that:

```text
sum(arr[i...j]) < K
```

The approach depends on whether the array contains only positive numbers or can contain negative numbers.

---

# 2. Approach 1: Brute Force

### Idea

Generate every possible subarray and calculate its sum.

### JavaScript Code

```js
const countSubarraysLessThanK = (arr, k) => {
  let count = 0;

  for (let i = 0; i < arr.length; i++) {
    let sum = 0;

    for (let j = i; j < arr.length; j++) {
      sum += arr[j];

      if (sum < k) {
        count++;
      }
    }
  }

  return count;
};
```

### Complexity

- Time: **O(n²)**
- Space: **O(1)**

---

# 3. Approach 2: Sliding Window (Optimal for Positive Numbers)

### Important Observation

This approach works **only when all array elements are positive**.

Since all numbers are positive:

- Expanding the window increases the sum.
- Shrinking the window decreases the sum.

This monotonic behavior enables a two-pointer solution.

---

## Idea

Maintain a window `[left, right]`.

For each `right`:

1. Add `arr[right]` to the sum.
2. While sum ≥ K, move `left`.
3. All subarrays ending at `right` and starting between `left` and `right` are valid.

Number of such subarrays:

```text
right - left + 1
```

---

## Example

```js
arr = [1, 2, 3];
K = 5;
```

### right = 0

```text
sum = 1

Valid:
[1]

count += 1
```

### right = 1

```text
sum = 3

Valid:
[2]
[1,2]

count += 2
```

### right = 2

```text
sum = 6

Shrink:
remove 1

sum = 5

Still >= 5
remove 2

sum = 3

Valid:
[3]

count += 1
```

Total:

```text
1 + 2 + 1 = 4
```

---

## JavaScript Code

```js
const countSubarraysLessThanK = (arr, k) => {
  let left = 0;
  let sum = 0;
  let count = 0;

  for (let right = 0; right < arr.length; right++) {
    sum += arr[right];

    while (left <= right && sum >= k) {
      sum -= arr[left];
      left++;
    }

    count += right - left + 1;
  }

  return count;
};
```

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

# 4. Why Sliding Window Fails with Negative Numbers

Consider:

```js
arr = [4, -2, 1];
K = 4;
```

Adding a new element may:

- Increase the sum
- Decrease the sum

So the window sum is no longer monotonic.

The key property required for sliding window breaks.

---

# 5. General Case (Negative Numbers Allowed)

For arrays containing negative values, we use:

### Prefix Sum + Ordered Data Structure

We need to count pairs:

```text
prefix[j] - prefix[i] < K
```

Rearrange:

```text
prefix[i] > prefix[j] - K
```

This can be solved using:

- Balanced BST
- Fenwick Tree + Coordinate Compression
- Merge Sort based counting

### Complexity

```text
O(n log n)
```

This is the standard solution when negative numbers are present.

---

# JavaScript Solution (Positive Numbers - Interview Favorite)

```js
const countSubarraysLessThanK = (arr, k) => {
  let left = 0;
  let sum = 0;
  let count = 0;

  for (let right = 0; right < arr.length; right++) {
    sum += arr[right];

    while (left <= right && sum >= k) {
      sum -= arr[left++];
    }

    count += right - left + 1;
  }

  return count;
};

// Example
console.log(countSubarraysLessThanK([1, 2, 3], 5)); // 4
```

---

# Complexity Analysis

| Approach                 | Time       | Space | Notes                 |
| ------------------------ | ---------- | ----- | --------------------- |
| Brute Force              | O(n²)      | O(1)  | Works for all arrays  |
| Sliding Window           | O(n)       | O(1)  | Only positive numbers |
| Prefix Sum + BST/Fenwick | O(n log n) | O(n)  | Handles negatives     |

---

# Edge Cases

### Empty Array

```js
[];
```

Answer:

```js
0;
```

---

### K ≤ 0 (Positive Array)

```js
arr = [1, 2, 3];
K = 0;
```

No positive-sum subarray can be less than 0.

Answer:

```js
0;
```

---

### All Elements Smaller Than K

```js
arr = [1, 1, 1];
K = 10;
```

All subarrays are valid:

```text
n(n+1)/2 = 6
```

---

# Interview Tips

1. Ask whether the array contains only positive numbers.
2. If all numbers are positive, immediately propose the **O(n) sliding window** solution.
3. Explain why every valid window contributes:

```text
(right - left + 1)
```

subarrays.

4. Mention that the presence of negative numbers invalidates sliding window and requires an **O(n log n)** prefix-sum-based approach.

This distinction is a very common interview follow-up.

## Question 3. Find equilibrium index of an array

## Question 4. Maximum difference between two elements such that larger element comes after smaller

## Question 5. Find subarray with given XOR

## Question 6. Maximum sum of two non-overlapping subarrays

## Question 7. Maximum product subarray (including negative numbers)

## Question 8. Smallest subarray with sum greater than S

## Question 9. Count subarrays with exactly K distinct integers

## Question 10. Find duplicate numbers in an array of size n+1 using Floyd's algorithm

## Question 11. Check if a string can be segmented into words from dictionary

## Question 12. Minimum number of deletions to make two strings anagrams

## Question 13. Implement shortest palindrome by adding characters at front

## Question 14. Remove adjacent duplicates in a string repeatedly

## Question 15. Check if two strings are isomorphic

## Question 16. Encode and decode a string (like "aaabbbccc" → "a3b3c3")

## Question 17. Longest substring with at most K distinct characters

## Question 18. Find all palindromic substrings

## Question 19. Longest common prefix among strings

## Question 20. Count number of substrings containing exactly K distinct characters
