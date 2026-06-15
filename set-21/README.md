# Set 21

| S.No. | Question                                                                                                                                                          |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Count all subarrays where the sum is divisible by a given number](#question-1-count-all-subarrays-where-the-sum-is-divisible-by-a-given-number)                  |
| 2.    | [Maximum sum subarray with exactly K elements](#question-2-maximum-sum-subarray-with-exactly-k-elements)                                                          |
| 3.    | [Find smallest positive missing number in unsorted array](#question-3-find-smallest-positive-missing-number-in-unsorted-array)                                    |
| 4.    | [Find two elements whose sum is closest to zero](#question-4-find-two-elements-whose-sum-is-closest-to-zero)                                                      |
| 5.    | [Find maximum sum path in two sorted arrays (merge-like problem)](#question-5-find-maximum-sum-path-in-two-sorted-arrays-merge-like-problem)                      |
| 6.    | [Check if an array is a mountain array](#question-6-check-if-an-array-is-a-mountain-array)                                                                        |
| 7.    | [Find the maximum sum of i\*arr[i] after rotations](#question-7-find-the-maximum-sum-of-iarri-after-rotations)                                                    |
| 8.    | [Count all inversions in an array using BIT or segment tree](#question-8-count-all-inversions-in-an-array-using-bit-or-segment-tree)                              |
| 9.    | [Count number of subarrays with exactly K odd numbers](#question-9-count-number-of-subarrays-with-exactly-k-odd-numbers)                                          |
| 10.   | [Rearrange array elements so that even and odd numbers alternate](#question-10-rearrange-array-elements-so-that-even-and-odd-numbers-alternate)                   |
| 11.   | [Minimum insertions to make a string palindrome](#question-11-minimum-insertions-to-make-a-string-palindrome)                                                     |
| 12.   | [Count number of distinct subsequences](#question-12-count-number-of-distinct-subsequences)                                                                       |
| 13.   | [Longest substring with at most two distinct characters](#question-13-longest-substring-with-at-most-two-distinct-characters)                                     |
| 14.   | [Implement wildcard pattern matching](#question-14-implement-wildcard-pattern-matching)                                                                           |
| 15.   | [Longest substring with exactly K unique characters](#question-15-longest-substring-with-exactly-k-unique-characters)                                             |
| 16.   | [Count number of palindromic subsequences](#question-16-count-number-of-palindromic-subsequences)                                                                 |
| 17.   | [Lexicographically smallest rotation of a string](#question-17-lexicographically-smallest-rotation-of-a-string)                                                   |
| 18.   | [Compress string using run-length encoding](#question-18-compress-string-using-run-length-encoding)                                                               |
| 19.   | [Count anagram substrings of a pattern in a text](#question-19-count-anagram-substrings-of-a-pattern-in-a-text)                                                   |
| 20.   | [Longest substring with at most K replacements to make all same character](#question-20-longest-substring-with-at-most-k-replacements-to-make-all-same-character) |

## Question 1. Count all subarrays where the sum is divisible by a given number

# Count All Subarrays Where the Sum Is Divisible by a Given Number

## Direct Answer

To count subarrays whose sum is divisible by `k`, use **prefix sums + remainder frequency counting (Hash Map)**.

The key observation:

If two prefix sums have the **same remainder when divided by `k`**, then the subarray between them has a sum divisible by `k`.

---

# 1. Problem Understanding

Given:

```js
arr = [4, 5, 0, -2, -3, 1];
k = 5;
```

Find the number of subarrays whose sum is divisible by `5`.

Valid subarrays include:

```js
[5]
[5,0]
[0]
[-2,-3]
[4,5,0,-2,-3,1]
...
```

Output:

```js
7;
```

---

# 2. Brute Force Approach

Generate every subarray and check whether its sum is divisible by `k`.

### Algorithm

1. Pick every starting index.
2. Extend subarray to every ending index.
3. Maintain running sum.
4. If `sum % k === 0`, increment count.

### JavaScript

```js
const countDivisibleSubarrays = (arr, k) => {
  let count = 0;

  for (let i = 0; i < arr.length; i++) {
    let sum = 0;

    for (let j = i; j < arr.length; j++) {
      sum += arr[j];

      if (sum % k === 0) {
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

Not suitable for large arrays.

---

# 3. Optimal Approach — Prefix Sum + Hash Map

## Key Observation

Suppose:

```js
prefixSum[i] % k = r
prefixSum[j] % k = r
```

Then:

```js
(prefixSum[j] - prefixSum[i]) % k = 0
```

which means:

```js
sum(i+1 ... j)
```

is divisible by `k`.

Therefore, whenever a remainder appears again, all previous occurrences of that remainder form valid subarrays.

---

## Example

```js
arr = [4, 5, 0, -2, -3, 1];
k = 5;
```

Prefix sums:

| Prefix Sum | Remainder |
| ---------- | --------- |
| 4          | 4         |
| 9          | 4         |
| 9          | 4         |
| 7          | 2         |
| 4          | 4         |
| 5          | 0         |

Remainders:

```js
[4, 4, 4, 2, 4, 0];
```

Remainder `4` appears 4 times.

Number of pairs:

```js
C(4,2) = 6
```

Plus remainder `0` contributes additional valid subarrays.

Total:

```js
7;
```

---

# 4. Efficient Algorithm

Maintain:

```js
remainderFreq[remainder];
```

Initially:

```js
remainderFreq[0] = 1;
```

This handles subarrays starting from index `0`.

For each element:

1. Update prefix sum.
2. Compute remainder.
3. If remainder already exists:
   - add its frequency to answer.

4. Increment frequency.

---

# 5. JavaScript Solution

```js
const countDivisibleSubarrays = (arr, k) => {
  const freq = new Map();

  freq.set(0, 1);

  let prefixSum = 0;
  let count = 0;

  for (const num of arr) {
    prefixSum += num;

    let remainder = prefixSum % k;

    // Handle negative numbers
    if (remainder < 0) {
      remainder += k;
    }

    count += freq.get(remainder) || 0;

    freq.set(remainder, (freq.get(remainder) || 0) + 1);
  }

  return count;
};
```

---

# 6. Dry Run

```js
arr = [4, 5, 0, -2, -3, 1];
k = 5;
```

Initial:

```js
freq = { 0: 1 };
count = 0;
```

### 4

```js
prefix = 4;
rem = 4;

count += 0;

freq[4] = 1;
```

### 5

```js
prefix = 9;
rem = 4;

count += 1;
count = 1;

freq[4] = 2;
```

### 0

```js
prefix = 9;
rem = 4;

count += 2;
count = 3;

freq[4] = 3;
```

### -2

```js
prefix = 7;
rem = 2;

count += 0;
freq[2] = 1;
```

### -3

```js
prefix = 4;
rem = 4;

count += 3;
count = 6;

freq[4] = 4;
```

### 1

```js
prefix = 5;
rem = 0;

count += 1;
count = 7;
```

Answer:

```js
7;
```

---

# 7. Mathematical Insight

If a remainder occurs:

```js
f times
```

then it contributes:

```js
(f * (f - 1)) / 2;
```

valid subarrays because every pair of equal remainders forms one divisible-sum subarray.

The HashMap solution computes this on the fly.

---

# 8. Edge Cases

### Single element divisible by k

```js
arr = [10];
k = 5;

Answer = 1;
```

### No valid subarray

```js
arr = [1, 2, 4];
k = 7;

Answer = 0;
```

### Negative numbers

```js
arr = [-5, 10];
k = 5;
```

Use:

```js
remainder = ((prefixSum % k) + k) % k;
```

to avoid negative remainders.

### Entire array divisible

```js
arr = [1, 2, 3, 4];
k = 5;
```

The algorithm automatically counts it.

---

# 9. Interview Tips

- The crucial observation is:

  > Two prefix sums with the same remainder modulo `k` imply the subarray between them is divisible by `k`.

- Initialize:

  ```js
  freq.set(0, 1);
  ```

  otherwise subarrays starting from index `0` will be missed.

- Always normalize negative remainders:

  ```js
  remainder = ((prefixSum % k) + k) % k;
  ```

- This is a classic **Prefix Sum + HashMap** pattern frequently asked in interviews.

### Complexity (Optimal)

- **Time:** O(n)
- **Space:** O(min(n, k)) (or O(n) in the worst case)

This is the standard interview-optimal solution.

## Question 2. Maximum sum subarray with exactly K elements

# Maximum Sum Subarray with Exactly K Elements

## Direct Answer

For finding the **maximum sum subarray of exactly `K` elements**, the optimal approach is the **Sliding Window Technique**.

- Compute the sum of the first `K` elements.
- Slide the window one element at a time.
- Add the new element entering the window and remove the element leaving the window.
- Track the maximum sum seen so far.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

---

# 1. Problem Understanding

Given an array and an integer `K`, find the maximum sum among all contiguous subarrays of size exactly `K`.

### Example

```js
arr = [2, 1, 5, 1, 3, 2];
K = 3;
```

Subarrays of size 3:

```js
[2,1,5] => 8
[1,5,1] => 7
[5,1,3] => 9
[1,3,2] => 6
```

Output:

```js
9;
```

---

# 2. Brute Force Approach

Generate every subarray of length `K` and compute its sum.

### Algorithm

For each starting index:

1. Calculate sum of next `K` elements.
2. Update maximum sum.

### JavaScript

```js
const maxSumSubarrayK = (arr, k) => {
  let maxSum = -Infinity;

  for (let i = 0; i <= arr.length - k; i++) {
    let sum = 0;

    for (let j = i; j < i + k; j++) {
      sum += arr[j];
    }

    maxSum = Math.max(maxSum, sum);
  }

  return maxSum;
};
```

### Complexity

- Time: **O(n × k)**
- Space: **O(1)**

---

# 3. Optimal Approach — Sliding Window

## Idea

Instead of recalculating every window sum:

When moving from one window to the next:

```js
Old Window:
[a, b, c]

New Window:
[b, c, d]
```

We can update:

```js
newSum = oldSum - a + d;
```

This avoids recomputing the entire sum.

---

# 4. Sliding Window Algorithm

### Step 1

Compute sum of first `K` elements.

### Step 2

Store it as:

```js
maxSum = windowSum;
```

### Step 3

Slide window from index `K` to `n-1`.

For each step:

```js
windowSum += arr[i];
windowSum -= arr[i - k];
```

Update maximum.

---

# 5. JavaScript Solution

```js
const maxSumSubarrayK = (arr, k) => {
  const n = arr.length;

  if (k > n) return null;

  let windowSum = 0;

  // First window
  for (let i = 0; i < k; i++) {
    windowSum += arr[i];
  }

  let maxSum = windowSum;

  // Slide window
  for (let i = k; i < n; i++) {
    windowSum += arr[i] - arr[i - k];
    maxSum = Math.max(maxSum, windowSum);
  }

  return maxSum;
};
```

---

# 6. Dry Run

```js
arr = [2, 1, 5, 1, 3, 2];
k = 3;
```

### First Window

```js
[2, 1, 5];
sum = 8;
max = 8;
```

### Slide

```js
Remove 2, Add 1

sum = 8 - 2 + 1
    = 7
max = 8
```

### Slide

```js
Remove 1, Add 3

sum = 7 - 1 + 3
    = 9

max = 9
```

### Slide

```js
Remove 5, Add 2

sum = 9 - 5 + 2
    = 6

max = 9
```

Answer:

```js
9;
```

---

# 7. Variant: Return the Subarray as Well

Sometimes interviewers ask for the actual subarray.

```js
const maxSumSubarrayK = (arr, k) => {
  const n = arr.length;

  if (k > n) return null;

  let windowSum = 0;

  for (let i = 0; i < k; i++) {
    windowSum += arr[i];
  }

  let maxSum = windowSum;
  let startIndex = 0;

  for (let i = k; i < n; i++) {
    windowSum += arr[i] - arr[i - k];

    if (windowSum > maxSum) {
      maxSum = windowSum;
      startIndex = i - k + 1;
    }
  }

  return {
    maxSum,
    subarray: arr.slice(startIndex, startIndex + k),
  };
};
```

### Example Output

```js
{
  maxSum: 9,
  subarray: [5, 1, 3]
}
```

---

# 8. Edge Cases

### K = 1

```js
arr = [3, 7, 2];

Answer = 7;
```

---

### K = Array Length

```js
arr = [1, 2, 3];
k = 3;

Answer = 6;
```

---

### Negative Numbers

```js
arr = [-5, -2, -8, -1];
k = 2;
```

Windows:

```js
[-5,-2] = -7
[-2,-8] = -10
[-8,-1] = -9
```

Answer:

```js
-7;
```

Initialize with the first window sum, not `0`.

---

### K > N

```js
arr = [1, 2];
k = 5;
```

Return:

```js
null;
```

(or as specified by the problem statement).

---

# Interview Tips

- This is one of the most common **Sliding Window** interview problems.
- The key optimization is:

  ```js
  windowSum += arr[i] - arr[i - k];
  ```

- Avoid recomputing window sums from scratch.
- Be careful with:
  - Negative numbers
  - `k > n`
  - Initialization of the first window

### Complexity (Optimal)

- **Time:** O(n)
- **Space:** O(1)

This is the standard interview-optimal solution for **Maximum Sum Subarray with Exactly K Elements**.

## Question 3. Find smallest positive missing number in unsorted array

## Question 4. Find two elements whose sum is closest to zero

## Question 5. Find maximum sum path in two sorted arrays (merge-like problem)

## Question 6. Check if an array is a mountain array

## Question 7. Find the maximum sum of i\*arr[i] after rotations

## Question 8. Count all inversions in an array using BIT or segment tree

## Question 9. Count number of subarrays with exactly K odd numbers

## Question 10. Rearrange array elements so that even and odd numbers alternate

## Question 11. Minimum insertions to make a string palindrome

## Question 12. Count number of distinct subsequences

## Question 13. Longest substring with at most two distinct characters

## Question 14. Implement wildcard pattern matching

## Question 15. Longest substring with exactly K unique characters

## Question 16. Count number of palindromic subsequences

## Question 17. Lexicographically smallest rotation of a string

## Question 18. Compress string using run-length encoding

## Question 19. Count anagram substrings of a pattern in a text

## Question 20. Longest substring with at most K replacements to make all same character
