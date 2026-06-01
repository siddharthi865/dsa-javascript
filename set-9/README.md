# Set 9

| S.No. | Question                                                                                                                            |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Sort an array of 0s, 1s, and 2s (Dutch National Flag)](#question-1-sort-an-array-of-0s-1s-and-2s-dutch-national-flag)              |
| 2.    | [Sort nearly sorted (k-sorted) array](#question-2-sort-nearly-sorted-k-sorted-array)                                                |
| 3.    | [Find the k closest elements to a given value](#question-3-find-the-k-closest-elements-to-a-given-value)                            |
| 4.    | [Search in a bitonic array](#question-4-search-in-a-bitonic-array)                                                                  |
| 5.    | [Find the median in a stream of numbers](#question-5-find-the-median-in-a-stream-of-numbers)                                        |
| 6.    | [Subarray with given sum (unsorted array)](#question-6-subarray-with-given-sum-unsorted-array)                                      |
| 7.    | [Count distinct elements in every window of size k](#question-7-count-distinct-elements-in-every-window-of-size-k)                  |
| 8.    | [Two Sum problem - all pairs](#question-8-two-sum-problem---all-pairs)                                                              |
| 9.    | [Longest subarray with sum divisible by k](#question-9-longest-subarray-with-sum-divisible-by-k)                                    |
| 10.   | [Count number of subarrays with XOR equal to a given value](#question-10-count-number-of-subarrays-with-xor-equal-to-a-given-value) |
| 11.   | [Maximum sum increasing subsequence](#question-11-maximum-sum-increasing-subsequence)                                               |
| 12.   | [Minimum coins to make a target (combinations)](#question-12-minimum-coins-to-make-a-target-combinations)                           |
| 13.   | [Count number of ways to reach nth stair](#question-13-count-number-of-ways-to-reach-nth-stair)                                     |
| 14.   | [Partition problem (subset sum equal)](#question-14-partition-problem-subset-sum-equal)                                             |
| 15.   | [Maximum product subarray](#question-15-maximum-product-subarray)                                                                   |
| 16.   | [Longest palindromic substring](#question-16-longest-palindromic-substring)                                                         |
| 17.   | [Longest palindromic subsequence](#question-17-longest-palindromic-subsequence)                                                     |
| 18.   | [Matrix chain multiplication (counting ways)](#question-18-matrix-chain-multiplication-counting-ways)                               |
| 19.   | [Minimum insertions to make a string palindrome](#question-19-minimum-insertions-to-make-a-string-palindrome)                       |
| 20.   | [Decode ways (number of ways to decode digits to letters)](#question-20-decode-ways-number-of-ways-to-decode-digits-to-letters)     |

## Question 1. Sort an array of 0s, 1s, and 2s (Dutch National Flag)

# Sort an Array of 0s, 1s, and 2s (Dutch National Flag Problem)

## Concise Answer

The optimal solution is the **Dutch National Flag Algorithm**, which sorts an array containing only `0`, `1`, and `2` in a **single traversal** using **three pointers**.

- Time Complexity: **O(n)**
- Space Complexity: **O(1)**

---

# 1. Problem Understanding

Given an array containing only:

```js
(0, 1, 2);
```

Sort it such that:

```js
All 0s come first,
then all 1s,
then all 2s.
```

### Example

```js
Input: [2, 0, 2, 1, 1, 0];
Output: [0, 0, 1, 1, 2, 2];
```

### Follow-up

Can you do it:

- Without using a sorting algorithm?
- In one traversal?
- With constant extra space?

This is exactly what the Dutch National Flag algorithm achieves.

---

# 2. Approach 1: Counting Sort (Two Passes)

## Idea

Count occurrences of:

- 0s
- 1s
- 2s

Then overwrite the array.

### Steps

1. Count each number.
2. Fill array with:
   - count(0) zeros
   - count(1) ones
   - count(2) twos

---

## JavaScript Code

```js
const sortColors = (nums) => {
  let count0 = 0;
  let count1 = 0;
  let count2 = 0;

  for (const num of nums) {
    if (num === 0) count0++;
    else if (num === 1) count1++;
    else count2++;
  }

  let index = 0;

  while (count0--) nums[index++] = 0;
  while (count1--) nums[index++] = 1;
  while (count2--) nums[index++] = 2;

  return nums;
};
```

### Complexity

- Time: **O(n)**
- Space: **O(1)**

### Drawback

Requires **two passes**.

---

# 3. Approach 2: Dutch National Flag Algorithm (Optimal)

## Core Idea

Maintain three regions:

```text
[0 ... low-1]      -> 0s

[low ... mid-1]    -> 1s

[mid ... high]     -> Unknown

[high+1 ... n-1]   -> 2s
```

Use three pointers:

```js
low;
mid;
high;
```

---

## Rules

### Case 1: nums[mid] === 0

Swap with `low`.

```js
swap(low, mid);
low++;
mid++;
```

Because 0 belongs at the beginning.

---

### Case 2: nums[mid] === 1

Already in correct section.

```js
mid++;
```

---

### Case 3: nums[mid] === 2

Swap with `high`.

```js
swap(mid, high);
high--;
```

Do **not** increment `mid` here because the swapped element must still be checked.

---

# Dry Run

```js
[2, 0, 2, 1, 1, 0];

low = 0;
mid = 0;
high = 5;
```

### Step 1

```js
nums[mid] = 2;
```

Swap with high:

```js
[0, 0, 2, 1, 1, 2];

high = 4;
```

---

### Step 2

```js
nums[mid] = 0;
```

Swap low and mid:

```js
[0, 0, 2, 1, 1, 2];

low = 1;
mid = 1;
```

---

### Step 3

```js
nums[mid] = 0;
```

Swap:

```js
low = 2;
mid = 2;
```

---

### Step 4

```js
nums[mid] = 2;
```

Swap with high:

```js
[0, 0, 1, 1, 2, 2];

high = 3;
```

---

### Step 5

```js
nums[mid] = 1;
mid++;
```

### Step 6

```js
nums[mid] = 1;
mid++;
```

Done.

---

# JavaScript Code (Optimal)

```js
const sortColors = (nums) => {
  let low = 0;
  let mid = 0;
  let high = nums.length - 1;

  while (mid <= high) {
    if (nums[mid] === 0) {
      [nums[low], nums[mid]] = [nums[mid], nums[low]];
      low++;
      mid++;
    } else if (nums[mid] === 1) {
      mid++;
    } else {
      [nums[mid], nums[high]] = [nums[high], nums[mid]];
      high--;
    }
  }

  return nums;
};
```

---

# 4. Complexity Analysis

| Approach            | Time | Space | Passes |
| ------------------- | ---- | ----- | ------ |
| Counting Sort       | O(n) | O(1)  | 2      |
| Dutch National Flag | O(n) | O(1)  | 1      |

### Optimal

Dutch National Flag Algorithm:

```text
Time  : O(n)
Space : O(1)
```

Each element is processed at most once.

---

# 5. Edge Cases

### Empty Array

```js
[];
```

Output:

```js
[];
```

---

### Single Element

```js
[1];
```

Output:

```js
[1];
```

---

### Already Sorted

```js
[0, 0, 1, 1, 2, 2];
```

Works correctly.

---

### Reverse Sorted

```js
[2, 2, 1, 1, 0, 0];
```

Still O(n).

---

### All Same Values

```js
[0, 0, 0][(1, 1, 1)][(2, 2, 2)];
```

Handled naturally.

---

# 6. Common Pitfalls

### Mistake 1: Incrementing `mid` after swapping with `high`

Wrong:

```js
swap(mid, high);
high--;
mid++; // ❌
```

Why?

The new element at `mid` hasn't been processed yet.

Correct:

```js
swap(mid, high);
high--;
```

---

### Mistake 2: Using General Sorting

```js
nums.sort();
```

- Time: O(n log n)
- Doesn't satisfy the optimal requirement.

---

# 7. Interview Tips

When asked this question, mention:

1. Since only three distinct values exist, counting sort is possible.
2. The interviewer usually expects the **Dutch National Flag Algorithm**.
3. Maintain:
   - `low` → next position for 0
   - `mid` → current element
   - `high` → next position for 2

4. Process the array in a single traversal.
5. Remember: **after swapping with `high`, don't increment `mid`.**

This is a classic interview problem asked by companies such as Google, Amazon, Microsoft, and Meta to test in-place partitioning and pointer manipulation skills.

## Question 2. Sort nearly sorted (k-sorted) array

## Question 3. Find the k closest elements to a given value

## Question 4. Search in a bitonic array

## Question 5. Find the median in a stream of numbers

## Question 6. Subarray with given sum (unsorted array)

## Question 7. Count distinct elements in every window of size k

## Question 8. Two Sum problem - all pairs

## Question 9. Longest subarray with sum divisible by k

## Question 10. Count number of subarrays with XOR equal to a given value

## Question 11. Maximum sum increasing subsequence

## Question 12. Minimum coins to make a target (combinations)

## Question 13. Count number of ways to reach nth stair

## Question 14. Partition problem (subset sum equal)

## Question 15. Maximum product subarray

## Question 16. Longest palindromic substring

## Question 17. Longest palindromic subsequence

## Question 18. Matrix chain multiplication (counting ways)

## Question 19. Minimum insertions to make a string palindrome

## Question 20. Decode ways (number of ways to decode digits to letters)
