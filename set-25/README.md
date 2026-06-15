# Set 25

| S.No. | Question                                                                                                                     |
| ----- | ---------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Maximum sum rectangle in 2D matrix](#question-1-maximum-sum-rectangle-in-2d-matrix)                                         |
| 2.    | [Minimum cost to cut a stick into given lengths](#question-2-minimum-cost-to-cut-a-stick-into-given-lengths)                 |
| 3.    | [Longest increasing subsequence (with reconstruction)](#question-3-longest-increasing-subsequence-with-reconstruction)       |
| 4.    | [Egg dropping problem (minimum attempts)](#question-4-egg-dropping-problem-minimum-attempts)                                 |
| 5.    | [Weighted job scheduling for maximum profit](#question-5-weighted-job-scheduling-for-maximum-profit)                         |
| 6.    | [Longest common substring](#question-6-longest-common-substring)                                                             |
| 7.    | [Longest palindromic subsequence](#question-7-longest-palindromic-subsequence)                                               |
| 8.    | [Minimum insertions to make string palindrome](#question-8-minimum-insertions-to-make-string-palindrome)                     |
| 9.    | [Count number of ways to reach sum with unlimited coins](#question-9-count-number-of-ways-to-reach-sum-with-unlimited-coins) |
| 10.   | [Russian doll envelopes (envelope nesting problem)](#question-10-russian-doll-envelopes-envelope-nesting-problem)            |
| 11.   | [N-Queens problem (backtracking)](#question-11-n-queens-problem-backtracking)                                                |
| 12.   | [Rat in a maze (all paths)](#question-12-rat-in-a-maze-all-paths)                                                            |
| 13.   | [Maximum area of island in 2D grid](#question-13-maximum-area-of-island-in-2d-grid)                                          |
| 14.   | [Shortest bridge between two islands](#question-14-shortest-bridge-between-two-islands)                                      |
| 15.   | [Longest increasing path in a matrix](#question-15-longest-increasing-path-in-a-matrix)                                      |
| 16.   | [Trapping rainwater II (2D)](#question-16-trapping-rainwater-ii-2d)                                                          |
| 17.   | [Design LRU cache using hash map + doubly linked list](#question-17-design-lru-cache-using-hash-map--doubly-linked-list)     |
| 18.   | [Maximum XOR of two numbers in array](#question-18-maximum-xor-of-two-numbers-in-array)                                      |
| 19.   | [Word search in a grid](#question-19-word-search-in-a-grid)                                                                  |
| 20.   | [Sliding window median](#question-20-sliding-window-median)                                                                  |

## Question 1. Maximum sum rectangle in 2D matrix

# Maximum Sum Rectangle in a 2D Matrix

### Direct Answer

The **Maximum Sum Rectangle** problem asks us to find a contiguous rectangular submatrix with the largest possible sum.

The optimal interview solution extends **Kadane's Algorithm (1D Maximum Subarray Sum)** to 2D.

**Time Complexity:** `O(cols² × rows)` (or `O(rows² × cols)` depending on which dimension is smaller)

**Space Complexity:** `O(rows)`

---

# 1. Problem Understanding

Given a matrix:

```js
[
  [1, 2, -1, -4, -20],
  [-8, -3, 4, 2, 1],
  [3, 8, 10, 1, 3],
  [-4, -1, 1, 7, -6],
];
```

The rectangle with maximum sum is:

```js
[
  [-3, 4, 2],
  [8, 10, 1],
  [-1, 1, 7],
];
```

Sum:

```js
29;
```

---

# 2. Brute Force Approach

Generate every possible rectangle and calculate its sum.

### Steps

1. Choose top row.
2. Choose bottom row.
3. Choose left column.
4. Choose right column.
5. Calculate rectangle sum.

### Complexity

Number of rectangles:

```js
O(rows² × cols²)
```

Calculating each rectangle:

```js
O(rows × cols)
```

Total:

```js
O(rows³ × cols³)
```

Very inefficient.

---

# 3. Optimized Approach (Kadane's Algorithm)

## Key Idea

Convert the 2D problem into multiple 1D maximum subarray problems.

### Example

Matrix:

```js
1  2 -1
3  4  5
-2 1  6
```

Fix:

```js
left = 0;
right = 0;
```

Compressed array:

```js
[1, 3, -2];
```

Run Kadane.

---

Now expand:

```js
left = 0;
right = 1;
```

Compressed rows:

```js
[3, 7, -1];
```

Run Kadane again.

---

Then:

```js
left = 0;
right = 2;
```

Compressed rows:

```js
[2, 12, 5];
```

Run Kadane.

---

Repeat for all column pairs.

The maximum Kadane result across all pairs is the answer.

---

# 4. Kadane's Algorithm

For each compressed array:

```js
[-2, 3, 4, -1, 5];
```

Kadane finds:

```js
3 + 4 - 1 + 5 = 11
```

in:

```js
O(n);
```

time.

---

# 5. JavaScript Solution

## Maximum Sum Only

```js
const maxSumRectangle = (matrix) => {
  const rows = matrix.length;
  const cols = matrix[0].length;

  let maxSum = -Infinity;

  for (let left = 0; left < cols; left++) {
    const temp = new Array(rows).fill(0);

    for (let right = left; right < cols; right++) {
      for (let row = 0; row < rows; row++) {
        temp[row] += matrix[row][right];
      }

      const currentMax = kadane(temp);
      maxSum = Math.max(maxSum, currentMax);
    }
  }

  return maxSum;
};

const kadane = (arr) => {
  let maxEndingHere = arr[0];
  let maxSoFar = arr[0];

  for (let i = 1; i < arr.length; i++) {
    maxEndingHere = Math.max(arr[i], maxEndingHere + arr[i]);
    maxSoFar = Math.max(maxSoFar, maxEndingHere);
  }

  return maxSoFar;
};
```

---

## Example

```js
const matrix = [
  [1, 2, -1, -4, -20],
  [-8, -3, 4, 2, 1],
  [3, 8, 10, 1, 3],
  [-4, -1, 1, 7, -6],
];

console.log(maxSumRectangle(matrix));
```

Output:

```js
29;
```

---

# 6. Finding Rectangle Coordinates

Interviewers often ask for:

- Maximum sum
- Top row
- Bottom row
- Left column
- Right column

We modify Kadane to return indices.

```js
const kadaneWithIndices = (arr) => {
  let maxSum = arr[0];
  let currentSum = arr[0];

  let start = 0;
  let end = 0;
  let tempStart = 0;

  for (let i = 1; i < arr.length; i++) {
    if (currentSum < 0) {
      currentSum = arr[i];
      tempStart = i;
    } else {
      currentSum += arr[i];
    }

    if (currentSum > maxSum) {
      maxSum = currentSum;
      start = tempStart;
      end = i;
    }
  }

  return { maxSum, start, end };
};
```

Complete solution:

```js
const maxSumRectangleWithCoordinates = (matrix) => {
  const rows = matrix.length;
  const cols = matrix[0].length;

  let maxSum = -Infinity;

  let finalLeft = 0;
  let finalRight = 0;
  let finalTop = 0;
  let finalBottom = 0;

  for (let left = 0; left < cols; left++) {
    const temp = new Array(rows).fill(0);

    for (let right = left; right < cols; right++) {
      for (let row = 0; row < rows; row++) {
        temp[row] += matrix[row][right];
      }

      const result = kadaneWithIndices(temp);

      if (result.maxSum > maxSum) {
        maxSum = result.maxSum;

        finalLeft = left;
        finalRight = right;
        finalTop = result.start;
        finalBottom = result.end;
      }
    }
  }

  return {
    maxSum,
    top: finalTop,
    bottom: finalBottom,
    left: finalLeft,
    right: finalRight,
  };
};
```

---

# 7. Complexity Analysis

Let:

```js
R = rows;
C = columns;
```

Outer loops:

```js
O(C²)
```

Building compressed array:

```js
O(R);
```

Kadane:

```js
O(R);
```

Total:

```js
O(C² × R)
```

Space:

```js
O(R);
```

If rows < columns, you can compress columns instead:

```js
O(min(R, C)² × max(R, C))
```

which is the most optimized form.

---

# 8. Edge Cases

### All Negative Numbers

```js
[
  [-5, -2],
  [-8, -1],
];
```

Answer:

```js
-1;
```

Kadane must correctly handle all-negative arrays.

---

### Single Row

```js
[1, -2, 3, 4];
```

Becomes normal Kadane.

---

### Single Column

```js
[[1], [2], [-3]];
```

Also handled correctly.

---

# 9. Interview Tips

1. Mention that this is a **2D extension of Kadane's Algorithm**.
2. Explain the **column-pair compression** idea clearly.
3. State the optimized complexity:

```js
O(cols² × rows)
```

4. Discuss how to return rectangle coordinates.
5. Mention the dimension optimization:

```js
O(min(R,C)² × max(R,C))
```

This is the standard interview-expected solution for **Maximum Sum Rectangle in a 2D Matrix**.

## Question 2. Minimum cost to cut a stick into given lengths

# Minimum Cost to Cut a Stick

### Direct Answer

Given a stick of length `n` and an array of cut positions, each cut costs the **current length of the stick being cut**. The goal is to determine the minimum total cost to perform all cuts.

This is a classic **Interval Dynamic Programming** problem, similar to **Matrix Chain Multiplication**.

---

# 1. Problem Understanding

### Example

```js
n = 7;
cuts = [1, 3, 4, 5];
```

Stick:

```js
0 ------- 7
```

Cuts must be made at:

```js
(1, 3, 4, 5);
```

### Important Observation

The order of cuts matters.

#### Bad Order

Cut at `1` first:

```js
Cost = 7;
```

Remaining cuts produce additional costs.

Total may become:

```js
20+
```

#### Better Order

Cut at `3` first:

```js
Cost = 7;
```

Then cut smaller segments.

Total becomes:

```js
16;
```

Minimum cost:

```js
16;
```

---

# 2. Key Insight

Whenever we cut between positions:

```js
left and right
```

the cost is:

```js
right - left;
```

After choosing one cut position `k`:

```js
(left, k)(k, right);
```

become two independent subproblems.

This naturally suggests DP on intervals.

---

# 3. Recursive Formulation

Add boundaries:

```js
cuts = [0, ...cuts, n];
```

Example:

```js
[0, 1, 3, 4, 5, 7];
```

Define:

```js
dp(i, j);
```

= minimum cost to perform all cuts between

```js
cuts[i] and cuts[j]
```

If no cut exists:

```js
j - i <= 1;
```

then:

```js
dp(i, j) = 0
```

---

### Transition

Try every possible first cut:

```js
k ∈ (i+1 ... j-1)
```

Cost:

```js
(current segment length)
+
(left subproblem)
+
(right subproblem)
```

So:

```text
dp(i,j) =
min(
    cuts[j]-cuts[i]
    + dp(i,k)
    + dp(k,j)
)
```

---

# 4. Top-Down DP (Memoization)

### JavaScript

```js
const minCost = (n, cuts) => {
  cuts.sort((a, b) => a - b);

  const points = [0, ...cuts, n];
  const m = points.length;

  const memo = Array.from({ length: m }, () => Array(m).fill(-1));

  const dfs = (i, j) => {
    if (j - i <= 1) return 0;

    if (memo[i][j] !== -1) {
      return memo[i][j];
    }

    let ans = Infinity;

    for (let k = i + 1; k < j; k++) {
      ans = Math.min(ans, points[j] - points[i] + dfs(i, k) + dfs(k, j));
    }

    return (memo[i][j] = ans);
  };

  return dfs(0, m - 1);
};
```

---

# 5. Bottom-Up DP

### Idea

Build smaller intervals first.

```js
const minCost = (n, cuts) => {
  cuts.sort((a, b) => a - b);

  const points = [0, ...cuts, n];
  const m = points.length;

  const dp = Array.from({ length: m }, () => Array(m).fill(0));

  for (let len = 2; len < m; len++) {
    for (let i = 0; i + len < m; i++) {
      const j = i + len;

      dp[i][j] = Infinity;

      for (let k = i + 1; k < j; k++) {
        dp[i][j] = Math.min(
          dp[i][j],
          points[j] - points[i] + dp[i][k] + dp[k][j],
        );
      }

      if (dp[i][j] === Infinity) {
        dp[i][j] = 0;
      }
    }
  }

  return dp[0][m - 1];
};
```

---

# 6. Example Walkthrough

```js
n = 7;
cuts = [1, 3, 4, 5];
```

After adding boundaries:

```js
[0, 1, 3, 4, 5, 7];
```

Final answer:

```js
16;
```

Optimal order:

```js
Cut at 3
Cut at 5
Cut at 1
Cut at 4
```

---

# 7. Complexity Analysis

Let:

```js
m = cuts.length + 2;
```

### States

```js
O(m²)
```

### Transition

For each state, try all cuts:

```js
O(m);
```

### Total

```js
O(m³)
```

### Space

```js
O(m²)
```

---

# 8. Edge Cases

### No Cuts

```js
n = 10;
cuts = [];
```

Answer:

```js
0;
```

---

### One Cut

```js
n = 9;
cuts = [4];
```

Cost:

```js
9;
```

---

### Cuts Already Sorted

```js
[1, 2, 3];
```

Still sort defensively.

---

### Duplicate Cuts

If duplicates are possible:

```js
cuts = [...new Set(cuts)];
```

before processing.

---

# 9. Interview Tips

- Recognize this as an **Interval DP** problem.
- Mention its similarity to:
  - Matrix Chain Multiplication
  - Burst Balloons
  - Optimal BST

- State the recurrence clearly:

[
dp(i,j)=\min\_{k=i+1}^{j-1}
\Big((cuts[j]-cuts[i]) + dp(i,k)+dp(k,j)\Big)
]

dp(i,j)=\min\_{i<k<j}\left((cuts[j]-cuts[i])+dp(i,k)+dp(k,j)\right)

- Expected interview solution: **O(m³) time, O(m²) space**.

## Question 3. Longest increasing subsequence (with reconstruction)

## Question 4. Egg dropping problem (minimum attempts)

## Question 5. Weighted job scheduling for maximum profit

## Question 6. Longest common substring

## Question 7. Longest palindromic subsequence

## Question 8. Minimum insertions to make string palindrome

## Question 9. Count number of ways to reach sum with unlimited coins

## Question 10. Russian doll envelopes (envelope nesting problem)

## Question 11. N-Queens problem (backtracking)

## Question 12. Rat in a maze (all paths)

## Question 13. Maximum area of island in 2D grid

## Question 14. Shortest bridge between two islands

## Question 15. Longest increasing path in a matrix

## Question 16. Trapping rainwater II (2D)

## Question 17. Design LRU cache using hash map + doubly linked list

## Question 18. Maximum XOR of two numbers in array

## Question 19. Word search in a grid

## Question 20. Sliding window median
