
# Overview
- Also known as **Disjoin Set Union** is a data structure used to keep track of a set of elements partitioned into non-overlapping subsets
	- Each subset is represented by *a representative*, and *a representative can also belongs to another subset with another representative* => Creates a *tree-like structure*
- Provides *two main operations*
	- **Find**: Determine which subset a particular element is in. This can be used to check if two elements are in the same subset.
		- This always returns *the top-most representative* of each set
	- **Union**: Merge two subsets into a single subset
- This post done a good job visualizing it: https://leetcode.com/discuss/general-discussion/1072418/Disjoint-Set-Union-(DSU)Union-Find-A-Complete-Guide
- **Commonly used for**:
	- Merging connected components (dynamically)
	- Detect cycle in the graph (*If two node are already in one components and there is another edge to connect those nodes, there is a cycle*) [[Union Find#^bc18de | Graph Valid Tree]]

- Basic python implementation => **Should memorize**
```python
class UnionFind:
    def __init__(self):
        self.size = collections.defaultdict(lambda: 1)
        self.parents = {}

    def find(self, node: int) -> int:
        if node not in self.parents:
            self.parents[node] = node
            return node

        if self.parents[node] == node:
            return node

        # Path compression
        self.parents[node] = self.find(self.parents[node])

        return self.parents[node]

    def union(self, node1: int, node2: int) -> None:
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            # Union by rank/size
            if self.size[root1] > self.size[root2]:
                self.parents[root2] = root1
                self.size[root1] += self.size[root2]
            else:
                self.parents[root1] = root2
                self.size[root2] += self.size[root1]
```

- By definition, the actual implementation would be much simpler where the **find** operations only recursively find the top-most representative and **union** operations just merge node1 and node2 together
	- Doing this may cause inefficiency where each subset *tree-like structure* will have a big *height* => *find* will take *O(n)* every time
		- By doing **path compression**, we point every *element* to the top-most representative (during the search) so that later operation will only take *O(1)*
		- Similar idea to **union by rank/size** where we want to minimize the *height* as much as possible
			- *By rank*: rank refers to the depth of the tree in each group, which means we should only increase the rank if we are merging equals group
			- *By size*: size refers to the actual number of element in each group, which is the approach we are taking here

# Kruskal's Algorithm for MST
This example finds the MST of a graph using Kruskal’s algorithm.
• **Problem Explanation**:
	The MST of a weighted graph is a subgraph that connects all nodes with the smallest total edge weight and without cycles. Kruskal’s algorithm sorts edges by weight and uses Union-Find to ensure no cycles are added while building the MST.
```python
def kruskal(n, edges):
    edges.sort(key=lambda x: x[2])  # Sort edges by weight
    uf = UnionFind(n)  # Initialize Union-Find for n nodes
    mst_weight = 0

    for u, v, weight in edges:
        if not uf.connected(u, v):  # If u and v are not connected
            uf.union(u, v)  # Union the nodes
            mst_weight += weight  # Add the edge weight to the MST
    return mst_weight
```

# Example

- Most obvious example: https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/
	- Only merge when it is time to merge
```python
class Solution:
    def earliestAcq(self, logs: List[List[int]], n: int) -> int:
        logs.sort()

        uf = UnionFind()
        groupCount = n

        for timestamp, x, y  in logs:
            if uf.connected(x, y):
                continue
            uf.union(x, y)
            groupCount -= 1
            if groupCount == 1:
                return timestamp
        
        return -1

class UnionFind:
    def __init__(self):
        self.parents = {}
        self.size = collections.defaultdict(lambda: 1)
    
    def find(self, node):
        if node not in self.parents:
            self.parents[node] = node
            return node
        
        if self.parents[node] == node:
            return node

        self.parents[node] = self.find(self.parents[node])

        return self.parents[node]
    
    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            if self.size[root1] > self.size[root2]:
                self.parents[root2] = root1
                self.size[root1] += self.size[root2]
            else:
                self.parents[root1] = root2
                self.size[root2] += self.size[root1]

    def connected(self, node1, node2):
        return self.find(node1) == self.find(node2)
```

https://leetcode.com/problems/accounts-merge/description/
- A really good and thorough example of using UnionFind data structure
	- Only merges when needed
	- Only store the relationship of representatives in the data structure instead of storing all the emails 
```python
class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        uf = UnionFind()
        n = len(accounts)

        email_group = {}
        for i in range(n):
            for j in range(1, len(accounts[i])):
                email = accounts[i][j]

				# If this is the first time seeing this email then
                # assign component group as the account index
                if email not in email_group:
                    email_group[email] = i
                else:
	                # If we have seen this email before then union this
                    # group with the previous group of the email
                    uf.union(i, email_group[email])

		# Store emails corresponding to the component's representative
        components = collections.defaultdict(list)
        for email, group in email_group.items():
            root_group = uf.find(group)
            components[root_group].append(email)
        
        return [[accounts[group][0]] + sorted(emails) for group, emails in components.items()]

class UnionFind:
    def __init__(self):
        self.parents = {}
        self.size = collections.defaultdict(lambda: 1)
    
    def find(self, node):
        if node not in self.parents:
            self.parents[node] = node
            return node
        if self.parents[node] == node:
            return node
        
        self.parents[node] = self.find(self.parents[node])
        return self.parents[node]
    
    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            if self.size[root1] > self.size[root2]:
                self.parents[root2] = root1
                self.size[root1] += self.size[root2]
            else:
                self.parents[root1] = root2
                self.size[root2] += self.size[root1]
```

https://leetcode.com/problems/number-of-provinces/description/
```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        uf = UnionFind()
        n = len(isConnected)

        # init all nodes
        # can also refactor the class to take into account this
        for node in range(n):
            uf.find(node)

        for cur_node in range(n):
            for nb in range(cur_node+1, n):
                if isConnected[cur_node][nb] == 1 and not uf.connected(cur_node, nb):
                    uf.union(cur_node, nb)

        return uf.counts

class UnionFind:
    def __init__(self):
        self.parents = {}
        self.size = collections.defaultdict(lambda: 1)
        self.counts = 0
    
    def find(self, node):
        if node not in self.parents:
            self.parents[node] = node
            self.counts += 1
            return node
        if self.parents[node] == node:
            return node

        self.parents[node] = self.find(self.parents[node])
        return self.parents[node]
    
    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            if self.size[root1] > self.size[root2]:
                self.parents[root2] = root1
                self.size[root1] += self.size[root2]
            else:
                self.parents[root1] = root2
                self.size[root2] += self.size[root1]
            self.counts -= 1
    
    def connected(self, node1, node2):
        return self.find(node1) == self.find(node2)
```

https://leetcode.com/problems/graph-valid-tree ^bc18de
```python
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        if not edges and n <= 1:
            return True
            
        uf = UnionFind()

        for node1, node2 in edges:
            if uf.find(node1) == uf.find(node2):
                return False
            uf.union(node1, node2)
        
        return uf.counts == 1 and len(uf.parents) == n

class UnionFind:
    def __init__(self):
        self.parents = {}
        self.size = collections.defaultdict(lambda: 1)
        self.counts = 0
    
    def find(self, node):
        if node not in self.parents:
            self.parents[node] = node
            self.counts += 1
            return node
        
        if self.parents[node] == node:
            return node
        
        self.parents[node] = self.find(self.parents[node])
        return self.parents[node]
    
    def union(self, node1, node2):
        root1 = self.find(node1)
        root2 = self.find(node2)

        if root1 != root2:
            if self.size[root1] > self.size[root2]:
                self.parents[root2] = root1
                self.size[root1] += self.size[root2]
            else:
                self.parents[root1] = root2
                self.size[root2] += self.size[root1]
            self.counts -=1 
```