# Greedy Technique: Complete Guide & Notes

## What is Greedy?
Greedy is an algorithmic paradigm that builds up a solution piece by piece, always choosing the next piece that offers the most immediate benefit. It is used for optimization problems where local choices lead to a global optimum.

## When to Use Greedy
- Problems asking for minimum/maximum, shortest/longest, largest/smallest
- Problems where making the locally optimal choice leads to a global solution
- Problems involving intervals, scheduling, or resource allocation

## How to Recognize Greedy Problems
- The problem can be solved by making a series of choices
- The problem involves sorting or prioritizing elements
- The problem asks for optimal solution (min/max, fewest/most)

## Common Greedy Patterns
1. **Interval Scheduling:** Select non-overlapping intervals
2. **Resource Allocation:** Assign resources to maximize/minimize outcome
3. **Coin Change:** Minimize number of coins/steps
4. **Jump Game:** Reach the end with minimum jumps

## Typical Algorithms
- Activity Selection
- Jump Game
- Gas Station
- Assign Cookies
- Minimum Number of Arrows to Burst Balloons
- Non-overlapping Intervals
- Partition Labels
- Task Scheduler

## Steps to Apply Greedy
1. **Sort or prioritize elements:** Based on problem requirements
2. **Iterate and make choices:** Always pick the best available option
3. **Update solution:** Track progress, resources, or intervals
4. **Check for feasibility:** Ensure choices are valid
5. **Edge cases:** Handle empty input, ties, or impossible cases

## Example Template
```python
# Example: Activity Selection (interval scheduling)
intervals = [[1,3],[2,4],[3,5]]
intervals.sort(key=lambda x: x[1])
count, end = 0, float('-inf')
for start, finish in intervals:
    if start >= end:
        count += 1
        end = finish
print(count)
```

## Tips for Problem Solving
- Always prove greedy choice is optimal (counterexamples help)
- Sort input if needed for intervals/resources
- For coin change, use largest denomination first
- For scheduling, track end times and update as needed
- For jump/reach problems, track farthest reach

## Practice Problems
- Jump Game (#55)
- Gas Station (#134)
- Assign Cookies (#455)
- Candy (#135)
- Minimum Number of Arrows to Burst Balloons (#452)
- Non-overlapping Intervals (#435)
- Activity Selection (custom)
- Partition Labels (#763)
- Task Scheduler (#621)
- Lemonade Change (#860)

---
**Visit this note before solving any Greedy problem to refresh concepts, patterns, and strategies!**
