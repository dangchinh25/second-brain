- The idea is quite simple but difficult to write bug-free implementation code in just a few minutes
	- When to exit the loop? Should we use `left < right` or `left <= right` as the while loop condition?
	- How to initialize the boundary variable `left` and `right`?
	- How to update the boundary? How to choose the appropriate combination from `left= mid`, `left = mid + 1`, and `rigth = mid`, `right = mid - 1`
- Another misunderstanding of binary search is that people often think this technique could only be used in simple scenario like given a sorted array, find a specific value in it => Can be applied to much more complicated situations.

## Idea
- Most of the time, the problem is not just find a target number in a sorted array. In this case the sorted array is a **search space** and target number is the **search target**. In some problem the **search space** and **search target** is not so readily available. => Have to correctly find and declare the **search space** and **search target** for binary search to work.
- *When can we use binary search?* => **If we can discover some kind of monotonicity, e.g if condition(k) is True then condition(k+1) is True** then we can consider binary search
- To stop thinking about binary search, we can think of these problems as **Finding the smallest number that satisfy a condition**

## Template
```python
def binary_search(array) -> int:
    def condition(value) -> bool:
        pass

    left, right = min(search_space), max(search_space) # could be [0, n], [1, n] etc. Depends on problem
    while left < right:
        mid = left + (right - left) // 2
        if condition(mid):
            right = mid
        else:
            left = mid + 1
    return left
```
- When using this template, **we only need to modify three parts**
	- Correctly initialize `left` and `right` to specify search space. One rule is to setup the boundary to **include all possible elements**
	- Decide return values. Is it `return left` or `return left - 1`? Remember this: **after exiting the while loop**, `left` **is the minimal k satisfying the `condition` function. 
	- Design `condition` function. 

## Example

