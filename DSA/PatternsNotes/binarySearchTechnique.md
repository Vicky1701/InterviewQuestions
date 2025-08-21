# Binary Search Technique: Complete Guide & Notes

## What is Binary Search?
Binary Search is an efficient algorithm for finding an item from a sorted list of items. It repeatedly divides the search interval in half, discarding the half that cannot contain the target.

## When to Use Binary Search
- The input array/list is sorted
- You need to search for an element, range, or condition efficiently
- Problems asking for minimum/maximum value that satisfies a condition

## How to Recognize Binary Search Problems
- The problem involves a sorted array or monotonic function
- The problem asks for search, count, or position
- The problem can be reframed as finding a boundary (first/last occurrence)
- The problem involves optimization (min/max value)

## Common Binary Search Patterns
1. **Classic Search:** Find target in sorted array
2. **Lower/Upper Bound:** Find first/last occurrence of target
3. **Search by Condition:** Find min/max value that satisfies a condition
4. **Search in Rotated Array:** Array is rotated, but still sorted in parts

## Typical Algorithms
- Search in sorted array
- Find first/last position of element
- Find peak element
- Search in rotated sorted array
- Find square root, cube root
- Find minimum/maximum in range

## Steps to Apply Binary Search
1. **Initialize pointers:** left = 0, right = n-1
2. **Loop until left <= right:**
3. **Calculate mid:** mid = left + (right - left) // 2
4. **Check condition:** If arr[mid] == target, return mid
5. **Update pointers:** Move left/right based on comparison
6. **Handle edge cases:** Duplicates, out-of-bounds, empty array

## Example Template
```python
# Example: Search for target in sorted array
arr = [1, 2, 3, 4, 5, 6]
target = 4
left, right = 0, len(arr) - 1
while left <= right:
    mid = left + (right - left) // 2
    if arr[mid] == target:
        print(mid)
        break
    elif arr[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
```

## Tips for Problem Solving
- Always check if the array is sorted
- Be careful with integer overflow in mid calculation
- For range problems, use lower/upper bound logic
- For rotated arrays, check which half is sorted
- For optimization, binary search on answer space

## Practice Problems
- Search in Rotated Sorted Array (#33)
- Find Minimum in Rotated Sorted Array (#153)
- Sqrt(x) (#69)
- First Bad Version (#278)
- Find Peak Element (#162)
- Search Insert Position (#35)
- Find First and Last Position of Element in Sorted Array (#34)
- Median of Two Sorted Arrays (#4)
- Kth Smallest Element in a Sorted Matrix (#378)
- Find Smallest Letter Greater Than Target (#744)

---
**Visit this note before solving any Binary Search problem to refresh concepts, patterns, and strategies!**
