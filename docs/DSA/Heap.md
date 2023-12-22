## Priority Queue
- Similar to normal queue except that *each element has a certain priority*
- The *priority* of the elements in the priority queue determine the order in which elements are removed from the PQ
- How does a PQ knows which element to remove (i.e how does it keep track of value and priority of each element) => [[Software Engineer/DSA/Heap#Heap| Heap]] **because when we give more priority to smallest or the largest node in the tree as we can extract these node very efficiently using heaps** (Heap is the not the only to implement PQ it's just the most common and efficient way)

## Heap
- A heap is a tree based DS that satisfies the **heap property**: If A is a parent node of B then A is ordered with respect to B for all nodes A, B in the hope => This means that **the value of the parent node always larger than or equal to the value of child node for all node, or vice versa**
![](https://i.imgur.com/bLRigqe.png)
- 2 types of Heap
	- Max Heap: value of parent always larger than value of children
	- Min Heap: value of parent always smaller than value of children
![](https://i.imgur.com/g7lG6ao.png)
