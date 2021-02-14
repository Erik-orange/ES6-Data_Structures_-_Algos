#Array Patterns for Solving Interview Questions

___
## Two-Pointers Pattern

Usually, we use one pointer to navigate each element in an array.
However, there are times when having two pointers (left **/** right, low **/** high) comes in handy.


### Example
Given a `sorted` array of integers, find two numbers that add up to a target and return their values.

We can use two pointers: one pointer starting from the left side and the other from the right side.

This technique only works for 'sorted' arrays. If the array was not sorted, you would have to sort it first or choose another approach.

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
