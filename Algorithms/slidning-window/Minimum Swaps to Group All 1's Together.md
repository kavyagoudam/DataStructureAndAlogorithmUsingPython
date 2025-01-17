A swap is defined as taking two distinct positions in an array and swapping the values in them.

A circular array is defined as an array where we consider the first element and the last element to be adjacent.

Given a binary circular array nums, return the minimum number of swaps required to group all 1's present in the array together at any location.

 

Example 1:

Input: nums = [0,1,0,1,1,0,0]
Output: 1
Explanation: Here are a few of the ways to group all the 1's together:
[0,0,1,1,1,0,0] using 1 swap.
[0,1,1,1,0,0,0] using 1 swap.
[1,1,0,0,0,0,1] using 2 swaps (using the circular property of the array).
There is no way to group all 1's together with 0 swaps.
Thus, the minimum number of swaps required is 1.
Example 2:

Input: nums = [0,1,1,1,0,0,1,1,0]
Output: 2
Explanation: Here are a few of the ways to group all the 1's together:
[1,1,1,0,0,0,0,1,1] using 2 swaps (using the circular property of the array).
[1,1,1,1,1,0,0,0,0] using 2 swaps.
There is no way to group all 1's together with 0 or 1 swaps.
Thus, the minimum number of swaps required is 2.
Example 3:

Input: nums = [1,1,0,0,1]
Output: 0
Explanation: All the 1's are already grouped together due to the circular property of the array.
Thus, the minimum number of swaps required is 0.

```python
class Solution:
    def minSwaps(self, nums: List[int]) -> int:
        # Calculate the minimum swaps needed to group all 1s or all 0s together
        op1 = self.min_swaps_helper(nums, 0)  # Grouping all 0s together
        op2 = self.min_swaps_helper(nums, 1)  # Grouping all 1s together
        return min(op1, op2)

    def min_swaps_helper(self, data: List[int], val: int) -> int:
        length = len(data)
        total_val_count = 0

        # Count the total number of `val` in the array
        for i in range(length - 1, -1, -1):
            if data[i] == val:
                total_val_count += 1

        # If there is no `val` or the array is full of `val`, no swaps are needed
        if total_val_count == 0 or total_val_count == length:
            return 0

        start = 0
        end = 0
        max_val_in_window = 0
        current_val_in_window = 0

        # Initial window setup: count the number of `val` in the first window of size `total_val_count`
        while end < total_val_count:
            if data[end] == val:
                current_val_in_window += 1
            end += 1
        max_val_in_window = max(max_val_in_window, current_val_in_window)

        # Slide the window across the array to find the maximum number of `val` in any window
        while end < length:
            if data[start] == val:
                current_val_in_window -= 1
            start += 1
            if data[end] == val:
                current_val_in_window += 1
            end += 1
            max_val_in_window = max(max_val_in_window, current_val_in_window)

        # Minimum swaps are the total `val` minus the maximum found in any window
        return total_val_count - max_val_in_window
```


