# Arrays - Complete Guide

## Table of Contents
1. [Overview](#overview)
2. [Key Concepts](#key-concepts)
3. [Time & Space Complexity](#time--space-complexity)
4. [Problems & Solutions](#problems--solutions)

---

## Overview

**Arrays** are one of the most fundamental data structures. They store elements in contiguous memory locations, allowing O(1) access by index. In Python, arrays are called **lists**.

### Why Arrays Matter?
- Foundation for many algorithms
- 40% of DSA interview focuses on arrays
- Most problems can be solved with optimization tricks

---

## Key Concepts

### 1. Array Basics
- **Indexing**: Access element by position `arr[i]`
- **Slicing**: Extract subarray `arr[start:end]`
- **Iteration**: Loop through elements
- **In-place modification**: Change without extra space

### 2. Techniques to Master

#### Two Pointer Technique
- Use two pointers at different speeds/positions
- Reduces time complexity
- Example: Remove duplicates, Reverse array

```python
# Example: Reverse array in-place
def reverse_array(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1
    return arr

# Output: [1, 2, 3, 4, 5] -> [5, 4, 3, 2, 1]
```

#### Sliding Window
- Maintain a window of fixed/variable size
- Move window and track some state
- Example: Longest substring, Max sum subarray

#### Prefix Sum
- Precompute cumulative sums
- Useful for range sum queries
- Time: O(n) preprocessing, O(1) per query

```python
# Prefix Sum Example
def build_prefix_sum(arr):
    n = len(arr)
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + arr[i]
    return prefix

# Range sum from index i to j: prefix[j+1] - prefix[i]
arr = [1, 2, 3, 4, 5]
prefix = build_prefix_sum(arr)
print(prefix)  # [0, 1, 3, 6, 10, 15]
# Sum from index 1 to 3: prefix[4] - prefix[1] = 10 - 1 = 9
```

---

## Time & Space Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Access by index | O(1) | - |
| Search (unsorted) | O(n) | - |
| Insert | O(n) | - |
| Delete | O(n) | - |
| Append | O(1) amortized | - |

---

## Problems & Solutions

### Problem 1: Two Sum ⭐⭐⭐

**Description**: Given an array of integers and a target, find two numbers that add up to target.

**Approach**:
- Brute force: Check all pairs O(n²)
- Optimal: Use hashmap for O(1) lookup → O(n) total

**Code**:
```python
def two_sum(nums, target):
    """
    Find two numbers that sum to target.
    
    Args:
        nums: List of integers
        target: Target sum
    
    Returns:
        List with indices of two numbers, or empty list if not found
    """
    num_map = {}  # value -> index
    
    for i, num in enumerate(nums):
        complement = target - num
        
        if complement in num_map:
            return [num_map[complement], i]
        
        num_map[num] = i
    
    return []

# Test Cases
print(two_sum([2, 7, 11, 15], 9))      # Output: [0, 1]
print(two_sum([3, 2, 4], 6))           # Output: [1, 2]
print(two_sum([3, 3], 6))              # Output: [0, 1]
print(two_sum([1, 2, 3], 10))          # Output: []
```

**Time Complexity**: O(n)  
**Space Complexity**: O(n) for hashmap  
**Why it works**: Dictionary allows O(1) lookup of complement

---

### Problem 2: Maximum Subarray (Kadane's Algorithm) ⭐⭐⭐

**Description**: Find the contiguous subarray with the largest sum.

**Approach**:
- Keep track of max sum ending at current position
- Keep track of global maximum
- At each position, decide: extend existing subarray or start new

**Code**:
```python
def max_subarray(nums):
    """
    Find maximum sum of contiguous subarray using Kadane's Algorithm.
    
    Args:
        nums: List of integers
    
    Returns:
        Maximum sum found
    """
    if not nums:
        return 0
    
    max_current = max_global = nums[0]
    
    for i in range(1, len(nums)):
        # Either extend subarray or start new
        max_current = max(nums[i], max_current + nums[i])
        # Update global maximum
        max_global = max(max_global, max_current)
    
    return max_global

# Version with indices
def max_subarray_with_indices(nums):
    """Returns max sum and the subarray itself"""
    if not nums:
        return 0, []
    
    max_current = max_global = nums[0]
    start = end = temp_start = 0
    
    for i in range(1, len(nums)):
        if nums[i] > max_current + nums[i]:
            max_current = nums[i]
            temp_start = i
        else:
            max_current = max_current + nums[i]
        
        if max_current > max_global:
            max_global = max_current
            start = temp_start
            end = i
    
    return max_global, nums[start:end+1]

# Test Cases
print(max_subarray([-2, 1, -3, 4, -1, 2, 1, -5, 4]))        # Output: 6
print(max_subarray([5, 4, -1, 7, 8]))                        # Output: 23
print(max_subarray([-1]))                                     # Output: -1
print(max_subarray_with_indices([-2, 1, -3, 4, -1, 2, 1, -5, 4]))  # Output: (6, [4, -1, 2, 1])
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)  
**Why it works**: At each step, we have optimal decision for that position

---

### Problem 3: Move Zeroes ⭐⭐

**Description**: Move all zeroes to the end while maintaining relative order of non-zero elements. Do it in-place with O(1) extra space.

**Approach**:
- Two pointers: one for position to place non-zero, one for iteration
- When non-zero found, swap and move both pointers

**Code**:
```python
def moveZeroes(nums):
    """
    Move all zeroes to end in-place.
    
    Args:
        nums: List of integers (modified in-place)
    """
    # Position where next non-zero should go
    write_pos = 0
    
    # Find first zero
    for i in range(len(nums)):
        if nums[i] != 0:
            # Swap
            nums[write_pos], nums[i] = nums[i], nums[write_pos]
            write_pos += 1

# Test Cases
test1 = [0, 1, 0, 3, 12]
moveZeroes(test1)
print(test1)  # Output: [1, 3, 12, 0, 0]

test2 = [0]
moveZeroes(test2)
print(test2)  # Output: [0]

test3 = [1, 2, 3]
moveZeroes(test3)
print(test3)  # Output: [1, 2, 3]

test4 = [0, 0, 1]
moveZeroes(test4)
print(test4)  # Output: [1, 0, 0]
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)  
**Why it works**: Two-pointer in-place modification without extra array

---

### Problem 4: Remove Duplicates from Sorted Array ⭐⭐

**Description**: Remove duplicates from sorted array in-place. Return count of unique elements.

**Approach**:
- Two pointers: slow (unique elements), fast (iteration)
- When different element found, place at slow pointer

**Code**:
```python
def removeDuplicates(nums):
    """
    Remove duplicates from sorted array in-place.
    
    Args:
        nums: Sorted list of integers (modified in-place)
    
    Returns:
        Count of unique elements
    """
    if not nums:
        return 0
    
    unique_pos = 0
    
    for i in range(1, len(nums)):
        if nums[i] != nums[unique_pos]:
            unique_pos += 1
            nums[unique_pos] = nums[i]
    
    return unique_pos + 1

# Test Cases
test1 = [1, 1, 2]
count1 = removeDuplicates(test1)
print(f"Count: {count1}, Array: {test1[:count1]}")  # Output: Count: 2, Array: [1, 2]

test2 = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]
count2 = removeDuplicates(test2)
print(f"Count: {count2}, Array: {test2[:count2]}")  # Output: Count: 5, Array: [0, 1, 2, 3, 4]

test3 = [1]
count3 = removeDuplicates(test3)
print(f"Count: {count3}, Array: {test3[:count3]}")  # Output: Count: 1, Array: [1]
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)  
**Why it works**: Sorted array allows easy detection of duplicates

---

### Problem 5: Best Time to Buy and Sell Stock ⭐⭐

**Description**: Find maximum profit from buying and selling stock once. Can only hold one share at a time.

**Approach**:
- Track minimum price seen so far
- At each position, calculate profit if sold today
- Track maximum profit

**Code**:
```python
def maxProfit(prices):
    """
    Find maximum profit from buying and selling stock once.
    
    Args:
        prices: List of integers representing daily prices
    
    Returns:
        Maximum profit possible
    """
    if not prices or len(prices) < 2:
        return 0
    
    min_price = prices[0]
    max_profit = 0
    
    for price in prices[1:]:
        # Profit if we sell at current price
        potential_profit = price - min_price
        max_profit = max(max_profit, potential_profit)
        # Update minimum price
        min_price = min(min_price, price)
    
    return max_profit

# Test Cases
print(maxProfit([7, 1, 5, 3, 6, 4]))      # Output: 5 (buy at 1, sell at 6)
print(maxProfit([7, 6, 4, 3, 1]))         # Output: 0 (no profit possible)
print(maxProfit([2, 4, 1, 7, 5, 11]))     # Output: 10 (buy at 1, sell at 11)
print(maxProfit([3, 3]))                   # Output: 0
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)  
**Why it works**: Single pass tracking minimum and maximum profit

---

### Problem 6: Majority Element ⭐⭐

**Description**: Find element appearing more than n/2 times. Guaranteed that majority element exists.

**Approach 1 - Boyer-Moore Voting Algorithm**:
- Maintain candidate and count
- If count is 0, update candidate
- Increment/decrement count based on match

**Code**:
```python
def majorityElement(nums):
    """
    Find majority element (appears > n/2 times) using Boyer-Moore.
    
    Args:
        nums: List of integers
    
    Returns:
        The majority element
    """
    candidate = None
    count = 0
    
    # Find candidate
    for num in nums:
        if count == 0:
            candidate = num
        count += 1 if num == candidate else -1
    
    # Verify candidate (optional but good practice)
    return candidate

# Test Cases
print(majorityElement([3, 2, 3]))                          # Output: 3
print(majorityElement([2, 2, 1, 1, 1, 2, 2]))             # Output: 2
print(majorityElement([1]))                                # Output: 1
print(majorityElement([2, 2, 2, 2, 1, 1, 1]))             # Output: 2
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)  
**Why it works**: Pairing elements - if majority > n/2, it survives cancellation

---

### Problem 7: Rotate Array ⭐⭐

**Description**: Rotate array to the right by k steps in-place.

**Approach - Three Reversals**:
1. Reverse entire array
2. Reverse first k elements
3. Reverse remaining elements

**Code**:
```python
def rotate(nums, k):
    """
    Rotate array right by k steps in-place.
    
    Args:
        nums: List of integers (modified in-place)
        k: Number of steps to rotate
    """
    def reverse(start, end):
        while start < end:
            nums[start], nums[end] = nums[end], nums[start]
            start += 1
            end -= 1
    
    k = k % len(nums)  # Handle k > len(nums)
    
    reverse(0, len(nums) - 1)         # Reverse entire
    reverse(0, k - 1)                 # Reverse first k
    reverse(k, len(nums) - 1)         # Reverse rest

# Test Cases
test1 = [1, 2, 3, 4, 5, 6, 7]
rotate(test1, 3)
print(test1)  # Output: [5, 6, 7, 1, 2, 3, 4]

test2 = [1, 2]
rotate(test2, 1)
print(test2)  # Output: [2, 1]

test3 = [1, 2, 3, 4, 5]
rotate(test3, 2)
print(test3)  # Output: [4, 5, 1, 2, 3]

test4 = [1]
rotate(test4, 0)
print(test4)  # Output: [1]
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)  
**Why it works**: Three reversals achieve rotation without extra space

---

## Summary Table

| Problem | Difficulty | Technique | Time | Space |
|---------|-----------|-----------|------|-------|
| Two Sum | ⭐⭐⭐ | Hashmap | O(n) | O(n) |
| Max Subarray | ⭐⭐⭐ | Kadane's | O(n) | O(1) |
| Move Zeroes | ⭐⭐ | Two Pointer | O(n) | O(1) |
| Remove Duplicates | ⭐⭐ | Two Pointer | O(n) | O(1) |
| Stock Buy/Sell | ⭐⭐ | Greedy | O(n) | O(1) |
| Majority Element | ⭐⭐ | Voting | O(n) | O(1) |
| Rotate Array | ⭐⭐ | Reversal | O(n) | O(1) |

---

## Interview Tips

1. **Always ask about constraints** - Can array be modified? What about space?
2. **Explain before coding** - Talk through your approach first
3. **Handle edge cases** - Empty array, single element, duplicates
4. **Optimize step by step** - Start with brute force, then optimize
5. **Practice in-place modifications** - Very common in interviews

---

**Next**: Move to [Strings](../02-Strings/README.md)
