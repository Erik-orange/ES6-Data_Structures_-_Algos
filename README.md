## Array Patterns for Solving Interview Questions

### Two-Pointers Pattern

Usually, we use one pointer to navigate each element in an array.
However, there are times when having two pointers (left **/** right, low **/** high) comes in handy.


#### Example
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

### Sliding Window Pattern

The sliding window pattern is similar to the two pointers pattern.
The difference is that the distance between the `left` and `right` pointer is always the same.
Also, the numbers don’t need to be `sorted`.

#### EXAMPLE

Find the max `sum` of an array of integers, only taking `k` items from the `right` and `left` side sequentially.

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

Since the `sum` always has `k` elements, we can compute the cumulative `sum` for the `k` first elements from the `left`.
Then, we slide the "window" to the `right` and remove one from the `left` until we cover all the `right` items.
In the end, we would have all the possible combinations without duplicated work.


The difference between the two pointers pattern and the sliding windows, it’s that we move both pointers at the _same time_ to
keep the _length of the window the same_.


#### Practice Problems

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
Given a text, return the most common words in descending order. You should sanitize the input by removing punctuation `! ? ' , ; .` and converting all letters to lowercase. Return the most common words in descending order.

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

#### Practice Problems

1. Fit two movies in a flight

You are working in an entertainment recommendation system for an airline.
Given a flight duration (target) and an array of movies length, you need to recommend two movies that fit exactly the length of the flight. Return an array with the indices of the two numbers that add up to the target. No duplicates are allowed.
If it’s not possible to return empty [].

```jsx
function twoSum(nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];

    if (map.has(nums[i])) return [map.get(nums[i]), i];
    map.set(complement, i);
  }


  return [];
}
```

2. Subarray Sum that Equals K

Given an array of integers, find all the possible subarrays to add up to k. Return the count.

```jsx
function subarraySum(nums, k) {
  let ans = 0;
  let sum = 0;
  const map = new Map([[0, 1]]);

  for (let i = 0; i < nums.length; i++) {
    sum += nums[i];

    if (map.has(sum - k)) ans += map.get(sum - k);

    map.set(sum, 1 + (map.get(sum) || 0));
  }

  return ans;
}
```

You might wonder, what the map is initialized with `[0, 1]`.

Consider this test case:
```jsx
subarraySum([1], 1);	 // k = 1
```

The `sum` is `1`, however `sum - k` is `0`.

If it doesn’t exist on the map, we will get the wrong answer since that number adds up to `k`.

We need to add an initial case on the map: `map.set(0, 1)`.

If `nums[i] - k = 0`, then that means that `nums[i] = k` and should be part of the solution.

___

# Set Patterns for Solving Interview Questions

JavaScript has a built-in Hash Set, so that' the one we are going to focus on.


One typical case for a Set is to eliminate duplicates from an array.

const arr = [1, 2, 2, 1, 3, 2];
console.log([...new Set(arr)]); // [ 1, 2, 3 ]


#### Practice Problems

1. Most Common Word

