# 🧠 Day 1: Two Sum Problem

## **Problem Details**
- **LeetCode #1**: Two Sum
- **Difficulty**: Easy
- **Pattern**: HashMap for O(1) lookup
- **Category**: Arrays & Hash Tables

---

## 📋 **Problem Statement**

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

**Example 1:**
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**
```
Input: nums = [3,3], target = 6
Output: [0,1]
```

---

## 🎯 **Learning Objectives**

### **What to Learn Today**:
- ✅ Understand the brute force O(n²) approach
- ✅ Learn how HashMap reduces time complexity to O(n)
- ✅ Grasp the concept of "complement" in pair problems
- ✅ Master the trade-off between time and space complexity

### **Key Concepts**:
1. **HashMap for constant lookup time**
2. **Complement calculation**: `target - current_element`
3. **Index tracking** while iterating
4. **Early return** when solution found

---

## 💡 **Solution Approaches**

### **Approach 1: Brute Force (Not Recommended)**
```java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[]{};
}
```
- **Time Complexity**: O(n²)
- **Space Complexity**: O(1)
- **Why not optimal**: Nested loops are inefficient

### **Approach 2: HashMap (Optimal)**
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        
        map.put(nums[i], i);
    }
    
    return new int[]{}; // No solution found
}
```
- **Time Complexity**: O(n)
- **Space Complexity**: O(n)
- **Why optimal**: Single pass with constant lookup time

---

## 🧪 **Test Cases to Try**

### **Basic Tests**:
1. `nums = [2,7,11,15], target = 9` → Expected: `[0,1]`
2. `nums = [3,2,4], target = 6` → Expected: `[1,2]`
3. `nums = [3,3], target = 6` → Expected: `[0,1]`

### **Edge Cases**:
1. `nums = [1,2], target = 3` → Expected: `[0,1]`
2. `nums = [5,5,5,5], target = 10` → Expected: `[0,1]`
3. `nums = [-1,-2,-3,-4,-5], target = -8` → Expected: `[2,4]`

---

## 🎯 **Step-by-Step Solution Process**

### **Step 1: Understand the Problem (5 minutes)**
- Read the problem statement carefully
- Identify what we need to return (indices, not values)
- Note constraints (exactly one solution exists)

### **Step 2: Think of Approach (5 minutes)**
- Consider brute force: check every pair
- Realize HashMap can help with faster lookup
- Think about what to store in HashMap (value → index)

### **Step 3: Code the Solution (15 minutes)**
- Start with HashMap approach
- Iterate through array once
- For each element, calculate its complement
- Check if complement exists in HashMap
- If yes, return indices; if no, add current element to HashMap

### **Step 4: Test and Debug (5 minutes)**
- Run through provided examples
- Check edge cases
- Verify the solution handles duplicates correctly

---

## 🔑 **Key Insights**

### **The "Aha!" Moment**:
Instead of checking if `nums[i] + nums[j] == target`, we can:
1. Calculate `complement = target - nums[i]`
2. Check if `complement` exists in our HashMap
3. This reduces nested loops to single loop!

### **Why HashMap Works**:
- **O(1) average lookup time** vs O(n) for array search
- **Store as we go**: no need to pre-populate
- **Index preservation**: HashMap stores value → index mapping

### **Pattern Recognition**:
This is a **"Two Sum"** pattern that appears in many variations:
- Two Sum II (sorted array)
- Three Sum
- Four Sum
- Two Sum in BST

---

## 📝 **Your Solution**

```java
// Write your solution here after understanding the pattern
public int[] twoSum(int[] nums, int target) {
    // Your implementation
    
}
```

### **Your Notes**:
- **Time taken**: _____ minutes
- **Attempts needed**: _____
- **Key insight learned**: ________________
- **Mistakes made**: ________________
- **What to remember**: ________________

---

## 🔗 **Connection to Java 8**

### **Modern Java Approach** (for learning, not interview):
```java
public int[] twoSumJava8(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    
    return IntStream.range(0, nums.length)
        .filter(i -> {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return true;
            }
            map.put(nums[i], i);
            return false;
        })
        .mapToObj(i -> new int[]{map.get(target - nums[i]), i})
        .findFirst()
        .orElse(new int[]{});
}
```

**Note**: While this shows Java 8 features, the traditional approach is clearer for interviews.

---

## 🎉 **Completion Checklist**

- [ ] ✅ Understood the problem completely
- [ ] 🧠 Grasped the HashMap optimization concept
- [ ] 💻 Coded the solution successfully
- [ ] 🧪 Tested with all provided examples
- [ ] 📝 Documented key insights
- [ ] ⏰ Completed within 30 minutes
- [ ] 🔗 Connected to broader "Two Sum" pattern

---

**🎯 Congratulations on solving your first DSA problem! 🎯**

**Key Takeaway**: HashMap transforms O(n²) nested loops into O(n) single loop!

**Next**: Valid Parentheses - Stack pattern for matching pairs
