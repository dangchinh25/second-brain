- https://leetcode.com/tag/greedy/discuss/1476329/Useful-posts-from-LeetCode-discussions-for-personal-use-and-concept-revision
## 1. Sliding Window
- Used to perform a required operation on a specific window size of a given array or linked list, such as finding the longest subarray containing all 1s
- In some cases, the window size remains constant and in other cases the size grows or shrinks
![](https://i.imgur.com/muSOF3n.png)
- Some ways to identify that the given problem might require a sliding window
	- The problem input is a linear data structures such as linked list, array, or string
	- You're asked to find the longest/shortest substring, subarray or a desired value
- Common problems you use the sliding window pattern with
	- Maximum sum subarray of size K
	- Longest substring with K distinct characters
	- String anagrams
- [[Sliding Window]]

## 2. Two pointers or Iterators
[[Two Pointer]]
- Two Pointers is a pattern where two pointers iterate through the data structure in tandem until one or both of the pointers hit a certain condition
- Two Pointers is often useful when searching pairs in a sorted array or linked list
- Two Pointers are needed because with just pointer, you would have to continually loop back through the array to find the answer, this back and forth with a single iterator is inefficient for time and space complexity
![](https://i.imgur.com/PL3Xe8g.png)
- Some ways to identify that the given problem might require a two pointer
	- It will feature problems where you deal with sorted array (or Linked List) and need to find a set of elements that fufill certain conditions
	- The set of elements in the array is a pair, triple, or subarray
- Common problems you use the sliding two pointers with
	- Squaring a sorted array
	- Triples that sum to zero
	- Comparing strings that contains backspaces

## 3. Fast and Slow pointers
- Also known as the **Hare & Tortoise algorithm**, is a pointer algorithm that uses two pointers which move through the array (or sequence/linked list) at different speeds. *This approach is quite useful when dealing with cyclic linked or arrays*
- By moving at different speeds, the algorithm proves that the two pointers are bound to meet. The fast pointer should catch the slow pointer once both the pointers are in a cyclic loop.
-  Some ways to identify that the given problem might require a Fast and Slow pattern
	- The problem will deal with a loop in a linked list or array
	- When you need to know the position of a certain element or the overall length of the linked list
- When should I use it over the [[14 Patterns DSA#Two pointers or Iterators | Two Pointer method]]:
	- There are some cases where you shouldn't use the Two Pointer approach such as in a singly linked list where you can't move in a backwards direction. An example of when to use the Fast and Slow pattern is when you're trying to determine if a linked list is a palindrome
- Common problems you use the fast and slow pointers pattern 
	- Linked List Cycle
	- Palindrome Linked List
	- Cycle in a Circular Array
- More information: [[Linked List#Linked List Two Pointer]]

## 4. Merge Intervals
## 5. Cyclic Sort
## 6. In-place reversal of linked list

## 7. Tree BFS
- This pattern is based on the Breadth First Search (BFS) technique to traverse a tree and uses a queue to keep track of all the nodes of a level before jumping onto the next level.
- Any problem involving the traversal of a tree in a level-by-level order can be efficiently solved using this approach
- The Tree BFS pattern works by pushing the root node to the queue and then continually iterating until the queue is empty. For each iteration, we remove the node at the head of the queue and "visit" that node. After removing each node from the queue, we also insert all of its children into the queue.
- Some ways to identify that the given problem might require a Tree BFS pattern
	- If you' asked to traverse a tree in a level-by-level fashion (or level order traversal)
- Common problems you use the fast and slow pointers pattern 
	- Binary Tree Level Order Traversal
	- Zigzag Traversal
- [[Tree]]

## 8. Tree DFS
- Tree DFS is based on the Depth First Search (DFS) technique to traverse a tree.
- You can use recursion (or a stack for the iterative approach) to keep track of all the previous (parent) nodes while traversing
- The Tree DFS pattern works by starting at the root of the tree, if the node is not a leaf you need to do three things:
	- Decide whether to process the current node now (pre-order), or between processing two children (in-order) or after processing both children (post-order)
	- Make two recursive calls for both the children of the current node to process them
- Some ways to identify that the given problem might require a Tree BFS pattern
	- If you're asked to traverse a tree with in-order, preorder, or postorder DFS
	- If the problem requires searching for something where the node is closer to a leaf
- Common problems you use the fast and slow pointers pattern 
	- Sum of Path Numbers
	- All Paths for a Sum
- [[Tree]]
## 9. Two heaps
- In many problems, we are given a set of elements such that we can divide them into two parts. To solve the problem we are interested in knowing the smallest element in one part and the biggest element in the other part. This pattern is an efficient approach to solve such problem.
- This pattern uses two heaps. At any time the median of the current list of numbers can be calculated from the top element of the two heaps
- Ways to identify the Two Heaps pattern:
	- Useful in situations like Priority Queue, Scheduling
	- If the problem states that you need to find the smallest/largest/median elements of a set
	- Sometimes, useful in problems featuring a binary tree data structures
## 10. Subsets
- A huge number of coding interview problems involve dealing with Permutation and Combination of a given set of elements. The pattern Subsets describes an efficient Breadth First Search (BFS) approach to handle all these problems
![](https://i.imgur.com/L1oWwnS.png)
- Some ways to identify that the given problem might require a Tree BFS pattern
	- Problems where you need to find the combinations or permutations of a given set
- Common problems you use the fast and slow pointers pattern 
	- Subsets with Duplicates
	- String Permutations by changing case

## 11. Modified binary search
- Ref: [[Binary Search]]
- Whenever you are given a sorted array, linked list, or matrix, and are asked to find a certain element, the best algorithm you can use is Binary Search
- This pattern describes an efficient way to handle all problems involving Binary Search
![](https://i.imgur.com/Ba8xirK.png)
Common problems you use the modified binary search pattern
	- Orders-agnostic Binary Search
	- Search in a Sorted Infinite Array

## 12. Top K elements
[[Heap]]
- Any problem that asks us to find the top/smallest/frequent "K" elements among a given set falls under this pattern
- The best data structure to keep track of "K" elements is Heap. This pattern will make use of the Heap to solve multiple problems dealing with "K" elements at a time from a set of given elements.

- The pattern looks like this
	- We would need a way to continuously keep track of a (or many) smallest/largest element => Use a heap
	- Insert "K" elements into the min-heap or max-heap based on the problem
	- Iterate through the remaining numbers and if you find one that is larger than what you have in the heap, then remove that number and insert the larger one or decide what to do with the largest/smallest element (pop, replace, etc)

- **NOTE**: Python doesn't have built-in max-heap, only min-heap, we can use a trick which is multiply all element by -1 before adding to the heap, when we pop out we can multiply by -1 again to get the true value

- **References**: https://leetcode.com/tag/heap-priority-queue/discuss/2307171/Python-Heap-Guide-for-Beginners

- Some ways to identify the Top K element
	- If you're asked to find the top/smallest/frequent K elements of a given set
	- If you're asked to sort an array to find an exact element
- Common problems you use the Top K elements
	- Top K numbers
	- Top K frequent numbers

## 13. K-way Merge

## 14. Topological sort
- Topological Sort is used to find a linear ordering of elements that have dependencies on each other (event B is dependent on event A, A comes before B in topological ordering)
- [[Graph#Topological Sort]]
- Some ways to identify that the given problem might require a Topological Sort pattern
	- The problem will deal with graphs that have no directed cycles
	- If you're asked to update all objects in a sorted order
	- If you have a class of objects that follow a particular order
- Common problems you use the Topological Sort pattern 
	- Task Scheduling
	- Minimum height of a tree