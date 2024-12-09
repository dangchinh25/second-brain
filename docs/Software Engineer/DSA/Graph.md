## References
- Comprehensive(must read):
	- https://leetcode.com/tag/graph/discuss/2360573/Become-Master-In-Graph
- Practices list:
	- https://leetcode.com/discuss/general-discussion/655708/graph-for-beginners-problems-pattern-sample-solutions/
	- https://leetcode.com/discuss/study-guide/1326900/graph-algorithms-problems-to-practice
- https://leetcode.com/tag/graph/discuss/2732038/Graph-study-guide
- https://leetcode.com/tag/graph/discuss/2750020/Summary-of-Graph-DFS-problems
- https://leetcode.com/tag/graph/discuss/1122034/Important-Graph-Algorithm-Notes-for-Interview
- https://leetcode.com/discuss/general-discussion/971272/Python-Graph-Algorithms-One-Place-for-quick-revision
- https://leetcode.com/discuss/interview-question/4283222/Graph-(Beginners-to-Advanced)-All-Algorithms-Python

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

- Basic implementation of DFS with stack, *only work with adjacency list*
    - **NOTES**: If we find the chance to use DFS but the original data structure is not adjacency list, we can iterate over it and construct our own adjacency list to use this DFS implementation (**Important pattern**)
    - Another pattern is **counting**, here we **may need to traverse through the graph to check all possible path** and stuff like that ⇒ Initialize an array to track all paths outside the DFS helper, and pass in as the parameter for every call and update the array in each **DFS call**
        - This also applies to problems where there maybe a cycle inside the graph, we can initialize an array(usually use **set** rather than array as it has O(1) lookup and also preserve unique characteristic) **visited** to keep track of visited node, and pass in as parameter for the dfs helper and update the array with new node with each call
        - Another key point is that we can setup for the dfs helper to return something to signal the change in the count, for example, the dfs will traverse all the node and return true, which means that the count can be increase
### Helpful Pattern
- Traverse all root to leaves path
- Sometimes we have to traverse all possible path and keep record of those paths
https://leetcode.com/problems/binary-tree-paths/
- We can extend this for more complex requirement
	- E.g: since we keep track of all the path, we can easily calculate path sum or node depths, etc
```python
def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
	res = []
	
	def dfs(node, cur_path):
		nonlocal res
		if not node:
			return
		cur_path.append(node.val)
		if not node.left and not node.right:
			res.append('->'.join([str(i) for i in cur_path]))
			return
		if node.left:
			dfs(node.left, cur_path.copy())
		if node.right:
			dfs(node.right, cur_path.copy())
	
	dfs(root, [])
	return res
```

- Another helpful thinking pattern is **reverse**, sometime it's easier to solve the problem like `find path to target` by traversing from target to every other nodes rather than traversing from every individual node to target 
	- [[Blind 75-150#^9216d6 | Rotting oranges]]
https://leetcode.com/problems/walls-and-gates/description/
- Here we traverse from the gate and update all the adjacent empty room instead of traversing from empty room and stop at gate
- Similar problem: https://leetcode.com/problems/as-far-from-land-as-possible
```python
def wallsAndGates(self, rooms: List[List[int]]) -> None:
        rows, cols = len(rooms), len(rooms[0])
        queue = []
        visit = set()
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        for row in range(rows):
            for col in range(cols):
                if rooms[row][col] == 0:
                    visit.add((row, col))
                    queue.append((row, col))

        dist = 0
        while queue:
            for i in range(len(queue)):
                cur_row, cur_col = queue.pop(0)
                rooms[cur_row][cur_col] = dist

                for row_dir, col_dir in directions:
                    next_row, next_col = cur_row + row_dir, cur_col + col_dir
                    if (
                        next_row not in range(rows) or
                        next_col not in range(cols) or
                        rooms[next_row][next_col] == -1 or
                        (next_row, next_col) in visit
                    ): 
                        continue
                    visit.add((next_row, next_col))
                    queue.append((next_row, next_col))
            dist += 1
```

## Topological Sort
 -   Another pattern in Graph section is **Topological Sort**
    -   **Problems that can be modelled as a graph with directed edges where some events must occur before others**
        -   School class prerequisites
        -   Program dependencies
        -   Event scheduling
        -   Etc
-   **Topological Sort** algorithm can find a **topological ordering** in **O(V+E)**
    -   **Topological ordering** is an ordering of the nodes in a directed graph where for each directed edge from node A to node B, node A appears before node B in the ordering
    -   Not every graph can have a **topological ordering** ⇒ Graph that contains a cycle
    -   The only type of graph that can contain **topological ordering** is **Directed Acyclic Graphs (DAG)** (Tree is also a DAG by definition)

```python
graph = {
    0: [1, 3],
    1: [2],
    2: [3,4],
    3: [4],
    4: []
}

def explore(graph: dict, node: int, visited: list, result: list):
    visited[node] = True
    for neighbor in graph[node]:
        if visited[neighbor] is False:
            explore(graph, neighbor, visited, result)
    result.insert(0, node)

def topological_sort(graph: dict):
    visited = [False]*len(graph)
    result = list()
    
    for node in graph:
        if visited[node] is False:
            explore(graph, node, visited, result)
    
    print(result)
    
topological_sort(graph)
```


**NOTES:** There will be problem that we have to check for a cycle inside a graph, we can utilize the **visited** array ⇒ Rather than just storing True/False value (to signal if we have visited the node), we can store numerical value to indicate different things. For example, we can store 0 as not visited, 1 as currently visiting and traversing its neighbor, and 2 as done visiting the current node and all its neighbor. In this way we could see that if the state of the neighbor we are trying to visit is 1, that means that the graph contains a cycle.