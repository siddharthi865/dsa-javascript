# Set 24

| S.No. | Question                                                                                                                                                 |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Search in rotated sorted array with duplicates](#question-1-search-in-rotated-sorted-array-with-duplicates)                                             |
| 2.    | [Find median of two sorted arrays](#question-2-find-median-of-two-sorted-arrays)                                                                         |
| 3.    | [Find K closest elements to a target](#question-3-find-k-closest-elements-to-a-target)                                                                   |
| 4.    | [Allocate minimum number of pages (book allocation problem)](#question-4-allocate-minimum-number-of-pages-book-allocation-problem)                       |
| 5.    | [Painter's partition problem](#question-5-painters-partition-problem)                                                                                    |
| 6.    | [Quickselect algorithm to find kth smallest/largest](#question-6-quickselect-algorithm-to-find-kth-smallestlargest)                                      |
| 7.    | [Search in 2D matrix where each row and column is sorted](#question-7-search-in-2d-matrix-where-each-row-and-column-is-sorted)                           |
| 8.    | [Count number of elements smaller than or equal to X in sorted matrix](#question-8-count-number-of-elements-smaller-than-or-equal-to-x-in-sorted-matrix) |
| 9.    | [Find peak element in 2D matrix](#question-9-find-peak-element-in-2d-matrix)                                                                             |
| 10.   | [Maximum sum of i\*arr[i] after rotations](#question-10-maximum-sum-of-iarri-after-rotations)                                                            |
| 11.   | [Longest subarray with sum divisible by K (negative numbers included)](#question-11-longest-subarray-with-sum-divisible-by-k-negative-numbers-included)  |
| 12.   | [Count pairs with given XOR](#question-12-count-pairs-with-given-xor)                                                                                    |
| 13.   | [Subarray with sum closest to zero](#question-13-subarray-with-sum-closest-to-zero)                                                                      |
| 14.   | [Count number of subarrays with XOR = K](#question-14-count-number-of-subarrays-with-xor--k)                                                             |
| 15.   | [Check if array can be divided into pairs with sum divisible by K](#question-15-check-if-array-can-be-divided-into-pairs-with-sum-divisible-by-k)        |
| 16.   | [Longest consecutive sequence](#question-16-longest-consecutive-sequence)                                                                                |
| 17.   | [Group anagrams together](#question-17-group-anagrams-together)                                                                                          |
| 18.   | [Find first non-repeating element in stream](#question-18-find-first-non-repeating-element-in-stream)                                                    |
| 19.   | [Maximum frequency element in subarrays of size K](#question-19-maximum-frequency-element-in-subarrays-of-size-k)                                        |
| 20.   | [Count all subarrays with distinct elements](#question-20-count-all-subarrays-with-distinct-elements)                                                    |

## Question 1. Search in rotated sorted array with duplicates

# Search in Rotated Sorted Array with Duplicates

## Direct Answer

Given a rotated sorted array that may contain duplicates, find whether a target exists in the array.

Unlike the version without duplicates, we cannot always determine which half is sorted because duplicates can make both ends appear identical. In such cases, we shrink the search space by moving the pointers inward.

---

# 1. Problem Understanding

### Example

```text
Input:
nums = [2,5,6,0,0,1,2]
target = 0

Output:
true
```

```text
Input:
nums = [2,5,6,0,0,1,2]
target = 3

Output:
false
```

### Why Duplicates Make It Hard

Consider:

```text
[1,1,1,3,1]
```

At some point:

```text
left = 0, mid = 2, right = 4

nums[left] = 1
nums[mid] = 1
nums[right] = 1
```

We cannot determine which side is sorted.

So we remove duplicates:

```js
left++;
right--;
```

---

# 2. Approach: Modified Binary Search

### Idea

At each step:

1. Find middle element.
2. If target found → return true.
3. If left, mid, and right are equal:
   - shrink search space.

4. Otherwise determine which half is sorted.
5. Check whether target lies in that sorted half.
6. Discard the other half.

---

## Algorithm

```text
while left <= right

    mid = middle

    if nums[mid] == target
        return true

    if nums[left] == nums[mid] == nums[right]
        left++
        right--

    else if left half sorted
        if target lies in left half
            move right
        else
            move left

    else
        right half sorted

        if target lies in right half
            move left
        else
            move right
```

---

# 3. JavaScript Solution

```javascript
const search = (nums, target) => {
  let left = 0;
  let right = nums.length - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    if (nums[mid] === target) {
      return true;
    }

    // Duplicates block decision
    if (nums[left] === nums[mid] && nums[mid] === nums[right]) {
      left++;
      right--;
    }

    // Left half sorted
    else if (nums[left] <= nums[mid]) {
      if (target >= nums[left] && target < nums[mid]) {
        right = mid - 1;
      } else {
        left = mid + 1;
      }
    }

    // Right half sorted
    else {
      if (target > nums[mid] && target <= nums[right]) {
        left = mid + 1;
      } else {
        right = mid - 1;
      }
    }
  }

  return false;
};
```

---

# 4. Example Walkthrough

### Input

```js
nums = [2, 5, 6, 0, 0, 1, 2];
target = 0;
```

### Iteration 1

```text
left = 0
right = 6
mid = 3

nums[mid] = 0
```

Target found immediately.

```text
Return true
```

---

### Example with Duplicates

```js
nums = [1, 1, 1, 3, 1];
target = 3;
```

Initially:

```text
left = 0
mid = 2
right = 4

1 == 1 == 1
```

Shrink:

```text
left++
right--
```

Now search continues and eventually reaches:

```text
nums[mid] = 3
```

Return:

```text
true
```

---

# 5. Complexity Analysis

### Average Case

```text
Time: O(log n)
Space: O(1)
```

### Worst Case

When many duplicates exist:

```text
[1,1,1,1,1,1,1]
```

or

```text
[1,1,1,1,2,1,1]
```

we may shrink one element at a time.

```text
Time: O(n)
Space: O(1)
```

---

# 6. Edge Cases

### All elements same

```js
[1, 1, 1, 1, 1];
target = 1;
```

Output:

```text
true
```

---

### Target absent

```js
[1, 1, 1, 1, 1];
target = 2;
```

Output:

```text
false
```

---

### Not rotated

```js
[1, 2, 3, 4, 5];
target = 4;
```

Output:

```text
true
```

---

### Single element

```js
[5];
target = 5;
```

Output:

```text
true
```

---

# 7. Interview Tips

- First explain the classic "Search in Rotated Sorted Array" using binary search.
- Mention that duplicates break the ability to identify the sorted half in some cases.
- Handle the ambiguous case using:

```js
if (nums[left] === nums[mid] && nums[mid] === nums[right]) {
  left++;
  right--;
}
```

- Be clear that:
  - Average complexity remains **O(log n)**.
  - Worst-case complexity degrades to **O(n)** because duplicates may force linear shrinking.

This is the standard interview-expected solution for **LeetCode 81 — Search in Rotated Sorted Array II**.

## Question 2. Find median of two sorted arrays

# Find Median of Two Sorted Arrays

## Direct Answer

Given two sorted arrays `nums1` and `nums2`, find the median of the combined sorted array **without actually merging them**.

The optimal solution uses **Binary Search on the smaller array** and runs in **O(log(min(m, n)))** time with **O(1)** extra space.

---

# 1. Problem Understanding

### Example 1

```js
nums1 = [1, 3];
nums2 = [2];
```

Merged array:

```js
[1, 2, 3];
```

Median:

```js
2;
```

---

### Example 2

```js
nums1 = [1, 2];
nums2 = [3, 4];
```

Merged array:

```js
[1, 2, 3, 4];
```

Median:

```js
(2 + 3) / 2 = 2.5
```

---

# 2. Approach 1: Merge Then Find Median

### Idea

Merge both sorted arrays like Merge Sort and then compute the median.

### Algorithm

1. Merge arrays.
2. Find total length.
3. If odd → middle element.
4. If even → average of two middle elements.

---

## JavaScript Code

```javascript
const findMedianSortedArrays = (nums1, nums2) => {
  const merged = [];

  let i = 0;
  let j = 0;

  while (i < nums1.length && j < nums2.length) {
    if (nums1[i] <= nums2[j]) {
      merged.push(nums1[i++]);
    } else {
      merged.push(nums2[j++]);
    }
  }

  while (i < nums1.length) merged.push(nums1[i++]);
  while (j < nums2.length) merged.push(nums2[j++]);

  const n = merged.length;

  if (n % 2 === 1) {
    return merged[Math.floor(n / 2)];
  }

  return (merged[n / 2] + merged[n / 2 - 1]) / 2;
};
```

### Complexity

```text
Time: O(m + n)
Space: O(m + n)
```

---

# 3. Approach 2: Optimal Binary Search Partition

### Key Insight

Instead of merging:

Find a partition such that:

```text
Left Half Size = Right Half Size
```

and

```text
max(left part) <= min(right part)
```

Once such a partition is found, the median can be computed immediately.

---

## Visualization

```text
nums1 = [1, 3]
nums2 = [2]

Partition:

[1 | 3]
[2 | ]

Left Side  = [1,2]
Right Side = [3]
```

```text
maxLeft = 2
minRight = 3
```

Valid partition.

Median = 2

---

## Partition Logic

Suppose:

```text
cut1 = partition in nums1
cut2 = partition in nums2
```

Then:

```text
left1  = nums1[cut1 - 1]
right1 = nums1[cut1]

left2  = nums2[cut2 - 1]
right2 = nums2[cut2]
```

A valid partition satisfies:

```text
left1 <= right2
and
left2 <= right1
```

---

# 4. Optimal JavaScript Solution

```javascript
const findMedianSortedArrays = (nums1, nums2) => {
  if (nums1.length > nums2.length) {
    return findMedianSortedArrays(nums2, nums1);
  }

  const m = nums1.length;
  const n = nums2.length;

  let left = 0;
  let right = m;

  while (left <= right) {
    const cut1 = Math.floor((left + right) / 2);
    const cut2 = Math.floor((m + n + 1) / 2) - cut1;

    const left1 = cut1 === 0 ? -Infinity : nums1[cut1 - 1];

    const right1 = cut1 === m ? Infinity : nums1[cut1];

    const left2 = cut2 === 0 ? -Infinity : nums2[cut2 - 1];

    const right2 = cut2 === n ? Infinity : nums2[cut2];

    if (left1 <= right2 && left2 <= right1) {
      // Odd total length
      if ((m + n) % 2 === 1) {
        return Math.max(left1, left2);
      }

      // Even total length
      return (Math.max(left1, left2) + Math.min(right1, right2)) / 2;
    }

    if (left1 > right2) {
      right = cut1 - 1;
    } else {
      left = cut1 + 1;
    }
  }

  return -1;
};
```

---

# 5. Dry Run

### Input

```js
nums1 = [1, 3];
nums2 = [2];
```

After ensuring first array is smaller:

```js
nums1 = [2];
nums2 = [1, 3];
```

```text
m = 1
n = 2
```

### Iteration 1

```text
cut1 = 0
cut2 = 2

left1  = -∞
right1 = 2

left2  = 3
right2 = +∞
```

Invalid because:

```text
left2 > right1
```

Move right.

---

### Iteration 2

```text
cut1 = 1
cut2 = 1

left1 = 2
right1 = +∞

left2 = 1
right2 = 3
```

Valid partition:

```text
2 <= 3
1 <= +∞
```

Total length is odd:

```text
Median = max(2,1) = 2
```

---

# 6. Complexity Analysis

### Optimal Solution

```text
Time: O(log(min(m, n)))
Space: O(1)
```

This is why it is considered a Hard problem and is frequently asked in top product-company interviews.

---

# 7. Edge Cases

### One Array Empty

```js
nums1 = [];
nums2 = [1];
```

Median:

```js
1;
```

---

### Both Arrays Same Size

```js
[1, 2][(3, 4)];
```

Median:

```js
2.5;
```

---

### Different Sizes

```js
[1, 3, 8][(7, 9, 10, 11)];
```

Median:

```js
8;
```

---

### Duplicates

```js
[1, 2, 2][(2, 2, 3)];
```

Median:

```js
2;
```

---

# Interview Tips

- Start with the merge solution (`O(m+n)`) to show understanding.
- Then explain why the interviewer expects better than merging.
- Emphasize the partition condition:

```text
left1 <= right2
left2 <= right1
```

- Always perform binary search on the **smaller array**.
- Use `-Infinity` and `Infinity` to handle boundary partitions cleanly.

The binary-search partition approach with **O(log(min(m,n)))** time and **O(1)** space is the most interview-preferred solution.

## Question 3. Find K closest elements to a target

## Question 4. Allocate minimum number of pages (book allocation problem)

## Question 5. Painter's partition problem

## Question 6. Quickselect algorithm to find kth smallest/largest

## Question 7. Search in 2D matrix where each row and column is sorted

## Question 8. Count number of elements smaller than or equal to X in sorted matrix

## Question 9. Find peak element in 2D matrix

## Question 10. Maximum sum of i\*arr[i] after rotations

## Question 11. Longest subarray with sum divisible by K (negative numbers included)

## Question 12. Count pairs with given XOR

## Question 13. Subarray with sum closest to zero

## Question 14. Count number of subarrays with XOR = K

## Question 15. Check if array can be divided into pairs with sum divisible by K

## Question 16. Longest consecutive sequence

## Question 17. Group anagrams together

## Question 18. Find first non-repeating element in stream

## Question 19. Maximum frequency element in subarrays of size K

## Question 20. Count all subarrays with distinct elements
