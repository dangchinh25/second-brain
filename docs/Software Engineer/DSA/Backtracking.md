https://leetcode.com/tag/backtracking/discuss/1405817/Backtracking-algorithm-%2B-problems-to-practice
https://leetcode.com/tag/backtracking/discuss/1141947/Backtracking%3A-Study-and-Analysis
- **Idea**:
> Backtracking can be seen as an optimized way to brute force. Bruteforce approaches and evaluate every  possibility. 
> In backtracking you stop evaluating a possibility as soon it breaks some constraint provided in the problem, take a step back and keep trying other possible cases, see if those lead to a valid solution
> It kinda like a DFS with an added constraint that we stop exploring the subtree as soon as we know for sure that it won't lead to a valid solution
- **Identifier**:
	- You are explicitly asked to return a collection of all answers
	- **You are concerned with what the actual solutions are rather than say the most optimum value of some parameter (if it were the latter it's most likely DP or greedy)**
- General template
```python
def backtrack(arguments) {
	if (condition == true) { // Condition when we should stop our exploration.
		result.append(current);
		return;
	}
	for (int i = num; i <= last; i++) {
		current.append(i); // Explore candidate.
		backtrack(arguments);
		current.pop();   // Abandon candidate.
	}
}

def backtrack(arguments) {
	if (condition == true) { // Condition when we should stop our exploration.
		result.append(current);
		return;
	}
	
	current.append(i); // Explore candidate.
	backtrack(arguments);
	current.pop();   // Abandon candidate.
	backtrack(arguments) // Explore case when this candidate is not included
}


```
- The idea is that we keep a `set/array` or sth similar to keep track of what is currently being explored
- Before the actual processing we add new item in the `set/array` to indicate that we gonna explore that item, and after done processing we pop the item out to begin a new loop to explore another item.
- **It's also worth noting that usually the working set holds the value that later on can be pushed to the result array
> Choose => Process => Unchoose
- It is also important to understand what argument should we used for the `helper()` function that will explore an item. Normally it's gonna be the `location`(?) or some kind of identifier for the item that we are current tracking
- Ref: [[Blind 75-150#^3a0803| Combination sum]]
- Example: 
https://leetcode.com/problems/subsets/
```python
 def subsets(self, nums: List[int]) -> List[List[int]]:
        result = []
        
        workingSet = []
        def dfs(i):
            if i >= len(nums):
                result.append(workingSet.copy())
                return
            
            workingSet.append(nums[i])
            dfs(i+1)
            
            workingSet.pop()
            dfs(i+1)
            
        dfs(0)
        
        return result
```
https://leetcode.com/problems/combinations/
```python
 def combine(self, n: int, k: int) -> List[List[int]]:
        result = []
        working = []
        
        def dfs(num):
            if len(working) == k:
                result.append(working.copy())
                return
            for i in range(num+1, n+1):
                working.append(i)
                dfs(i)
                working.pop()
        dfs(0)
        return result
```
**Notes**:
> - This problem is pretty standard where we need to explore the current number but we cant pass the current number to the next iteration because we don't allow duplication => Really popular in Backtracking problem
> - The solution is only iterate in range (num+1, n) and then when we call the backtracking function in the outer loop we can step one back so that +1 will be the one that we want to explore (in this case the number is in range(1, ...) we can call with 0).
> - Another maybe possible solution is to still iterate using (num, n) but when we call backtracking we call with (num+1)

[[Blind 75-150#^8990f6 | Subset 2]]
- This problem is quite common where we want to generate a combination from a list with duplication => Sort it [[Cheap Trick#^73797d | Duplication handling trick]]
	- The intuition behind this is that at each step, we have 2 decision `include or not include`. Let's say we are exploring `include` case, we know what somewhere down the line we probably handle the case where we `include index + 1`, so we want to skip that?


- **Permutation**: Another trick pattern in backtracking is Permutation where we want to check possible permutation of an array. Pretty classic backtracking where we want to explore all permutation starts from each value and pop it out and explore the next one. The idea is we once we choose to include an index, we can remove it from the array and use the new array for the recursive call so that it only get permutation for the smaller subarray. For the following example: If choose index 0, the recursive call will only operate on the subarray without the index 0.
	- [[Useful math#^082db8 | Useful math: Calculate total of permutations]]
```python
def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
		working = []
		def backtrack(nums):
			if not nums:
				res.append(working.copy())
				return
			for i in range(len(nums)):
				working.append(nums[i])
				backtrack(nums[:i] + nums[i+1:])
				working.pop()
		backtrack(nums)
		
		return res
```
- [[Blind 75-150#^d4487e | Permutation 2]] is example of permutation pattern but with [[Cheap Trick#^73797d | Duplication handling trick]] (check intuition above)

- These 2 questions below are quite interesting as it demonstrate one of the core idea of backtracking quite clearly *evaluate every possibilities and try to break as soon as possible*
	- The idea for both problems is quite simple and similar, but we have to identify all cases where we can break earlier than just simply iterating through every options
https://leetcode.com/problems/fair-distribution-of-cookies/description/
```python
 def distributeCookies(self, cookies: List[int], k: int) -> int:
	res = float('inf')
	bag_sums = [0] * k

	def backtrack(index):
		nonlocal res
		if index >= len(cookies):
			res = min(res, max(bag_sums))
			return
		
		for i in range(k):
			if bag_sums[i] + cookies[index] >= res:
				continue
			bag_sums[i] += cookies[index]
			backtrack(index+1)
			bag_sums[i] -= cookies[index]
			if bag_sums[i] == 0:
				break
	
	backtrack(0)
	return res
```
- https://leetcode.com/problems/partition-to-k-equal-sum-subsets/description/
	- Here, since we only need to return True or False, the trick is to reverse sort the list so that we can exit as soon as possible
```python
 def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
	if sum(nums) % k != 0:
		return False
	
	target = sum(nums) // k
	nums.sort(reverse=True)
	subset_sums = [0] * k

	def backtrack(index):
		if index >= len(nums):
			return all(subset_sum == target for subset_sum in subset_sums)

		for i in range(k):
			if subset_sums[i] + nums[index] > target:
				continue
			subset_sums[i] += nums[index]
			if backtrack(index + 1):
				return True
			subset_sums[i] -= nums[index]
			
			if subset_sums[i] == 0:
				break
		return False

	return backtrack(0)
```
## Different vs Dynamic Programming
- Unlike [[DP | Dynamic Programming]], backtracking is typically not looking for one optimal situation, but is instead looking for **all that satisfy some criteria**.

## Resources
https://betterprogramming.pub/the-technical-interview-guide-to-backtracking-e1a03ca4abad
https://guides.codepath.com/compsci/Backtracking