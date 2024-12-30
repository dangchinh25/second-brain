- Tree problems usually just spin around DFS or BFS and utilizes some specific tree properties so it's important to be able to implement these 2 algo while blindfolded

- **DFS**
	- The idea is usually to use recursion and recursively traverse the left and right subtree while performing some operation
	- Sometimes it's helpful to use another params in the recursive function to keep track of some data (eg: path sum, current node path, etc)

- **Preorder Traversal**
```python
def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
	if not root:
		return []

	left = self.preorderTraversal(root.left)
	right = self.preorderTraversal(root.right)

	return [root.val] + left + right
```
- **Inorder Traversal
```python
def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
	if not root:
		return []

	left = self.inorderTraversal(root.left)
	right = self.inorderTraversal(root.right)

	return left + [root.val]+ right
```
- **Postorder Traversal
```python
def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
	if not root:
		return []

	left = self.postorderTraversal(root.left)
	right = self.postorderTraversal(root.right)

	return left + right + [root.val]
```


- Example:
https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/
```python
def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None

        root = TreeNode(preorder[0])
        # We can use rootIndex of inorder because
        # preorder: root + left + right
        # inorder: left + root + right
        # So we can use the rootIndex to split the preorder and inoder to 2 subarrays
        rootIndex = inorder.index(root.val)
        root.left = self.buildTree(preorder[1:rootIndex+1], inorder[:rootIndex+1])
        root.right = self.buildTree(preorder[rootIndex+1:], inorder[rootIndex+1:])

        return root
```

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

- Sometimes it is helpful to think of a Tree as a [[Graph]], and we can create an adjacency list from a Tree and then traverse it like a Graph or apply Graph algorithm.