[278. First Bad Version [Easy]](https://leetcode.com/problems/first-bad-version/)
You are a product manager and currently leading a team to develop a new product. Since each version is developed based on the previous version, all the versions after a bad version are also bad. Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad. You are given an API `bool isBadVersion(version)` which will return whether `version` is bad.
**Example:**
```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 
```

First, we initialize `left = 1` and `right = n` to include all possible values. Then we notice that we don't even need to design the `condition` function. It's already given by the `isBadVersion` API. Finding the first bad version is equivalent to finding the minimal k satisfying `isBadVersion(k) is True`. Our template can fit in very nicely:

```python
def firstBadVersion(self, n) -> int:
	left, right = 1, n
	while left < right:
		mid = left + (right - left) // 2
		if isBadVersion(mid):
			right = mid
		else:
			left = mid + 1
	return left
```


https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/
- Idea:
	- We are given an array of weights and a day limit
	- We might automatically treat *weights* as search space 
	- The way to look at this problem is 
		- We are looking at the minimal one among many feasible capacities. If we can successfully ship all packages within `D` days with capacity `m`, then we can definitely ship them all with any capacity larger than `m`.
		- We can design a `condition` function, given an input `capacity`, it returns whether it's possible to ship all packages within `D` days. This is just a regular function with a for loop.
		- In this case the *search space* is the `capacity`, where *minimum* is `max(weights)` as it has to be able to ship the heaviest package, and *maximum* is `sum(weights)` as if the capacity is `sum(weight)` we can ship all packages in one day.
- Now we can apply the template
```python
def shipWithinDays(weights: List[int], D: int) -> int:
    def feasible(capacity) -> bool:
        days = 1
        total = 0
        for weight in weights:
            total += weight
            if total > capacity:  # too heavy, wait for the next day
                total = weight
                days += 1
                if days > D:  # cannot ship within D days
                    return False
        return True

    left, right = max(weights), sum(weights)
    while left < right:
        mid = left + (right - left) // 2
        if feasible(mid):
            right = mid
        else:
            left = mid + 1
    return left
```

[[Blind 75-150#^e3a2b2 | Koko eating bananas]]

https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag
- Search max size of each bags
- At each `mid` value, check if we can split every bag to the `mid` value within `maxOperations`
```python
def minimumSize(self, nums: List[int], maxOperations: int) -> int:
	def split(size):
		count = 0
		for num in nums:
				count += num // size
				# split 9 to 3,3,3 take 2 split instead of 3
				if num % size == 0:
					count -= 1
		return count <= maxOperations
	
	# search the max size of the bags
	left, right = 1, max(nums)
	while left < right:
		mid = left + (right-left)//2
		if split(mid):
			right = mid
		else:
			left = mid + 1
	return left
```

https://leetcode.com/problems/minimized-maximum-of-products-distributed-to-any-store
```python
def minimizedMaximum(self, n: int, quantities: List[int]) -> int:
	def canSplit(num):
		count = 0
		for q in quantities:
			count += math.ceil(q / num)
		return count
	
	# search over number of products each store can have
	left, right = 1, max(quantities)
	while left < right:
		mid = left + (right-left)//2
		count = canSplit(mid)
		# if we can distribute with current num of product, maybe we can push for it and do less?
		if count <= n:
			right = mid
		# the num of product is too little => have excess products
		else:
			left = mid + 1
	return left
```

https://leetcode.com/problems/maximum-candies-allocated-to-k-children/description/
- Similar intuition
```python
 def maximumCandies(self, candies: List[int], k: int) -> int:
	def canAllocate(num):
		count = 0
		for c in candies:
			count += c // num
		return count >= k
	
	left, right = 1, max(candies)
	res = 0

	while left <= right:
		mid = left + (right - left)//2
		if canAllocate(mid):
			res = max(res, mid)
			left = mid + 1
		else:
			right = mid - 1
	return res
```

## Tips:
When implementing binary search, choosing between left <= right vs left < right or using right = mid vs right = mid - 1 depends on:
1. **Inclusivity of Bounds**:
• left <= right: Used when right is a valid candidate for the answer.
• left < right: Used when right is not a valid candidate, so the range [left, right] reduces to [left, mid) or (mid, right].

2. **Mid Calculation**:
• Always calculate mid = left + (right - left) // 2 to avoid overflow.

3. **Shrinking the Range**:
• right = mid - 1: Excludes mid from the search space, typically when mid has already been validated as not meeting the condition.
• right = mid: Includes mid in the search space if mid is a possible answer.

**Tricks**: 
- If we want to include the `right` value as a candidate to the answer and avoid infinite loop
	- Do `left <= right` and when shrinking the range, do `right = mid - 1` => if `mid` is a valid answer, we already account for it by the `left == right` case
	- Do `left < right` and when shrinking the range, do `right = mid`, **but we have to initialize `right = max(searchSpace) + 1`  so that we include it by default


https://leetcode.com/problems/magnetic-force-between-two-balls
- This is also a good one
```python
 def maxDistance(self, position: List[int], m: int) -> int:
	position = sorted(position)
	
	def canDistribute(force):
		count = 1
		pos = position[0]

		for i in range(1, len(position)):
			if position[i] - pos >= force:
				count += 1
				pos = position[i]
		return count >= m
	
	left, right = 0, position[-1] - position[0]
	# left <= right because right value is also a valid answer
	while left <= right:
		mid = left + (right-left)//2
		# exclude mid from the search
		if canDistribute(mid):
			left = mid + 1
		else:
			right = mid - 1
	return left - 1 # handle left == right in the last iteration
```

https://leetcode.com/problems/search-in-rotated-sorted-array/description/
```python
def search(self, nums: List[int], target: int) -> int:
	left, right = 0, len(nums) - 1
	while left <= right:
		mid = left + (right-left)//2

		if nums[mid] == target:
			return mid
		
		# Case 2: subarray on mid's left is sorted
		elif nums[mid] >= nums[left]:
			if target >= nums[left] and target < nums[mid]:
				right = mid - 1
			else:
				left = mid + 1
		# Case 3: subarray on mid's right is sorted.
		else:
			if target <= nums[right] and target > nums[mid]:
				left = mid + 1
			else:
				right = mid - 1
	
	return -1
```
## Resources:
- https://leetcode.com/discuss/study-guide/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems