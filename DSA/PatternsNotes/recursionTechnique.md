# Recursion Technique: Complete Guide & Notes

## What is Recursion?
Recursion is a technique where a function calls itself to solve smaller instances of a problem. It is widely used for problems that can be broken down into similar subproblems.

## When to Use Recursion
- Problems with a natural divide-and-conquer structure
- Tree and graph traversals
- Backtracking problems (combinations, permutations)
- Problems where the solution depends on solutions to smaller subproblems

## How to Recognize Recursion Problems
- The problem can be defined in terms of smaller versions of itself
- The problem involves exploring all possibilities (combinations, paths)
- The problem asks for all solutions, not just one
- The problem involves hierarchical data (trees, graphs)

## Common Recursion Patterns
1. **Simple Recursion:** Directly calls itself with smaller input (e.g., factorial, Fibonacci)
2. **Backtracking:** Explores all possibilities, undoes choices (e.g., permutations, N-Queens)
3. **Divide and Conquer:** Splits problem, solves parts recursively (e.g., merge sort)

## Typical Algorithms
- Fibonacci sequence
- Factorial
- Permutations and combinations
- Subsets
- Tree traversals (preorder, inorder, postorder)
- Backtracking (N-Queens, Sudoku)
- Merge sort, quick sort

## Steps to Apply Recursion
1. **Define base case:** When should recursion stop?
2. **Define recursive case:** How does the problem reduce?
3. **Combine results:** How do you use results from subproblems?
4. **Avoid infinite recursion:** Ensure base case is always reached
5. **Optimize with memoization:** For overlapping subproblems

## Example Template
```python
# Example: Fibonacci sequence

def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)
```

## Tips for Problem Solving
- Always write the base case first
- Draw recursion trees for understanding
- Use memoization or DP for efficiency
- For backtracking, undo choices after recursive call
- Be careful with stack overflow for deep recursion

## Practice Problems
- Fibonacci Number (#509)
- Permutations (#46)
- Subsets (#78)
- Letter Combinations of a Phone Number (#17)
- Generate Parentheses (#22)
- Combination Sum (#39)
- Merge Two Sorted Lists (#21)
- Maximum Depth of Binary Tree (#104)
- N-Queens (#51)
- Sudoku Solver (#37)

---
**Visit this note before solving any Recursion problem to refresh concepts, patterns, and strategies!**
