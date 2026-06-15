# Set 14

| S.No. | Question                                                                                                                               |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Merge intervals](#question-1-merge-intervals)                                                                                         |
| 2.    | [Insert interval in sorted intervals](#question-2-insert-interval-in-sorted-intervals)                                                 |
| 3.    | [Quickselect algorithm (k-th smallest element)](#question-3-quickselect-algorithm-k-th-smallest-element)                               |
| 4.    | [Wiggle sort](#question-4-wiggle-sort)                                                                                                 |
| 5.    | [Find max consecutive ones in a binary array](#question-5-find-max-consecutive-ones-in-a-binary-array)                                 |
| 6.    | [Longest subarray with sum k](#question-6-longest-subarray-with-sum-k)                                                                 |
| 7.    | [Subarray with sum divisible by k (negative numbers included)](#question-7-subarray-with-sum-divisible-by-k-negative-numbers-included) |
| 8.    | [Count pairs with XOR = k](#question-8-count-pairs-with-xor--k)                                                                        |
| 9.    | [Check if two arrays are permutations of each other](#question-9-check-if-two-arrays-are-permutations-of-each-other)                   |
| 10.   | [Frequency of elements in sliding window](#question-10-frequency-of-elements-in-sliding-window)                                        |
| 11.   | [Maximum sum rectangle in a 2D matrix](#question-11-maximum-sum-rectangle-in-a-2d-matrix)                                              |
| 12.   | [Count number of ways to reach target (combination sum)](#question-12-count-number-of-ways-to-reach-target-combination-sum)            |
| 13.   | [Longest bitonic subsequence](#question-13-longest-bitonic-subsequence)                                                                |
| 14.   | [Egg drop problem (minimum trials)](#question-14-egg-drop-problem-minimum-trials)                                                      |
| 15.   | [Boolean parenthesization problem](#question-15-boolean-parenthesization-problem)                                                      |
| 16.   | [Maximum length chain of pairs](#question-16-maximum-length-chain-of-pairs)                                                            |
| 17.   | [Palindrome partitioning II (minimum cuts)](#question-17-palindrome-partitioning-ii-minimum-cuts)                                      |
| 18.   | [Largest sum of non-adjacent elements in array](#question-18-largest-sum-of-non-adjacent-elements-in-array)                            |
| 19.   | [Russian doll envelopes problem](#question-19-russian-doll-envelopes-problem)                                                          |
| 20.   | [Minimum cost to cut a stick](#question-20-minimum-cost-to-cut-a-stick)                                                                |

## Question 1. Merge intervals

# Merge Intervals

## Concise Answer

Given a list of intervals, merge all overlapping intervals and return the resulting non-overlapping intervals.

Example:

```text
Input: [[1,3],[2,6],[8,10],[15,18]]

Output: [[1,6],[8,10],[15,18]]
```

The standard approach is:

1. Sort intervals by start time.
2. Traverse the sorted intervals.
3. If the current interval overlaps with the last merged interval, merge them.
4. Otherwise, add it as a new interval.

---

# 1. Problem Understanding

Two intervals overlap if:

```text
current.start <= previous.end
```

Example:

```text
[1,3] and [2,6]
```

Since:

```text
2 <= 3
```

they overlap and become:

```text
[1,6]
```

---

# 2. Approach 1: Sorting + Greedy (Optimal)

### Idea

After sorting by start time:

- Overlapping intervals will appear next to each other.
- Keep a result array.
- Compare each interval with the last merged interval.

### Steps

For:

```js
[
  [1, 3],
  [2, 6],
  [8, 10],
  [15, 18],
];
```

After sorting:

```js
[
  [1, 3],
  [2, 6],
  [8, 10],
  [15, 18],
];
```

Start:

```js
result = [[1, 3]];
```

Process `[2,6]`

```text
2 <= 3
```

Merge:

```js
result = [[1, 6]];
```

Process `[8,10]`

```text
8 > 6
```

Add new interval:

```js
result = [
  [1, 6],
  [8, 10],
];
```

Process `[15,18]`

```js
result = [
  [1, 6],
  [8, 10],
  [15, 18],
];
```

---

# 3. JavaScript Solution

```javascript
const mergeIntervals = (intervals) => {
  if (intervals.length <= 1) return intervals;

  intervals.sort((a, b) => a[0] - b[0]);

  const result = [intervals[0]];

  for (let i = 1; i < intervals.length; i++) {
    const current = intervals[i];
    const lastMerged = result[result.length - 1];

    if (current[0] <= lastMerged[1]) {
      lastMerged[1] = Math.max(lastMerged[1], current[1]);
    } else {
      result.push(current);
    }
  }

  return result;
};
```

### Example

```javascript
console.log(
  mergeIntervals([
    [1, 3],
    [2, 6],
    [8, 10],
    [15, 18],
  ]),
);
```

Output:

```javascript
[
  [1, 6],
  [8, 10],
  [15, 18],
];
```

---

# 4. Complexity Analysis

### Time Complexity

Sorting:

```text
O(n log n)
```

Traversal:

```text
O(n)
```

Total:

```text
O(n log n)
```

---

### Space Complexity

Result array:

```text
O(n)
```

---

# 5. Approach 2: In-Place Merging

If modifying the input is allowed, we can reuse the sorted array.

```javascript
const mergeIntervalsInPlace = (intervals) => {
  if (intervals.length <= 1) return intervals;

  intervals.sort((a, b) => a[0] - b[0]);

  let index = 0;

  for (let i = 1; i < intervals.length; i++) {
    if (intervals[i][0] <= intervals[index][1]) {
      intervals[index][1] = Math.max(intervals[index][1], intervals[i][1]);
    } else {
      index++;
      intervals[index] = intervals[i];
    }
  }

  return intervals.slice(0, index + 1);
};
```

### Complexity

```text
Time:  O(n log n)
Space: O(1) extra (excluding output)
```

---

# 6. Edge Cases

### Single Interval

```js
[[1, 5]];
```

Output:

```js
[[1, 5]];
```

---

### No Overlap

```js
[
  [1, 2],
  [3, 4],
  [5, 6],
];
```

Output:

```js
[
  [1, 2],
  [3, 4],
  [5, 6],
];
```

---

### Complete Overlap

```js
[
  [1, 10],
  [2, 3],
  [4, 5],
];
```

Output:

```js
[[1, 10]];
```

---

### Touching Intervals

Many interviewers consider these overlapping:

```js
[
  [1, 4],
  [4, 5],
];
```

Output:

```js
[[1, 5]];
```

because:

```text
4 <= 4
```

---

### Unsorted Input

```js
[
  [8, 10],
  [1, 3],
  [2, 6],
];
```

Sorting handles this automatically.

Output:

```js
[
  [1, 6],
  [8, 10],
];
```

---

# 7. Common Pitfalls

### Forgetting to Sort

```js
[
  [5, 6],
  [1, 4],
];
```

Without sorting, merging logic fails.

---

### Using `<` Instead of `<=`

Wrong:

```js
current[0] < lastMerged[1];
```

This won't merge:

```js
[1,4] and [4,5]
```

Correct:

```js
current[0] <= lastMerged[1];
```

---

# 8. Interview Tip

A common follow-up is:

> "How many intervals were removed after merging?"

After merging:

```javascript
removed = originalLength - mergedLength;
```

Another frequent variation is **Insert Interval**, where a new interval must be inserted into an already sorted list and merged efficiently in **O(n)** time.

**Key takeaway:** The optimal interview solution is **Sort + Greedy Merge**, with **O(n log n)** time and **O(n)** space.

## Question 2. Insert interval in sorted intervals

# Insert Interval in Sorted Intervals

## Concise Answer

Given a list of **non-overlapping intervals sorted by start time** and a new interval, insert the new interval into the correct position and merge if necessary.

Example:

```text
Input:
intervals = [[1,3],[6,9]]
newInterval = [2,5]

Output:
[[1,5],[6,9]]
```

The optimal solution runs in **O(n)** time because the intervals are already sorted.

---

# 1. Problem Understanding

We are given:

- Sorted intervals
- No existing overlaps
- One new interval to insert

After insertion:

- Maintain sorted order
- Merge overlapping intervals

Example:

```text
Intervals:   [1,2] [3,5] [6,7] [8,10] [12,16]
New:                 [4,9]
```

After merging:

```text
[1,2] [3,10] [12,16]
```

---

# 2. Approach 1: Linear Scan (Optimal)

Since intervals are already sorted:

### Three Cases

For each interval:

### Case 1: Current interval is completely before new interval

```text
current.end < new.start
```

Add current interval to result.

Example:

```text
[1,2]   [5,7]
```

---

### Case 2: Current interval is completely after new interval

```text
current.start > new.end
```

Add new interval once and then all remaining intervals.

Example:

```text
[8,10] after [3,6]
```

---

### Case 3: Overlapping

```text
current.start <= new.end
AND
current.end >= new.start
```

Merge:

```js
newStart = Math.min(current.start, new.start)
newEnd = Math.max(current.end, new.end)
```

Update `newInterval`.

---

# 3. Dry Run

```js
intervals = [
  [1, 3],
  [6, 9],
];
newInterval = [2, 5];
```

### Interval [1,3]

Overlap:

```text
[1,3]
[2,5]
```

Merge:

```js
newInterval = [1, 5];
```

### Interval [6,9]

No overlap:

```text
6 > 5
```

Insert merged interval:

```js
[
  [1, 5],
  [6, 9],
];
```

---

# 4. JavaScript Solution (Optimal O(n))

```javascript
const insertInterval = (intervals, newInterval) => {
  const result = [];
  let i = 0;
  const n = intervals.length;

  // Add intervals completely before newInterval
  while (i < n && intervals[i][1] < newInterval[0]) {
    result.push(intervals[i]);
    i++;
  }

  // Merge overlapping intervals
  while (i < n && intervals[i][0] <= newInterval[1]) {
    newInterval[0] = Math.min(newInterval[0], intervals[i][0]);

    newInterval[1] = Math.max(newInterval[1], intervals[i][1]);

    i++;
  }

  result.push(newInterval);

  // Add remaining intervals
  while (i < n) {
    result.push(intervals[i]);
    i++;
  }

  return result;
};
```

---

# Example

```javascript
const intervals = [
  [1, 2],
  [3, 5],
  [6, 7],
  [8, 10],
  [12, 16],
];

const newInterval = [4, 9];

console.log(insertInterval(intervals, newInterval));
```

Output:

```javascript
[
  [1, 2],
  [3, 10],
  [12, 16],
];
```

---

# 5. Complexity Analysis

### Time Complexity

We scan each interval at most once:

```text
O(n)
```

---

### Space Complexity

Result array:

```text
O(n)
```

---

# 6. Alternative Approach: Insert + Merge

### Idea

1. Insert interval into array.
2. Run the standard Merge Intervals algorithm.

```javascript
const insertAndMerge = (intervals, newInterval) => {
  intervals.push(newInterval);

  intervals.sort((a, b) => a[0] - b[0]);

  const result = [];

  for (const interval of intervals) {
    if (result.length === 0 || result[result.length - 1][1] < interval[0]) {
      result.push([...interval]);
    } else {
      result[result.length - 1][1] = Math.max(
        result[result.length - 1][1],
        interval[1],
      );
    }
  }

  return result;
};
```

### Complexity

```text
Sorting: O(n log n)
Merging: O(n)

Total: O(n log n)
```

Not optimal because the intervals are already sorted.

---

# 7. Edge Cases

### Empty Input

```js
intervals = [];
newInterval = [2, 5];
```

Output:

```js
[[2, 5]];
```

---

### Insert at Beginning

```js
intervals = [
  [5, 7],
  [8, 10],
];
newInterval = [1, 3];
```

Output:

```js
[
  [1, 3],
  [5, 7],
  [8, 10],
];
```

---

### Insert at End

```js
intervals = [
  [1, 2],
  [3, 4],
];
newInterval = [6, 8];
```

Output:

```js
[
  [1, 2],
  [3, 4],
  [6, 8],
];
```

---

### Merge All Intervals

```js
intervals = [
  [1, 3],
  [4, 6],
  [7, 9],
];
newInterval = [2, 8];
```

Output:

```js
[[1, 9]];
```

---

### Touching Intervals

```js
intervals = [
  [1, 2],
  [5, 7],
];
newInterval = [2, 5];
```

Using:

```js
intervals[i][0] <= newInterval[1];
```

Output:

```js
[[1, 7]];
```

---

# Interview Tips

### Why is O(n) possible?

Because the intervals are already:

1. Sorted by start time.
2. Non-overlapping.

This allows a single linear scan without sorting again.

### Common Follow-Up

**What if intervals are not sorted?**

Then:

1. Insert the interval.
2. Sort all intervals.
3. Apply Merge Intervals.

Complexity:

```text
O(n log n)
```

### Key Insight

For sorted, non-overlapping intervals, the optimal solution is a **single-pass linear scan**:

```text
Time  : O(n)
Space : O(n)
```

## Question 3. Quickselect algorithm (k-th smallest element)

## Question 4. Wiggle sort

## Question 5. Find max consecutive ones in a binary array

## Question 6. Longest subarray with sum k

## Question 7. Subarray with sum divisible by k (negative numbers included)

## Question 8. Count pairs with XOR = k

## Question 9. Check if two arrays are permutations of each other

## Question 10. Frequency of elements in sliding window

## Question 11. Maximum sum rectangle in a 2D matrix

## Question 12. Count number of ways to reach target (combination sum)

## Question 13. Longest bitonic subsequence

## Question 14. Egg drop problem (minimum trials)

## Question 15. Boolean parenthesization problem

## Question 16. Maximum length chain of pairs

## Question 17. Palindrome partitioning II (minimum cuts)

## Question 18. Largest sum of non-adjacent elements in array

## Question 19. Russian doll envelopes problem

## Question 20. Minimum cost to cut a stick
