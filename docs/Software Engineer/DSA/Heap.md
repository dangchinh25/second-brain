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

Example:
https://leetcode.com/problems/kth-largest-element-in-an-array/description/
- Most basic use of heap
```python
def findKthLargest(self, nums: List[int], k: int) -> int:
	heap = []
	
	for num in nums:
		heapq.heappush(heap, num)

		if len(heap) > k:
			heapq.heappop(heap)

	return heapq.heappop(heap)
```

- Top K Frequent Element - https://leetcode.com/problems/top-k-frequent-elements/
	- Also pretty standard and straightforward use of heap
```python
def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        frequency = {}
        for num in nums:
            if num not in frequency:
                frequency[num] = 0
            frequency[num] +=1
        
        items = [(-freq, num) for num, freq in frequency.items()]
        heapq.heapify(items)
        
        result = []
        for i in range(k):
            freq, num = heapq.heappop(items)
            result.append(num)
            
        return result
```

**NOTES**: `heapq.heapify` in python creates a *min heap* by default, if we want a *max heap*, we can multiply all value by -1 as seen in the code above (where we do `-freq` ) 

https://leetcode.com/problems/meeting-rooms-ii/description/
```python
def minMeetingRooms(self, intervals: List[List[int]]) -> int:
	# this can be thought of as occupying room
	heap = []
	intervals.sort()
	heapq.heappush(heap, intervals[0][1])

	for start, end in intervals[1:]:
		# free up a room by popping it off the heap
		if heap[0] <= start:
			heapq.heappop(heap)
		heapq.heappush(heap, end)
	
	return len(heap)
```

https://leetcode.com/problems/brightest-position-on-street
- We can also use a *HashMap* to store the range differences too
	- This can be especially *helpful when we have to deal with negative ranges*
```python
def brightestPosition(self, lights: List[List[int]]) -> int:
	diff = collections.defaultdict(int)

	for i, dist in lights:
		diff[i-dist] += 1
		diff[i+dist+1] -= 1

	cur_sum = 0
	max_idx = 0
	max_val = 0

	for idx, val in sorted(diff.items()):
		cur_sum += val
		if cur_sum > max_val:
			max_val = cur_sum
			max_idx = idx
	
	return max_idx
```