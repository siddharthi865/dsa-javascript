# Set 20

| S.No. | Question                                                                                                                                     |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Maximum sum increasing subsequence](#question-1-maximum-sum-increasing-subsequence)                                                         |
| 2.    | [Count number of ways to reach target sum using unlimited coins](#question-2-count-number-of-ways-to-reach-target-sum-using-unlimited-coins) |
| 3.    | [Longest palindromic subsequence](#question-3-longest-palindromic-subsequence)                                                               |
| 4.    | [Minimum cuts to partition palindrome substrings](#question-4-minimum-cuts-to-partition-palindrome-substrings)                               |
| 5.    | [Matrix chain multiplication (min cost)](#question-5-matrix-chain-multiplication-min-cost)                                                   |
| 6.    | [Longest common substring](#question-6-longest-common-substring)                                                                             |
| 7.    | [Maximum sum rectangle in 2D matrix](#question-7-maximum-sum-rectangle-in-2d-matrix)                                                         |
| 8.    | [Maximum product subarray](#question-8-maximum-product-subarray)                                                                             |
| 9.    | [Egg dropping problem (min trials)](#question-9-egg-dropping-problem-min-trials)                                                             |
| 10.   | [Weighted job scheduling problem](#question-10-weighted-job-scheduling-problem)                                                              |
| 11.   | [Trapping rainwater II (2D matrix)](#question-11-trapping-rainwater-ii-2d-matrix)                                                            |
| 12.   | [Maximum area of island in 2D grid](#question-12-maximum-area-of-island-in-2d-grid)                                                          |
| 13.   | [Shortest bridge between two islands](#question-13-shortest-bridge-between-two-islands)                                                      |
| 14.   | [Longest increasing path in a matrix](#question-14-longest-increasing-path-in-a-matrix)                                                      |
| 15.   | [Word search problem in grid](#question-15-word-search-problem-in-grid)                                                                      |
| 16.   | [Sliding window maximum/minimum in array](#question-16-sliding-window-maximumminimum-in-array)                                               |
| 17.   | [Implement LRU cache](#question-17-implement-lru-cache)                                                                                      |
| 18.   | [Maximum XOR of two numbers in array](#question-18-maximum-xor-of-two-numbers-in-array)                                                      |
| 19.   | [N-Queens problem](#question-19-n-queens-problem)                                                                                            |
| 20.   | [Rat in a maze problem (all paths)](#question-20-rat-in-a-maze-problem-all-paths)                                                            |

## Question 1. Maximum sum increasing subsequence

# Maximum Sum Increasing Subsequence (MSIS)

## Concise Answer

Given an array, find an **increasing subsequence** such that the **sum of its elements is maximum**.

Example:

```text
Input:  [1, 101, 2, 3, 100, 4, 5]
Output: 106

Explanation:
Increasing subsequence = [1, 2, 3, 100]
Sum = 106
```

The most common solution uses **Dynamic Programming** in **O(n²)** time.

---

# 1. Problem Understanding

Unlike the Longest Increasing Subsequence (LIS), where we maximize the **length**, here we maximize the **sum**.

For every element:

- Either start a new subsequence from it.
- Or extend a previous increasing subsequence.

---

# 2. Approach 1: Dynamic Programming (O(n²))

## Idea

Let:

```js
dp[i];
```

represent the **maximum sum of an increasing subsequence ending at index i**.

Initially:

```js
dp[i] = arr[i];
```

because every element itself forms an increasing subsequence.

For every previous element:

```js
if (arr[j] < arr[i])
```

then we can append `arr[i]` to the subsequence ending at `j`.

Transition:

```js
dp[i] = Math.max(dp[i], dp[j] + arr[i]);
```

Answer:

```js
max(dp);
```

---

## Dry Run

```js
arr = [1, 101, 2, 3, 100];
```

Initial:

```js
dp = [1, 101, 2, 3, 100];
```

### i = 1 (101)

```js
1 < 101

dp[1] = max(101, 1 + 101)
      = 102
```

```js
dp = [1, 102, 2, 3, 100];
```

### i = 2 (2)

```js
1 < 2

dp[2] = max(2,1+2)
      = 3
```

```js
dp = [1, 102, 3, 3, 100];
```

### i = 3 (3)

```js
1 < 3 → 4
2 < 3 → 6
```

```js
dp[3] = 6;
```

### i = 4 (100)

```js
1 <100 →101
2 <100 →103
3 <100 →106
```

```js
dp[4] = 106;
```

Answer:

```js
106;
```

---

# 3. JavaScript Solution (O(n²))

```js
const maxSumIncreasingSubsequence = (arr) => {
  const n = arr.length;

  if (n === 0) return 0;

  const dp = [...arr];

  let maxSum = arr[0];

  for (let i = 1; i < n; i++) {
    for (let j = 0; j < i; j++) {
      if (arr[j] < arr[i]) {
        dp[i] = Math.max(dp[i], dp[j] + arr[i]);
      }
    }

    maxSum = Math.max(maxSum, dp[i]);
  }

  return maxSum;
};
```

### Example

```js
console.log(maxSumIncreasingSubsequence([1, 101, 2, 3, 100, 4, 5]));
```

Output:

```js
106;
```

---

# 4. Approach 2: Print the Actual Subsequence

Interviewers sometimes ask:

> "Can you also return the subsequence?"

Maintain a parent array.

## Idea

```js
parent[i];
```

stores the previous index used to build the optimal sum ending at `i`.

---

## JavaScript

```js
const maxSumISWithSequence = (arr) => {
  const n = arr.length;

  const dp = [...arr];
  const parent = Array(n).fill(-1);

  let maxIndex = 0;

  for (let i = 1; i < n; i++) {
    for (let j = 0; j < i; j++) {
      if (arr[j] < arr[i] && dp[j] + arr[i] > dp[i]) {
        dp[i] = dp[j] + arr[i];
        parent[i] = j;
      }
    }

    if (dp[i] > dp[maxIndex]) {
      maxIndex = i;
    }
  }

  const sequence = [];

  let curr = maxIndex;

  while (curr !== -1) {
    sequence.push(arr[curr]);
    curr = parent[curr];
  }

  sequence.reverse();

  return {
    maxSum: dp[maxIndex],
    sequence,
  };
};
```

### Example

```js
console.log(maxSumISWithSequence([1, 101, 2, 3, 100, 4, 5]));
```

Output:

```js
{
  maxSum: 106,
  sequence: [1, 2, 3, 100]
}
```

---

# 5. Complexity Analysis

### DP Solution

| Operation | Complexity |
| --------- | ---------- |
| Time      | O(n²)      |
| Space     | O(n)       |

---

# 6. Edge Cases

### Single Element

```js
[10];
```

Output:

```js
10;
```

---

### Strictly Decreasing

```js
[10, 9, 8, 7];
```

Output:

```js
10;
```

Each element alone is an increasing subsequence.

---

### All Equal

```js
[5, 5, 5];
```

Output:

```js
5;
```

Because subsequence must be strictly increasing.

---

### Negative Numbers

```js
[-5, -2, -8];
```

Output:

```js
-2;
```

The maximum sum subsequence is simply `[-2]`.

---

# 7. Interview Tips

- Clearly distinguish **MSIS** from **LIS**:
  - LIS maximizes **length**
  - MSIS maximizes **sum**

- State the DP definition first:

  ```js
  dp[i] = maximum sum of an increasing subsequence ending at i
  ```

- Initialize:

  ```js
  dp[i] = arr[i];
  ```

  because each element can form a subsequence by itself.

- If asked to print the sequence, maintain a `parent` array and reconstruct the path.

This is the standard interview solution and is expected in most DSA interviews.

## Question 2. Count number of ways to reach target sum using unlimited coins

# Count Number of Ways to Reach Target Sum Using Unlimited Coins (Coin Change II)

## Concise Answer

Given a set of coin denominations and a target sum, count the number of **distinct combinations** to make the target when each coin can be used **unlimited times**.

Example:

```text
Coins = [1, 2, 5]
Target = 5

Ways:
1+1+1+1+1
1+1+1+2
1+2+2
5

Output: 4
```

This is a classic **Unbounded Knapsack / Coin Change II** problem and is typically solved using Dynamic Programming.

---

# 1. Problem Understanding

Given:

```js
coins = [c1, c2, c3, ...]
target = T
```

Find:

```text
Number of combinations that sum to T
```

Important:

- Coins can be used unlimited times.
- Order does NOT matter.

Example:

```text
1 + 2 + 2
2 + 1 + 2
2 + 2 + 1
```

These count as **one combination**, not three.

---

# 2. Approach 1: Recursive Solution (Brute Force)

For each coin:

1. Include the coin.
2. Exclude the coin.

Recurrence:

```text
ways(i, sum)
=
ways(i, sum - coin[i])     // include
+
ways(i + 1, sum)           // exclude
```

### Base Cases

```text
sum == 0  → 1
sum < 0   → 0
i == n    → 0
```

### JavaScript

```js
const countWays = (coins, index, target) => {
  if (target === 0) return 1;
  if (target < 0) return 0;
  if (index === coins.length) return 0;

  return (
    countWays(coins, index, target - coins[index]) +
    countWays(coins, index + 1, target)
  );
};
```

### Complexity

```text
Time: Exponential
Space: O(target)
```

Not suitable for large inputs.

---

# 3. Approach 2: Memoization (Top-Down DP)

Store already computed states:

```js
(index, target);
```

### JavaScript

```js
const coinChangeWays = (coins, target) => {
  const memo = new Map();

  const dfs = (index, remain) => {
    if (remain === 0) return 1;
    if (remain < 0) return 0;
    if (index === coins.length) return 0;

    const key = `${index}-${remain}`;

    if (memo.has(key)) {
      return memo.get(key);
    }

    const include = dfs(index, remain - coins[index]);

    const exclude = dfs(index + 1, remain);

    memo.set(key, include + exclude);

    return memo.get(key);
  };

  return dfs(0, target);
};
```

### Complexity

```text
Time: O(n × target)
Space: O(n × target)
```

---

# 4. Approach 3: Bottom-Up DP (2D)

## DP Definition

```js
dp[i][j];
```

= number of ways to make sum `j`
using first `i` coin types.

### Transition

Exclude current coin:

```text
dp[i-1][j]
```

Include current coin:

```text
dp[i][j - coin]
```

Therefore:

```text
dp[i][j] =
dp[i-1][j] +
dp[i][j-coin]
```

---

### JavaScript

```js
const coinChangeWays = (coins, target) => {
  const n = coins.length;

  const dp = Array.from({ length: n + 1 }, () => Array(target + 1).fill(0));

  for (let i = 0; i <= n; i++) {
    dp[i][0] = 1;
  }

  for (let i = 1; i <= n; i++) {
    const coin = coins[i - 1];

    for (let sum = 1; sum <= target; sum++) {
      dp[i][sum] = dp[i - 1][sum];

      if (sum >= coin) {
        dp[i][sum] += dp[i][sum - coin];
      }
    }
  }

  return dp[n][target];
};
```

### Complexity

```text
Time: O(n × target)
Space: O(n × target)
```

---

# 5. Approach 4: Space Optimized DP (Best Solution)

Observe:

```text
dp[i][*]
depends only on:
dp[i][*]
and
dp[i-1][*]
```

We can reduce to a 1D array.

---

## DP Definition

```js
dp[s];
```

= number of ways to make sum `s`.

Initialize:

```js
dp[0] = 1;
```

because there is one way to make sum 0 (choose nothing).

---

### Transition

For each coin:

```js
dp[sum] += dp[sum - coin];
```

---

### JavaScript (Recommended)

```js
const coinChangeWays = (coins, target) => {
  const dp = Array(target + 1).fill(0);

  dp[0] = 1;

  for (const coin of coins) {
    for (let sum = coin; sum <= target; sum++) {
      dp[sum] += dp[sum - coin];
    }
  }

  return dp[target];
};
```

---

## Dry Run

```text
coins = [1,2,5]
target = 5
```

Initial:

```text
dp = [1,0,0,0,0,0]
```

After coin = 1:

```text
[1,1,1,1,1,1]
```

After coin = 2:

```text
[1,1,2,2,3,3]
```

After coin = 5:

```text
[1,1,2,2,3,4]
```

Answer:

```text
4
```

---

# 6. Why Coin Loop Must Be Outside?

Correct:

```js
for (const coin of coins) {
  for (let sum = coin; sum <= target; sum++) {
    dp[sum] += dp[sum - coin];
  }
}
```

This counts **combinations**.

---

Incorrect:

```js
for (let sum = 1; sum <= target; sum++) {
    for (const coin of coins) {
        ...
    }
}
```

This counts **permutations** (different orders separately).

Example:

```text
1+2
2+1
```

would be counted twice.

This is a very common interview follow-up.

---

# 7. Complexity Analysis

### Space Optimized DP

| Metric | Complexity    |
| ------ | ------------- |
| Time   | O(n × target) |
| Space  | O(target)     |

where:

```text
n = number of coin types
```

---

# 8. Edge Cases

### Target = 0

```js
coins = [1, 2, 5];
target = 0;
```

Output:

```text
1
```

(Choose nothing)

---

### No Coins

```js
coins = [];
target = 5;
```

Output:

```text
0
```

---

### Coin Larger Than Target

```js
coins = [10];
target = 5;
```

Output:

```text
0
```

---

### Single Coin

```js
coins = [2];
target = 8;
```

Output:

```text
1
```

Only:

```text
2+2+2+2
```

---

# Interview Tips

- Recognize this as **Coin Change II** (count ways), not Coin Change I (minimum coins).
- State the recurrence:

  ```text
  ways = include + exclude
  ```

- Mention that this is an **Unbounded Knapsack** because coins can be reused.
- For optimal interviews, present the **1D DP solution**:

  ```js
  dp[sum] += dp[sum - coin];
  ```

- Be ready to explain why iterating coins first avoids counting permutations.

## Question 3. Longest palindromic subsequence

## Question 4. Minimum cuts to partition palindrome substrings

## Question 5. Matrix chain multiplication (min cost)

## Question 6. Longest common substring

## Question 7. Maximum sum rectangle in 2D matrix

## Question 8. Maximum product subarray

## Question 9. Egg dropping problem (min trials)

## Question 10. Weighted job scheduling problem

## Question 11. Trapping rainwater II (2D matrix)

## Question 12. Maximum area of island in 2D grid

## Question 13. Shortest bridge between two islands

## Question 14. Longest increasing path in a matrix

## Question 15. Word search problem in grid

## Question 16. Sliding window maximum/minimum in array

## Question 17. Implement LRU cache

## Question 18. Maximum XOR of two numbers in array

## Question 19. N-Queens problem

## Question 20. Rat in a maze problem (all paths)
