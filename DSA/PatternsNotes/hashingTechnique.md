# Hashing Technique: Complete Guide & Notes

## What is Hashing?
Hashing is a technique for mapping data to a fixed-size value (hash code) for fast lookup, insertion, and deletion. It is widely used in hash tables, sets, and maps for O(1) average time complexity operations.

## When to Use Hashing
- Problems involving fast lookup, frequency counting, or duplicate detection
- Problems requiring grouping or categorization
- Problems involving set operations (union, intersection, difference)

## How to Recognize Hashing Problems
- The problem asks for frequency, uniqueness, or grouping
- You need to check for existence or count elements quickly
- The problem involves mapping or categorizing data

## Common Hashing Patterns
1. **Frequency Map:** Count occurrences of elements
2. **Hash Set:** Track unique elements or detect duplicates
3. **Hash Map for Grouping:** Group elements by a property (e.g., anagrams)
4. **Custom Hashing:** Map complex data to hashable keys

## Typical Algorithms
- Two Sum
- Group Anagrams
- First Unique Character in a String
- Intersection of Two Arrays
- Find Duplicates
- Valid Anagram
- Top K Frequent Elements
- Subarray Sum Equals K

## Steps to Apply Hashing
1. **Choose hash structure:** Hash map, hash set, etc.
2. **Insert/update elements:** Track frequency, existence, or grouping
3. **Query hash structure:** Check for existence, count, or retrieve groups
4. **Handle collisions:** For custom hashing, ensure unique keys
5. **Edge cases:** Empty input, large data, hash collisions

## Example Template
```python
# Example: Two Sum using hash map
nums = [2, 7, 11, 15]
target = 9
lookup = {}
for i, num in enumerate(nums):
    if target - num in lookup:
        print(lookup[target - num], i)
        break
    lookup[num] = i
```

## Tips for Problem Solving
- Use hash maps for frequency/counting problems
- Use hash sets for uniqueness/duplicate detection
- For grouping, use hash map with lists as values
- Be careful with hash collisions for custom keys
- For large data, consider space complexity

## Practice Problems
- Two Sum (#1)
- Group Anagrams (#49)
- First Unique Character in a String (#387)
- Intersection of Two Arrays (#349)
- Happy Number (#202)
- Isomorphic Strings (#205)
- Contains Duplicate (#217)
- Valid Anagram (#242)
- Top K Frequent Elements (#347)
- Subarray Sum Equals K (#560)

---
**Visit this note before solving any Hashing problem to refresh concepts, patterns, and strategies!**
