# Sliding Window Technique: Complete Guide & Notes

## What is the Sliding Window Technique?
The Sliding Window technique is a method for solving problems involving subarrays or substrings in arrays/strings. It uses a window (range) that slides over the data structure to maintain a subset of elements and efficiently compute results.

## When to Use Sliding Window
- Problems involving subarrays/substrings with a given property (sum, length, distinct elements)
- Problems asking for maximum/minimum/average in a range
- Problems requiring counting or finding patterns in contiguous elements

## How to Recognize Sliding Window Problems
- The problem asks for a result in a contiguous subarray or substring
- You need to find the longest/shortest/maximum/minimum subarray/substring
- You need to count or sum elements in a moving range
- Constraints allow O(N) or O(N log N) solutions

## Common Sliding Window Patterns
1. **Fixed Size Window:** Window size is constant (e.g., max sum of subarray of size k)
2. **Variable Size Window:** Window size changes based on conditions (e.g., longest substring without repeating characters)
3. **Two Pointers Window:** Left and right pointers define the window boundaries

## Typical Algorithms
- Maximum sum subarray of size k
- Longest substring without repeating characters
- Minimum window substring
- Subarray product less than k
- Find all anagrams in a string
- Count number of subarrays with a given sum
- Maximum number of vowels in a substring of given length

## Steps to Apply Sliding Window Technique
1. **Initialize pointers:** Usually left and right at start
2. **Expand window:** Move right pointer to include new elements
3. **Shrink window:** Move left pointer to exclude elements when condition is violated
4. **Update result:** Track max/min/count as needed
5. **Edge cases:** Handle empty arrays, window size larger than array, etc.

## Example Template
```python
# Example: Maximum sum subarray of size k
arr = [2, 1, 5, 1, 3, 2]
k = 3
max_sum = 0
window_sum = 0
for i in range(len(arr)):
    window_sum += arr[i]
    if i >= k - 1:
        max_sum = max(max_sum, window_sum)
        window_sum -= arr[i - (k - 1)]
print(max_sum)
```

## Tips for Problem Solving
- Use hash maps or sets for variable window problems (e.g., longest substring without repeats)
- Always update the result before shrinking the window
- Visualize the window movement for tricky cases
- For counting problems, maintain frequency maps
- For minimum/maximum problems, update result at each valid window

## Practice Problems
- Maximum Sum Subarray of Size K (#643)
- Longest Substring Without Repeating Characters (#3)
- Minimum Size Subarray Sum (#209)
- Permutation in String (#567)
- Longest Repeating Character Replacement (#424)
- Subarray Product Less Than K (#713)
- Find All Anagrams in a String (#438)
- Sliding Window Maximum (#239)
- Longest Substring with At Most K Distinct Characters (#340)
- Minimum Window Substring (#76)

---
**Visit this note before solving any Sliding Window problem to refresh concepts, patterns, and strategies!**
