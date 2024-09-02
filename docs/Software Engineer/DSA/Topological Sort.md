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

```
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