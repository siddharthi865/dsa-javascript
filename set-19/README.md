# Set 19

| S.No. | Question                                                                                                                                          |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Search in 2D matrix where rows and columns are sorted](#question-1-search-in-2d-matrix-where-rows-and-columns-are-sorted)                        |
| 2.    | [Find peak element in 2D matrix](#question-2-find-peak-element-in-2d-matrix)                                                                      |
| 3.    | [Merge k sorted arrays](#question-3-merge-k-sorted-arrays)                                                                                        |
| 4.    | [Find kth largest sum of contiguous subarray](#question-4-find-kth-largest-sum-of-contiguous-subarray)                                            |
| 5.    | [Sort array of strings by frequency](#question-5-sort-array-of-strings-by-frequency)                                                              |
| 6.    | [Find smallest range covering elements from k lists](#question-6-find-smallest-range-covering-elements-from-k-lists)                              |
| 7.    | [Count inversions in array using merge sort](#question-7-count-inversions-in-array-using-merge-sort)                                              |
| 8.    | [Fractional knapsack (greedy)](#question-8-fractional-knapsack-greedy)                                                                            |
| 9.    | [Allocate minimum number of pages problem](#question-9-allocate-minimum-number-of-pages-problem)                                                  |
| 10.   | [Find maximum sum of i\*arr[i] after rotations](#question-10-find-maximum-sum-of-iarri-after-rotations)                                           |
| 11.   | [Count distinct elements in all windows of size k](#question-11-count-distinct-elements-in-all-windows-of-size-k)                                 |
| 12.   | [Longest subarray with sum zero](#question-12-longest-subarray-with-sum-zero)                                                                     |
| 13.   | [Longest substring with all distinct characters](#question-13-longest-substring-with-all-distinct-characters)                                     |
| 14.   | [Check if array can be divided into pairs with sum divisible by k](#question-14-check-if-array-can-be-divided-into-pairs-with-sum-divisible-by-k) |
| 15.   | [Subarray with sum divisible by k (including negative numbers)](#question-15-subarray-with-sum-divisible-by-k-including-negative-numbers)         |
| 16.   | [Count number of subarrays with XOR = k](#question-16-count-number-of-subarrays-with-xor--k)                                                      |
| 17.   | [Group anagrams together](#question-17-group-anagrams-together)                                                                                   |
| 18.   | [Find two numbers appearing odd times in array](#question-18-find-two-numbers-appearing-odd-times-in-array)                                       |
| 19.   | [Count pairs with difference k](#question-19-count-pairs-with-difference-k)                                                                       |
| 20.   | [Maximum frequency element in subarrays of size k](#question-20-maximum-frequency-element-in-subarrays-of-size-k)                                 |

## Question 1. Search in 2D matrix where rows and columns are sorted

# Search in a 2D Matrix Where Rows and Columns Are Sorted

## Direct Answer

Given an `m × n` matrix where:

- Each row is sorted in ascending order.
- Each column is sorted in ascending order.

The most efficient approach is to start from the **top-right corner** (or bottom-left corner) and eliminate one row or one column in each step.

- **Time Complexity:** `O(m + n)`
- **Space Complexity:** `O(1)`

---

# 1. Problem Understanding

Example:

```js
[
  [10, 20, 30, 40],
  [15, 25, 35, 45],
  [27, 29, 37, 48],
  [32, 33, 39, 50],
];
```

Search for:

```js
29;
```

Output:

```js
true;
```

Since both rows and columns are sorted, we can use their ordering to discard large portions of the matrix.

---

# 2. Approach 1: Staircase Search (Optimal)

## Idea

Start from the **top-right corner**.

At any position:

- If current element equals target → found.
- If current element > target → move left.
- If current element < target → move down.

Why?

- Moving left decreases values.
- Moving down increases values.

Thus, one row or one column gets eliminated in every step.

---

## Example

Search `29`:

```js
[
  [10, 20, 30, 40],
  [15, 25, 35, 45],
  [27, 29, 37, 48],
  [32, 33, 39, 50],
];
```

Start at `40`.

```text
40 > 29 → left
30 > 29 → left
20 < 29 → down
25 < 29 → down
29 found
```

---

## JavaScript Code

```js
const searchMatrix = (matrix, target) => {
  if (!matrix.length || !matrix[0].length) return false;

  let row = 0;
  let col = matrix[0].length - 1;

  while (row < matrix.length && col >= 0) {
    const current = matrix[row][col];

    if (current === target) {
      return true;
    }

    if (current > target) {
      col--;
    } else {
      row++;
    }
  }

  return false;
};
```

---

## Complexity Analysis

- Time: `O(m + n)`
- Space: `O(1)`

This is the optimal solution for this problem.

---

# 3. Approach 2: Binary Search on Every Row

## Idea

Since each row is sorted:

1. Iterate through every row.
2. Perform binary search on that row.

---

## JavaScript Code

```js
const searchMatrixBinary = (matrix, target) => {
  const binarySearch = (row) => {
    let left = 0;
    let right = row.length - 1;

    while (left <= right) {
      const mid = Math.floor((left + right) / 2);

      if (row[mid] === target) return true;

      if (row[mid] < target) {
        left = mid + 1;
      } else {
        right = mid - 1;
      }
    }

    return false;
  };

  for (const row of matrix) {
    if (target >= row[0] && target <= row[row.length - 1]) {
      if (binarySearch(row)) {
        return true;
      }
    }
  }

  return false;
};
```

---

## Complexity Analysis

- Time: `O(m log n)`
- Space: `O(1)`

Good when the number of rows is small.

---

# 4. Approach 3: Brute Force

Check every element.

```js
const searchMatrixBrute = (matrix, target) => {
  for (const row of matrix) {
    for (const num of row) {
      if (num === target) return true;
    }
  }

  return false;
};
```

### Complexity

- Time: `O(m × n)`
- Space: `O(1)`

Not recommended in interviews unless discussing baseline solutions.

---

# Edge Cases

### Empty Matrix

```js
[];
```

Output:

```js
false;
```

---

### Single Element

```js
[[5]];
```

Search `5` → `true`

Search `10` → `false`

---

### Target Smaller Than Minimum

```js
target = 1;
```

Minimum value is `10`.

Output:

```js
false;
```

---

### Target Greater Than Maximum

```js
target = 100;
```

Output:

```js
false;
```

---

# Interview Tips

When asked this question, mention:

> "Because rows and columns are both sorted, starting from the top-right corner gives a position where moving left decreases values and moving down increases values. This allows eliminating an entire row or column at each step, leading to an O(m + n) solution."

This explanation is usually what interviewers look for, not just the code.

### Final Recommended Solution

```js
const searchMatrix = (matrix, target) => {
  if (!matrix.length || !matrix[0].length) return false;

  let row = 0;
  let col = matrix[0].length - 1;

  while (row < matrix.length && col >= 0) {
    if (matrix[row][col] === target) return true;

    if (matrix[row][col] > target) {
      col--;
    } else {
      row++;
    }
  }

  return false;
};
```

**Time:** `O(m + n)`
**Space:** `O(1)`
**Best choice for interviews.**

## Question 2. Find peak element in 2D matrix

# Find Peak Element in a 2D Matrix

## Direct Answer

A **peak element** in a 2D matrix is an element that is **greater than or equal to its neighboring elements** (typically up, down, left, and right).

The optimal interview solution uses **Binary Search on columns**:

- Find the maximum element in the middle column.
- Compare it with its left and right neighbors.
- Move to the half that must contain a peak.
- Continue until a peak is found.

**Time Complexity:** `O(m log n)`
**Space Complexity:** `O(1)`

where:

- `m` = number of rows
- `n` = number of columns

---

# 1. Problem Understanding

Example:

```js
[
  [10, 8, 10, 10],
  [14, 13, 12, 11],
  [15, 9, 11, 21],
  [16, 17, 19, 20],
];
```

Possible peak:

```js
21;
```

Because:

```text
21 > 11 (left)
21 > 20 (down)
```

and it has no right/up neighbor.

---

# 2. Approach 1: Brute Force

## Idea

Check every cell and verify whether it is greater than or equal to all valid neighbors.

### JavaScript Code

```js
const findPeak2DBrute = (matrix) => {
  const rows = matrix.length;
  const cols = matrix[0].length;

  const directions = [
    [-1, 0],
    [1, 0],
    [0, -1],
    [0, 1],
  ];

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      let isPeak = true;

      for (const [dr, dc] of directions) {
        const nr = r + dr;
        const nc = c + dc;

        if (
          nr >= 0 &&
          nr < rows &&
          nc >= 0 &&
          nc < cols &&
          matrix[nr][nc] > matrix[r][c]
        ) {
          isPeak = false;
          break;
        }
      }

      if (isPeak) {
        return [r, c];
      }
    }
  }

  return [-1, -1];
};
```

### Complexity

- Time: `O(m × n)`
- Space: `O(1)`

---

# 3. Approach 2: Binary Search on Columns (Optimal)

## Key Observation

Consider a middle column.

1. Find the row containing the maximum element in that column.
2. Let that element be:

```text
matrix[maxRow][mid]
```

3. Compare with:

```text
left neighbor
right neighbor
```

### Cases

#### Case 1: Current is greater than both

```text
left < current > right
```

Current element is a peak.

---

#### Case 2: Left is greater

```text
left > current
```

A peak must exist in the left half.

Move:

```text
high = mid - 1
```

---

#### Case 3: Right is greater

```text
right > current
```

A peak must exist in the right half.

Move:

```text
low = mid + 1
```

---

# Example

```js
[
  [10, 8, 10, 10],
  [14, 13, 12, 11],
  [15, 9, 11, 21],
  [16, 17, 19, 20],
];
```

Middle column:

```text
Column 1

8
13
9
17
```

Maximum = 17

Compare:

```text
left = 16
right = 19
```

Since:

```text
19 > 17
```

Move right.

Eventually reach:

```text
21
```

which is a peak.

---

# JavaScript Code (Optimal)

```js
const findPeak2D = (matrix) => {
  const rows = matrix.length;
  const cols = matrix[0].length;

  let left = 0;
  let right = cols - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    let maxRow = 0;

    for (let r = 1; r < rows; r++) {
      if (matrix[r][mid] > matrix[maxRow][mid]) {
        maxRow = r;
      }
    }

    const current = matrix[maxRow][mid];

    const leftValue = mid > 0 ? matrix[maxRow][mid - 1] : -Infinity;

    const rightValue = mid < cols - 1 ? matrix[maxRow][mid + 1] : -Infinity;

    if (current >= leftValue && current >= rightValue) {
      return [maxRow, mid];
    }

    if (leftValue > current) {
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }

  return [-1, -1];
};
```

---

# Complexity Analysis

For every binary-search step:

- Finding maximum in a column → `O(m)`
- Binary search on columns → `O(log n)`

Total:

```text
O(m log n)
```

Space:

```text
O(1)
```

---

# Why Does This Work?

When the maximum element in a column is chosen:

```text
      ?
      ↑
left current right
      ↓
      ?
```

The up/down neighbors cannot be larger because `current` is already the largest in that column.

Therefore only left and right neighbors matter.

If one side is larger, a peak must exist in that direction, allowing binary search.

---

# Edge Cases

### Single Element

```js
[[5]];
```

Peak = `5`

---

### Single Row

```js
[[1, 3, 7, 5]];
```

Peak = `7`

---

### Single Column

```js
[[1], [4], [3]];
```

Peak = `4`

---

### Multiple Peaks

```js
[
  [10, 20],
  [30, 25],
];
```

Both `20` and `30` are valid peaks.

Returning any peak is acceptable unless specified otherwise.

---

# Interview Tips

Mention the progression:

1. Brute force → `O(mn)`
2. Use binary search on columns.
3. Find maximum in middle column.
4. Move toward the larger neighbor.
5. Achieve **O(m log n)** time and **O(1)** space.

### Final Recommended Solution

```js
const findPeak2D = (matrix) => {
  let left = 0;
  let right = matrix[0].length - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    let maxRow = 0;

    for (let r = 1; r < matrix.length; r++) {
      if (matrix[r][mid] > matrix[maxRow][mid]) {
        maxRow = r;
      }
    }

    const curr = matrix[maxRow][mid];
    const leftVal = mid > 0 ? matrix[maxRow][mid - 1] : -Infinity;
    const rightVal =
      mid < matrix[0].length - 1 ? matrix[maxRow][mid + 1] : -Infinity;

    if (curr >= leftVal && curr >= rightVal) {
      return [maxRow, mid];
    }

    if (leftVal > curr) {
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }

  return [-1, -1];
};
```

**Time:** `O(m log n)`
**Space:** `O(1)`
This is the standard optimal interview solution used in companies like Google, Amazon, Microsoft, and Meta.

## Question 3. Merge k sorted arrays

## Question 4. Find kth largest sum of contiguous subarray

## Question 5. Sort array of strings by frequency

## Question 6. Find smallest range covering elements from k lists

## Question 7. Count inversions in array using merge sort

## Question 8. Fractional knapsack (greedy)

## Question 9. Allocate minimum number of pages problem

## Question 10. Find maximum sum of i\*arr[i] after rotations

## Question 11. Count distinct elements in all windows of size k

## Question 12. Longest subarray with sum zero

## Question 13. Longest substring with all distinct characters

## Question 14. Check if array can be divided into pairs with sum divisible by k

## Question 15. Subarray with sum divisible by k (including negative numbers)

## Question 16. Count number of subarrays with XOR = k

## Question 17. Group anagrams together

## Question 18. Find two numbers appearing odd times in array

## Question 19. Count pairs with difference k

## Question 20. Maximum frequency element in subarrays of size k
