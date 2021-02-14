#Array Patterns for Solving Interview Questions

## Two-Pointers Pattern

Usually, we use one pointer to navigate each element in an array.
However, there are times when having two pointers (left **/** right, low **/** high) comes in handy.


### Example
Given a `sorted` array of integers, find two numbers that add up to a target and return their values.

We can use two pointers: one pointer starting from the left side and the other from the right side.

This technique only works for `sorted` arrays. If the array was not sorted, you would have to sort it first or choose another approach.

```jsx
function twoSum(arr, target) {
  let left = 0;
  right = arr.length -1;

  while (left < right) {
    const sum = arr[left] + arr[right];

    if (sum === target) return [arr[left], arr[right]];
    else if (sum > target) right--;
    else left++;
  }

  return [];
}

```

## Sliding Window Pattern

The sliding window pattern is similar to the two pointers pattern.
The difference is that the distance between the left and right pointer is always the same.
Also, the numbers don’t need to be `sorted`.

### EXAMPLE

Find the max sum of an array of integers, only taking `k` items from the right and left side sequentially.

Constraints: `k` won’t exceed the number of elements in the array: `1 <= k <= n`.

```jsx
function maxSum(arr, k) {
  let left = k - 1;
  let right = arr.length -1;
  let sum = 0;
  for (let i = 0; i < k; i++) sum += arr[i];

  let max = sum;
  for (let i = 0; i < k; i++) {
    sum += arr[right--] - arr[left--]; max = Math.max(max, sum);
  }

  return max;
};
```

Since the sum always has k elements, we can compute the cumulative sum for the k first elements from the left.
Then, we slide the "window" to the right and remove one from the left until we cover all the right items.
In the end, we would have all the possible combinations without duplicated work.


The difference between the two pointers pattern and the sliding windows, it’s that we move both pointers at the same time to
keep the length of the window the same.


### PRACTICE QUESTIONS

1. Max Subarray

Given an array of integers, find the maximum sum of consecutive elements (subarray).

```jsx
function maxSubArray(a) { 
  let max = -Infinity;
  let local = 0;

  a.forEach((n) => {
    local = Math.max(n, local + n);
    max = Math.max(max, local);
  });

  return max;
}
```

2. Best Time to Buy and Sell a Stock

You have an array of integers. Each value represents the closing value of the stock on that day.
You have only one chance to buy and then sell. What’s the maximum profit you can obtain? (Note: you have to buy first and then sell)

```jsx
function maxProfit(prices) {
  let max = 0;
  let local = Infinity;

  for (let i = 0; i < prices.length; i++) {
    local = Math.min(local, prices[i]);
    max = Math.max(max, prices[i] - local);
  }

  return max;
}
```

___

# HashMap Patterns for Solving Interview Questions

## Trading Speed for Space (Using HashMap to Count)

### EXAMPLE
Given a text, return the most common words in descending order. You should sanitize the input by removing punctuation !?',;. and converting all letters to lowercase. Return the most common words in descending order.

```jsx
function mostCommonWords(text, n = 1) {
  const words = text.toLowerCase().split(/\W+/);
  const map = words.reduce((m, w) => m.set(w, 1 + (m.get(w) || 0)), new Map());

  return Array.from(map.entries()).sort((a, b) => b[1] - a[1]).slice(0, n).map((w) => w[0]);
}
```

## Sliding Window Pattern

The idea is very similar, we still use the two pointers, and the solution is the "window" between the pointers.
We can increase or decrease the window as long as it keeps the constraints of the problem.

### EXAMPLE

Return the `length` of the longest substring without repeating characters.

```jsx
function longestSubstring(s) {
  const map = new Map();
  let max = 0;

  for (let hi = 0, lo = 0; hi < s.length; hi++) {
    if (map.has(s[hi])) lo = Math.max(lo, map.get(s[hi]) + 1);
    map.set(s[hi], hi);
    max = Math.max(max, hi - lo + 1);
  }

  return max;
};
```
