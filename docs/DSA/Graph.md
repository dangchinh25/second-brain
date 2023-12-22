## References
- https://leetcode.com/discuss/general-discussion/655708/graph-for-beginners-problems-pattern-sample-solutions/
- https://leetcode.com/discuss/study-guide/1326900/graph-algorithms-problems-to-practice
- https://leetcode.com/tag/graph/discuss/2732038/Graph-study-guide
- https://leetcode.com/tag/graph/discuss/2750020/Summary-of-Graph-DFS-problems
- https://leetcode.com/tag/graph/discuss/1122034/Important-Graph-Algorithm-Notes-for-Interview
- https://leetcode.com/tag/graph/discuss/2360573/Become-Master-In-Graph

## DFS
```python
def dfs_iterative(adj_list, start, end):
  stack = list()
  visited = set()
  stack.append(start)
  visited.add(start)
  
  while (len(stack) > 0):
      cur_node = stack.pop()
      for adj_node in adj_list[cur_node]:
          if adj_node == end:
              return True
          if adj_node in visited:
              continue
          stack.append(adj_node)
          visited.add(adj_node)
  return False
```

```python
def dfs_recursion(adj_list, start, end, visited):
  if start in visited:
      return False
  if start == end:
      return True
  visited.add(start)
  for adj_node in adj_list[start]:
      if dfs(adj_list, adj_node, end, visited):
          return True
  return False
```

- Basic implementation of DFS with stack, only work with adjacency list
    - NOTES: If we find the chance to use DFS but the original data structure is not adjacency list, we can iterate over it and construct our own adjacency list to use this DFS implementation (**Important partern**)
    - Another pattern is **counting**, here we **may need to traverse through the graph to check all possible path** and stuff like that ⇒ Initialize an array to track all paths outside the dfs helper, and pass in as the parameter for every call and update the array in each **DFS call**
        - This also applies to problems where there maybe a cycle inside the graph, we can initialize an array(usually use **set** rather than array as it has O(1) lookup and also preserve unique characteristic) **visited** to keep track of visited node, and pass in as parameter for the dfs helper and update the array with new node with each call
        - Another key point is that we can setup for the dfs helper to return something to signal the change in the count, for example, the dfs will traverse all the node and return true, which means that the count can be increase