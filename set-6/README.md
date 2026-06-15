# Set 6

| S.No. | Question                                                                                                                                     |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Find the majority element in an array](#question-1-find-the-majority-element-in-an-array)                                                   |
| 2.    | [Move all zeros to the end of an array](#question-2-move-all-zeros-to-the-end-of-an-array)                                                   |
| 3.    | [Merge two sorted arrays without extra space](#question-3-merge-two-sorted-arrays-without-extra-space)                                       |
| 4.    | [Find the first repeating element in an array](#question-4-find-the-first-repeating-element-in-an-array)                                     |
| 5.    | [Find the missing number in a sequence of 1 to n](#question-5-find-the-missing-number-in-a-sequence-of-1-to-n)                               |
| 6.    | [Find the repeating and missing number in an array](#question-6-find-the-repeating-and-missing-number-in-an-array)                           |
| 7.    | [Find the union of two arrays](#question-7-find-the-union-of-two-arrays)                                                                     |
| 8.    | [Find the intersection of two arrays](#question-8-find-the-intersection-of-two-arrays)                                                       |
| 9.    | [Find the peak element in an array](#question-9-find-the-peak-element-in-an-array)                                                           |
| 10.   | [Check if an array is a subset of another array](#question-10-check-if-an-array-is-a-subset-of-another-array)                                |
| 11.   | [Check if two strings are anagrams](#question-11-check-if-two-strings-are-anagrams)                                                          |
| 12.   | [Count the number of words in a string](#question-12-count-the-number-of-words-in-a-string)                                                  |
| 13.   | [Reverse words in a string](#question-13-reverse-words-in-a-string)                                                                          |
| 14.   | [Remove duplicate characters from a string](#question-14-remove-duplicate-characters-from-a-string)                                          |
| 15.   | [Longest substring without repeating characters](#question-15-longest-substring-without-repeating-characters)                                |
| 16.   | [Check if a string is a rotation of another string](#question-16-check-if-a-string-is-a-rotation-of-another-string)                          |
| 17.   | [Find all permutations of a string](#question-17-find-all-permutations-of-a-string)                                                          |
| 18.   | [Implement a function to compress a string (like "aaabb" → "a3b2")](#question-18-implement-a-function-to-compress-a-string-like-aaabb--a3b2) |
| 19.   | [Convert a string to an integer (implement atoi)](#question-19-convert-a-string-to-an-integer-implement-atoi)                                |
| 20.   | [Implement strstr without library functions](#question-20-implement-strstr-without-library-functions)                                        |

## Question 1. Find the majority element in an array

# Find the Majority Element in an Array

## Concise Answer

A **majority element** is an element that appears **more than ⌊n/2⌋ times** in an array of size `n`.

The most optimal solution is **Boyer-Moore Voting Algorithm**, which runs in:

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

---

# 1. Problem Understanding

### Example

```js
Input: [2, 2, 1, 1, 1, 2, 2];
Output: 2;
```

`2` appears 4 times out of 7, which is more than `7/2 = 3.5`.

### Assumptions

Interviewers may ask either:

1. **Majority element is guaranteed to exist** (common version).
2. **Majority element may not exist** (follow-up question).

Always clarify this in an interview.

---

# 2. Approach 1: Brute Force

For each element, count its occurrences.

### Idea

- Pick every element.
- Count how many times it appears.
- If count > n/2, return it.

### JavaScript

```js
const majorityElement = (nums) => {
  const n = nums.length;

  for (let i = 0; i < n; i++) {
    let count = 0;

    for (let j = 0; j < n; j++) {
      if (nums[i] === nums[j]) {
        count++;
      }
    }

    if (count > Math.floor(n / 2)) {
      return nums[i];
    }
  }

  return -1;
};
```

### Complexity

- Time: `O(n²)`
- Space: `O(1)`

---

# 3. Approach 2: Hash Map Counting

Store frequencies in a map.

### Idea

1. Traverse array.
2. Count occurrences.
3. Return element whose count exceeds `n/2`.

### JavaScript

```js
const majorityElement = (nums) => {
  const freq = new Map();
  const limit = Math.floor(nums.length / 2);

  for (const num of nums) {
    freq.set(num, (freq.get(num) || 0) + 1);

    if (freq.get(num) > limit) {
      return num;
    }
  }

  return -1;
};
```

### Complexity

- Time: `O(n)`
- Space: `O(n)`

### When to Use

Good when:

- Need frequency information.
- Multiple queries on counts are required.

---

# 4. Approach 3: Boyer-Moore Voting Algorithm (Optimal)

### Core Insight

If the majority element appears more than half the time, it can never be completely canceled out by all other elements combined.

Maintain:

- `candidate`
- `count`

### Rules

1. If count becomes `0`, choose current element as candidate.
2. If current element equals candidate → increment count.
3. Otherwise → decrement count.

---

### Dry Run

```js
[2, 2, 1, 1, 1, 2, 2];
```

| Element | Candidate | Count |
| ------- | --------- | ----- |
| 2       | 2         | 1     |
| 2       | 2         | 2     |
| 1       | 2         | 1     |
| 1       | 2         | 0     |
| 1       | 1         | 1     |
| 2       | 1         | 0     |
| 2       | 2         | 1     |

Final candidate = `2`

---

### JavaScript

```js
const majorityElement = (nums) => {
  let candidate = null;
  let count = 0;

  for (const num of nums) {
    if (count === 0) {
      candidate = num;
    }

    count += num === candidate ? 1 : -1;
  }

  return candidate;
};
```

### Complexity

- Time: `O(n)`
- Space: `O(1)`

This is the solution interviewers usually expect.

---

# 5. Follow-Up: What If Majority Element Is Not Guaranteed?

Boyer-Moore only gives a **candidate**.

We must verify it.

### JavaScript

```js
const majorityElement = (nums) => {
  let candidate = null;
  let count = 0;

  for (const num of nums) {
    if (count === 0) {
      candidate = num;
    }

    count += num === candidate ? 1 : -1;
  }

  count = 0;

  for (const num of nums) {
    if (num === candidate) {
      count++;
    }
  }

  return count > Math.floor(nums.length / 2) ? candidate : -1;
};
```

### Complexity

- Time: `O(n)`
- Space: `O(1)`

---

# Edge Cases

### Single Element

```js
[5];
```

Output:

```js
5;
```

---

### All Same Elements

```js
[3, 3, 3, 3];
```

Output:

```js
3;
```

---

### No Majority Element

```js
[1, 2, 3, 4];
```

Output:

```js
-1;
```

(when verification step is included)

---

### Negative Numbers

```js
[-1, -1, -1, 2, 3];
```

Output:

```js
-1;
```

---

# Interview Tips

### Why does Boyer-Moore work?

Think of it as **pairwise cancellation**:

```text
Majority Element : A A A A A
Others           : B C D E
```

Each non-majority element cancels one occurrence of the majority element.

Since the majority element appears more than `n/2` times, it still survives after all cancellations.

### What interviewers often ask next

1. Find element occurring more than `n/3` times.
   - Use extended Boyer-Moore.
   - At most 2 such elements can exist.

2. Find all elements occurring more than `n/k` times.
   - Generalized voting algorithm.

---

## Recommended Interview Answer

> "I would use the Boyer-Moore Voting Algorithm. It maintains a candidate and a counter, canceling out different elements as we traverse the array. Because a majority element appears more than half the time, it survives all cancellations. This gives an O(n) time and O(1) space solution, which is optimal."

## Question 2. Move all zeros to the end of an array

# Move All Zeros to the End of an Array

## Concise Answer

Given an array, move all `0`s to the end while maintaining the relative order of non-zero elements.

The optimal solution uses the **Two-Pointer Technique**:

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

---

# 1. Problem Understanding

### Example

```js
Input: [0, 1, 0, 3, 12];
Output: [1, 3, 12, 0, 0];
```

Requirements usually are:

1. Move all zeros to the end.
2. Preserve the order of non-zero elements.
3. Do it in-place if possible.

---

# 2. Approach 1: Extra Array

### Idea

- Store all non-zero elements in a new array.
- Append zeros at the end.
- Copy back if needed.

### JavaScript

```js
const moveZeroes = (nums) => {
  const result = [];

  for (const num of nums) {
    if (num !== 0) {
      result.push(num);
    }
  }

  while (result.length < nums.length) {
    result.push(0);
  }

  return result;
};
```

### Complexity

- Time: `O(n)`
- Space: `O(n)`

### Example

```js
moveZeroes([0, 1, 0, 3, 12]);
// [1, 3, 12, 0, 0]
```

---

# 3. Approach 2: Two-Pointer (Optimal)

### Idea

Maintain a pointer `insertPos` indicating where the next non-zero element should go.

1. Traverse the array.
2. Whenever a non-zero element is found, place it at `insertPos`.
3. Increment `insertPos`.
4. After traversal, fill remaining positions with zeros.

---

### Dry Run

```js
[0, 1, 0, 3, 12];
```

| Element | insertPos | Array State   |
| ------- | --------- | ------------- |
| 0       | 0         | [0,1,0,3,12]  |
| 1       | 1         | [1,1,0,3,12]  |
| 0       | 1         | [1,1,0,3,12]  |
| 3       | 2         | [1,3,0,3,12]  |
| 12      | 3         | [1,3,12,3,12] |

Fill remaining positions with zeros:

```js
[1, 3, 12, 0, 0];
```

---

### JavaScript

```js
const moveZeroes = (nums) => {
  let insertPos = 0;

  for (let i = 0; i < nums.length; i++) {
    if (nums[i] !== 0) {
      nums[insertPos] = nums[i];
      insertPos++;
    }
  }

  while (insertPos < nums.length) {
    nums[insertPos] = 0;
    insertPos++;
  }

  return nums;
};
```

### Complexity

- Time: `O(n)`
- Space: `O(1)`

---

# 4. Approach 3: Swap-Based Two Pointers

### Idea

Keep:

- `left` → position for next non-zero
- `right` → current element

Whenever a non-zero is found, swap it with `left`.

---

### JavaScript

```js
const moveZeroes = (nums) => {
  let left = 0;

  for (let right = 0; right < nums.length; right++) {
    if (nums[right] !== 0) {
      [nums[left], nums[right]] = [nums[right], nums[left]];
      left++;
    }
  }

  return nums;
};
```

### Example

```js
moveZeroes([0, 1, 0, 3, 12]);
// [1, 3, 12, 0, 0]
```

### Complexity

- Time: `O(n)`
- Space: `O(1)`
- Performs more writes/swaps than Approach 2.

---

# Complexity Comparison

| Approach         | Time | Space |
| ---------------- | ---- | ----- |
| Extra Array      | O(n) | O(n)  |
| Two-Pointer Fill | O(n) | O(1)  |
| Swap-Based       | O(n) | O(1)  |

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

---

### All Zeros

```js
[0, 0, 0];
```

Output:

```js
[0, 0, 0];
```

---

### No Zeros

```js
[1, 2, 3];
```

Output:

```js
[1, 2, 3];
```

---

### Single Element

```js
[0];
```

Output:

```js
[0];
```

```js
[5];
```

Output:

```js
[5];
```

---

# Common Pitfalls

### ❌ Using `splice()` repeatedly

```js
nums.splice(i, 1);
nums.push(0);
```

This shifts elements every time and can degrade to **O(n²)**.

### ❌ Not preserving order

Some solutions move zeros correctly but change the order of non-zero elements, which usually violates the problem requirement.

---

# Interview Tips

When asked this question, mention:

> "Since the relative order of non-zero elements must remain unchanged, I'll use a two-pointer approach. One pointer tracks where the next non-zero element should be placed, and after processing all elements, the remaining positions are filled with zeros. This achieves O(n) time and O(1) extra space."

This is the most commonly expected interview solution.

## Question 3. Merge two sorted arrays without extra space

## Question 4. Find the first repeating element in an array

## Question 5. Find the missing number in a sequence of 1 to n

## Question 6. Find the repeating and missing number in an array

## Question 7. Find the union of two arrays

## Question 8. Find the intersection of two arrays

## Question 9. Find the peak element in an array

## Question 10. Check if an array is a subset of another array

## Question 11. Check if two strings are anagrams

## Question 12. Count the number of words in a string

## Question 13. Reverse words in a string

## Question 14. Remove duplicate characters from a string

## Question 15. Longest substring without repeating characters

## Question 16. Check if a string is a rotation of another string

## Question 17. Find all permutations of a string

## Question 18. Implement a function to compress a string (like "aaabb" → "a3b2")

## Question 19. Convert a string to an integer (implement atoi)

## Question 20. Implement strstr without library functions
