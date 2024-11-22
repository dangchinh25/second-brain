- Tree problems usually just spin around DFS or BFS and utilizes some specific tree properties so it's important to be able to implement these 2 algo while blindfolded

- **DFS**
	- The idea is usually to use recursion and recursively traverse the left and right subtree while performing some operation
	- Sometimes it's helpful to use another params in the recursive function to keep track of some data (eg: path sum, current node path, etc)
```python
# https://leetcode.com/problems/path-sum/
def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        def getSum(node, curSum):
            if not node:
                return False
            if not node.left and not node.right and curSum + node.val == targetSum:
                return True

            left = getSum(node.left, curSum + node.val)
            right = getSum(node.right, curSum + node.val)

            return left or right
        
        return getSum(root, 0)
```
- **BFS**
	- The idea is to use queue to keep track of what to traverse next (we can use queue/stack for DFS too but recursion is usually cleaner)
```python
# https://leetcode.com/problems/binary-tree-level-order-traversal/
 def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        res = []
        if not root:
            return res
        queue = [root]

        while queue:
            cur_level = []
            cur_len = len(queue)
            for i in range(cur_len):
                node = queue.pop(0)
                cur_level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            res.append(cur_level)

        return res
```


**NOTES**
- An important property when dealing with *Full Binary Tree* is that it only has 2 childrens, from this we can calculate that:
> **Binary Tree in Array Representation**
	If a binary tree is represented as an array:
	1. **Index 0** represents the root node.
	2. For a node at index i:
	• **Left Child**: The left child is located at index 2i + 1.
	• **Right Child**: The right child is located at index 2i + 2.