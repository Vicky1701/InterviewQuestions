# BFS / DFS Technique: Complete Guide & Notes

## What are BFS and DFS?
Breadth-First Search (BFS) and Depth-First Search (DFS) are fundamental graph and tree traversal algorithms. BFS explores neighbors level by level, while DFS explores as deep as possible before backtracking.

## When to Use BFS / DFS
- Problems involving graphs or trees (traversal, connectivity, paths)
- Finding shortest path (BFS)
- Exploring all possible paths or components (DFS)
- Problems involving cycles, connected components, or reachability

## How to Recognize BFS / DFS Problems
- The problem involves traversing or searching a graph/tree
- You need to visit all nodes, find paths, or check connectivity
- The problem asks for shortest/longest path, number of islands, or components

## Common BFS / DFS Patterns
1. **Level Order Traversal (BFS):** Visit nodes level by level
2. **Recursive DFS:** Explore as deep as possible using recursion
3. **Iterative DFS:** Use a stack to simulate recursion
4. **Multi-source BFS:** Start BFS from multiple nodes
5. **Cycle Detection:** Use DFS/BFS to detect cycles in graphs

## Typical Algorithms
- Number of Islands
- Binary Tree Level Order Traversal
- Flood Fill
- Shortest Path in Binary Matrix
- Clone Graph
- Course Schedule
- Detect Cycle in Graph
- Connected Components

## Steps to Apply BFS / DFS
1. **Choose traversal type:** BFS (queue) or DFS (stack/recursion)
2. **Initialize data structures:** Queue for BFS, stack/recursion for DFS
3. **Visit nodes:** Mark visited, process neighbors/children
4. **Track state:** Level, parent, path, etc. as needed
5. **Handle edge cases:** Disconnected graphs, cycles, revisiting nodes

## Example Template (BFS)
```python
# Example: BFS for shortest path in grid
from collections import deque
queue = deque([(start_x, start_y)])
visited = set([(start_x, start_y)])
while queue:
    x, y = queue.popleft()
    for nx, ny in neighbors(x, y):
        if (nx, ny) not in visited:
            visited.add((nx, ny))
            queue.append((nx, ny))
```

## Example Template (DFS)
```python
# Example: DFS for connected components
visited = set()
def dfs(node):
    if node in visited:
        return
    visited.add(node)
    for neighbor in graph[node]:
        dfs(neighbor)
```

## Tips for Problem Solving
- For shortest path, use BFS
- For exploring all paths/components, use DFS
- Mark nodes as visited to avoid cycles
- For trees, DFS is often recursive; for graphs, handle cycles carefully
- For multi-source problems, initialize BFS from all sources

## Practice Problems
- Number of Islands (#200)
- Binary Tree Level Order Traversal (#102)
- Flood Fill (#733)
- Clone Graph (#133)
- Course Schedule (#207)
- Detect Cycle in Graph (custom)
- Shortest Path in Binary Matrix (#1091)
- Rotting Oranges (#994)
- Pacific Atlantic Water Flow (#417)
- Word Ladder (#127)

---
**Visit this note before solving any BFS/DFS problem to refresh concepts, patterns, and strategies!**
