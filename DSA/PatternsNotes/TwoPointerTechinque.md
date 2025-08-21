# Two Pointer Technique: Complete Guide & Notes

## What is the Two Pointer Technique?
The Two Pointer technique is a powerful approach for solving array and string problems efficiently. It involves using two indices (pointers) to traverse the data structure, often from opposite ends or moving at different speeds.

## When to Use Two Pointers
- Problems involving searching for pairs or triplets (e.g., sum, difference)
- Problems requiring partitioning or rearranging elements
- Problems where you need to compare elements from both ends
- Problems involving subarrays or substrings
- Linked list problems (finding cycles, merging, etc.)

## How to Recognize Two Pointer Problems
- The input is a sorted array or string
- You need to find pairs/triplets with a given property (sum, difference, etc.)
- You need to move or partition elements (e.g., move zeroes, sort colors)
- You need to check for palindromes or reverse elements
- You need to merge or compare two lists/arrays

## Common Two Pointer Patterns
1. **Opposite Ends:** Start pointers at both ends and move towards each other (e.g., pair sum, palindrome check)
2. **Same Direction:** Both pointers start at the beginning, one moves faster/slower (e.g., remove duplicates, sliding window)
3. **Multiple Arrays:** Use pointers to traverse two arrays/lists simultaneously (e.g., merge sorted arrays)

## Typical Algorithms
- Find pair with given sum in sorted array
- Remove duplicates from sorted array
- Move zeroes to end
- Reverse a string or array
- Merge two sorted arrays
- Partition array (Dutch National Flag problem)
- Detect cycle in linked list (Floyd’s Tortoise and Hare)
- Find intersection of two arrays/lists

## Steps to Apply Two Pointer Technique
1. **Initialize pointers:** Usually at start/end or both at start
2. **Define movement:** Decide how and when to move each pointer
3. **Check condition:** What property are you looking for? (sum, match, etc.)
4. **Update pointers:** Move left/right, increment/decrement as needed
5. **Edge cases:** Handle empty arrays, duplicates, out-of-bounds
## Example Template
```java
// Example: Find if a pair with sum exists in sorted array
int[] arr = {1, 2, 3, 4, 6};
int target = 6;
int left = 0, right = arr.length - 1;
while (left < right) {
    int currSum = arr[left] + arr[right];
    if (currSum == target) {
        System.out.println(left + " " + right);
        break;
    } else if (currSum < target) {
        left++;
    } else {
        right--;
    }
}
```

## Tips for Problem Solving
- Always check if the array/string is sorted (if not, sort if allowed)
- Think about what each pointer represents (start, end, window, etc.)
- Visualize pointer movement on paper for tricky cases
- Use while loops for flexible pointer movement
- For linked lists, use fast/slow pointers for cycle detection

## Practice Problems
- Two Sum II - Input array is sorted (#167)
- Remove Duplicates from Sorted Array (#26)
- Move Zeroes (#283)
- 3Sum (#15)
- Container With Most Water (#11)
- Valid Palindrome (#125)
- Reverse String (#344)
- Merge Sorted Array (#88)
- Linked List Cycle (#141)
- Intersection of Two Arrays II (#350)

---
**Visit this note before solving any Two Pointer problem to refresh concepts, patterns, and strategies!**