Given a text and a list of banned words. Find the most common word that is not on the banned list.
You might need to sanitize the text and strip out punctuation ‘?!,’.`

```jsx
function mostCommonWord(paragraph, banned) {
  const words = paragraph.toLowerCase().replace(/\W+/g, ' ').split(/\s+/);
  const b = new Set(banned);

  const map = words.reduce((m, w) => (b.has(w) ? m : m.set(w, 1 + (m.get(w) || 0))), new Map());
  const max = Math.max(...map.values());
  
  for (const [w, c] of map.entries()) if (c === max) return w;

  return '';
}
```

2. Longest Substring Without Repeating

Find the `length` of the longest substring without repeating characters.

```jsx
function lenLongestSubstring(s) {
  let max = 0;
  const set = new Set();

  for (let i = 0, j = 0; j < s.length; j++) {
    while (set.has(s[j])) set.delete(s[i++]);
    set.add(s[j]);
    max = Math.max(max, set.size);
  }

  return max;
}
```

___

# Linked List Patterns for Interview Questions

Most linked list problems are solved using 1 to 3 pointers. Sometimes we move them in tandem or individually.

Examples of problems that can be solved using multiple pointers:
 • Detecting if the linked list is circular (has a loop).
 • Finding the middle node of a linked list in 1-pass without any auxiliary data structure.
 • Reversing the linked list in 1-pass without any auxiliary data structure. e.g. `1->2->3` to `3->2- >1`.

# Fast/Slow Pointers

One standard algorithm to detect loops in a linked list is fast/slow runner pointers (a.k.a The Tortoise and the Hare OR Floyd’s Algorithm).

The slow pointer moves one node per iteration, while the fast pointer moves two nodes every time.

```jsx
let fast = head,
let slow = head;
while (fast && fast.next) {
  slow = slow.next;         // slow moves 1 by 1.
  fast = fast.next.next;    // slow moves 2 by 2. 
}
```

If you don’t have a loop, then fast and slow will never meet.

However, by the time the fast pointer reaches the end, the slow pointer would be precisely in the middle!

This technique is useful for getting the middle element of a singly list in one pass without using any auxiliary data structure (like array or map).

# EXAMPLE

Find out if a linked list has a cycle and, if so, return the intersection node (where the cycle begins).

```jsx
function findCycleStartBrute(head) { 
  const visited = new Set();
  let curr = head;
  
  while (curr) {
    if (visited.has(curr)) return curr;
    visited.add(curr);
    curr = curr.next;
  }

  return null;
}
```
////////////////////////////////////////////////////////////
```jsx
function findCycleStart(head) {
  let slow = head;
  let fast = head;

  while (fast && fast.next) {
    slow = slow.next; 		  // slow moves 1 by 1
    fast = fast.next.next; 	  // slow moves 2 by 2

    if (fast === slow) { 	  // detects loop!
      slow = head;		  // reset pointer to begining
      while (slow !== fast) { 	  // find intersection
        slow = slow.next;
        fast = fast.next; 	  // move both pointers one by one this time
      }

    return slow; 		  // return where the loop starts }
  }

  return null; 			  // not found
}
```

# Multiple Pointers

# EXAMPLE

Determine if a singly linked list is a palindrome.
A palindrome is a sequence that reads the same backward as forward.

```jsx
function isPalindrome(head) {
  let slow = head;
  let fast = head;

  while (fast) {      // use slow/fast pointers to find the middle
    slow = slow.next;
    fast = fast.next && fast.next.next;
  }

  const reverseList = (node) => {   // use 3 pointers to reverse a linked list
    let prev = null;
    let curr = node;

    while (curr) {
      const { next } = curr;     // same as: "const next = curr.next;" curr.next = prev;
      prev = curr;
      curr = next;
    }

    return prev;
  };

  const reversed = reverseList(slow);    // head of the reversed half

  for (let i = reversed, j = head;  i;  i = i.next, j = j.next) {
    if (i .value !== j.value) return false;
  }

  return true;
}
```

#### Practice Problems

1. Merge Linked Lists into One

Merge two sorted lists into one (and keep them sorted)

```jsx
function mergeTwoLists(l1, l2) { 
  const sentinel = new ListNode();
  let p0 = sentinel;
  let p1 = l1;
  let p2 = l2;

  while (p1 || p2) {
    if (!p1 || (p2 && p1.value > p2.value)) {
      p0.next = p2;
      p2 = p2.next;
    }
    else {
      p0.next = p1;
      p1 = p1.next;
    }
    p0 = p0.next;
  }

  return sentinel.next;
}
```


2. Check if two strings lists are the same

Given two linked lists with strings, check if the data is equivalent.

```jsx
function hasSameData(l1, l2) {
  let p1 = l1;
  let p2 = l2;
  let i1 = -1;
  let i2 = -1;

  const findNextPointerIndex = (p, i) => {
    let node = p;
    let index = i;

    while (node && index >= node.value.length) {
      node = node.next;
      index = 0;
    }

    return [node, index];
  };

  while (p1 && p2) {
    [p1, i1] = findNextPointerIndex(p1, i1 + 1);
    [p2, i2] = findNextPointerIndex(p2, i2 + 1);
    
    if ((p1 && p2 && p1.value[i1] !== p2.value[i2]) || ((!p1 || !p2) && p1 !== p2)) return false;
  }

  return true;
}
```

___

# Stack Patterns for Interview Questions

Use a Stack when:
• You need to access your data as last-in, first-out (LIFO). 
• You need to implement a Depth-First Search


#### Practice Problems

1. Validate Parentheses

Given a string with three types of brackets: `()`, `{}`, and `[]`. Validate they are correctly closed and opened.

```jsx
function isParenthesesValid(string) {
  const map = new Map([['(', ')'], ['{', '}'], ['[', ']']]);
  const stack = [];

  for (const c of string) {
    if (map.has(c)) stack.push(map.get(c));
    else if (c !== stack.pop()) return false;
  }

  return stack.length === 0;
}
```

2. Daily Temperatures

Given an array of integers from 30 to 100 (daily temperatures), return another array that, for each day in the input,
tells you how many days you would have to wait until warmer weather. If no warmer climate is possible, then return 0 for that element.

```jsx
function dailyTemperatures(t) {
  const last = (arr) => arr[arr.length - 1];
  const stack = [];
  const ans = [];

  for (let i = t.length - 1; i >= 0; i--) {
    while (stack.length && t[i] >= t[last(stack)]) stack.pop();

    ans[i] = stack.length ? last(stack) - i : 0;
    stack.push(i);
  }

  return ans;
}
```
___

Queue Patterns for Interview Questions

Use a Queue when:
• You need to access your data on a first-come, first-served basis (FIFO). 
• You need to implement a Breadth-First Search


#### Practice Problems

1. Recent Counter
Design a class that counts the most recent requests within a time window.

2. Design Snake Game
Design the move function for the snake game. The move function returns an integer representing the current score.
If the snake goes out of the given height and width or hit itself, the game is over and return -1.
