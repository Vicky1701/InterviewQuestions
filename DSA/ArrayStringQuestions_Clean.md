# Array/String Manipulation - LeetCode Interview Questions

## 🎯 **Essential Interview Questions (23 Problems)**

### **Easy Level (8 problems) - Days 1-4**

1. **Two Sum** - LC #1 ⭐⭐⭐⭐⭐
   - **Pattern**: Hash Map lookup
   - **Time**: O(n), Space: O(n)

2. **Best Time to Buy and Sell Stock** - LC #121 ⭐⭐⭐⭐⭐
   - **Pattern**: Single pass, track minimum
   - **Time**: O(n), Space: O(1)

3. **Contains Duplicate** - LC #217 ⭐⭐⭐⭐
   - **Pattern**: Hash Set for duplicates
   - **Time**: O(n), Space: O(n)

4. **Maximum Subarray** - LC #53 ⭐⭐⭐⭐⭐
   - **Pattern**: Kadane's Algorithm
   - **Time**: O(n), Space: O(1)

5. **Plus One** - LC #66 ⭐⭐⭐
   - **Pattern**: Array manipulation, carry handling
   - **Time**: O(n), Space: O(1)

6. **Move Zeroes** - LC #283 ⭐⭐⭐⭐
   - **Pattern**: Two pointers
   - **Time**: O(n), Space: O(1)

7. **Valid Anagram** - LC #242 ⭐⭐⭐⭐
   - **Pattern**: Character frequency counting
   - **Time**: O(n), Space: O(1)

8. **Missing Number** - LC #268 ⭐⭐⭐
   - **Pattern**: Mathematical sum or XOR
   - **Time**: O(n), Space: O(1)

### **Medium Level (12 problems) - Days 5-10**

9. **Product of Array Except Self** - LC #238 ⭐⭐⭐⭐⭐
   - **Pattern**: Prefix/Suffix products
   - **Time**: O(n), Space: O(1)

10. **3Sum** - LC #15 ⭐⭐⭐⭐⭐
    - **Pattern**: Two pointers after sorting
    - **Time**: O(n²), Space: O(1)

11. **Group Anagrams** - LC #49 ⭐⭐⭐⭐⭐
    - **Pattern**: Hash map with sorted string keys
    - **Time**: O(n*k*log(k)), Space: O(n*k)

12. **Longest Substring Without Repeating Characters** - LC #3 ⭐⭐⭐⭐⭐
    - **Pattern**: Sliding window
    - **Time**: O(n), Space: O(min(m,n))

13. **Container With Most Water** - LC #11 ⭐⭐⭐⭐⭐
    - **Pattern**: Two pointers
    - **Time**: O(n), Space: O(1)

14. **Rotate Array** - LC #189 ⭐⭐⭐⭐
    - **Pattern**: Array reversal technique
    - **Time**: O(n), Space: O(1)

15. **Merge Intervals** - LC #56 ⭐⭐⭐⭐⭐
    - **Pattern**: Sorting + merging
    - **Time**: O(n*log(n)), Space: O(1)

16. **Insert Interval** - LC #57 ⭐⭐⭐⭐
    - **Pattern**: Interval manipulation
    - **Time**: O(n), Space: O(1)

17. **Spiral Matrix** - LC #54 ⭐⭐⭐⭐
    - **Pattern**: Matrix traversal with boundaries
    - **Time**: O(m*n), Space: O(1)

18. **Set Matrix Zeroes** - LC #73 ⭐⭐⭐⭐
    - **Pattern**: In-place marking
    - **Time**: O(m*n), Space: O(1)

19. **Subarray Sum Equals K** - LC #560 ⭐⭐⭐⭐⭐
    - **Pattern**: Prefix sum + hash map
    - **Time**: O(n), Space: O(n)

20. **Find All Anagrams in a String** - LC #438 ⭐⭐⭐⭐
    - **Pattern**: Sliding window + frequency map
    - **Time**: O(n), Space: O(1)

### **Hard Level (3 problems) - Days 11-13**

21. **Trapping Rain Water** - LC #42 ⭐⭐⭐⭐⭐
    - **Pattern**: Two pointers or monotonic stack
    - **Time**: O(n), Space: O(1)

22. **Minimum Window Substring** - LC #76 ⭐⭐⭐⭐⭐
    - **Pattern**: Sliding window with hash map
    - **Time**: O(n), Space: O(k)

23. **Median of Two Sorted Arrays** - LC #4 ⭐⭐⭐⭐
    - **Pattern**: Binary search on smaller array
    - **Time**: O(log(min(m,n))), Space: O(1)

## 📅 **13-Day Study Schedule**

### Week 1: Foundation Building

- **Day 1**: Two Sum, Contains Duplicate
- **Day 2**: Best Time to Buy/Sell Stock, Maximum Subarray
- **Day 3**: Plus One, Move Zeroes
- **Day 4**: Valid Anagram, Missing Number

### Week 2: Core Patterns

- **Day 5**: Product of Array Except Self, 3Sum
- **Day 6**: Group Anagrams, Longest Substring Without Repeating
- **Day 7**: Container With Most Water, Rotate Array
- **Day 8**: Merge Intervals, Insert Interval
- **Day 9**: Spiral Matrix, Set Matrix Zeroes

### Week 2: Advanced & Review

- **Day 10**: Subarray Sum Equals K, Find All Anagrams
- **Day 11**: Trapping Rain Water
- **Day 12**: Minimum Window Substring
- **Day 13**: Median of Two Sorted Arrays + Review weak areas

## 🎯 **Problem-Solving Templates**

### **Two Pointers Template**
```
left = 0, right = n-1
while left < right:
    # Process current pair
    if condition_met:
        # Record result
        left++, right--
    elif need_larger_sum:
        left++
    else:
        right--
```

### **Sliding Window Template**
```
left = 0
for right in range(n):
    # Expand window by including arr[right]
    while window_condition_violated:
        # Shrink window from left
        left++
    # Process current window [left, right]
```

### **Frequency Map Template**
```
freq = {}
for char in string:
    freq[char] = freq.get(char, 0) + 1
# Use frequency map for comparisons/operations
```

## ✅ **Progress Tracker**

- [ ] Day 1: Two Sum, Contains Duplicate
- [ ] Day 2: Best Time to Buy/Sell Stock, Maximum Subarray  
- [ ] Day 3: Plus One, Move Zeroes
- [ ] Day 4: Valid Anagram, Missing Number
- [ ] Day 5: Product of Array Except Self, 3Sum
- [ ] Day 6: Group Anagrams, Longest Substring Without Repeating
- [ ] Day 7: Container With Most Water, Rotate Array
- [ ] Day 8: Merge Intervals, Insert Interval
- [ ] Day 9: Spiral Matrix, Set Matrix Zeroes
- [ ] Day 10: Subarray Sum Equals K, Find All Anagrams
- [ ] Day 11: Trapping Rain Water
- [ ] Day 12: Minimum Window Substring
- [ ] Day 13: Median of Two Sorted Arrays + Review

**Start Date**: ___________  
**Target Completion**: ___________

## 🔥 **Pro Tips for Success**

1. **Master the fundamentals first** - Two Sum, Maximum Subarray are building blocks
2. **Practice drawing arrays** - Visual representation helps with complex problems
3. **Learn multiple approaches** - Brute force → Optimized → Space optimized
4. **Focus on edge cases** - Empty arrays, single elements, all same elements
5. **Time yourself strictly** - Build interview pressure tolerance

**Good luck! Start with Day 1 tomorrow! 🚀**
