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
void backtrack(arguments) {
	if (condition == true) { // Condition when we should stop our exploration.
		result.push_back(current);
		return;
	}
	for (int i = num; i <= last; i++) {
		current.push_back(i); // Explore candidate.
		backtrack(arguments);
		current.pop_back();   // Abandon candidate.
	}
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
- **Permutation**: Another trick pattern in backtracking is Permutation where we want to check possible permutation of an array. Pretty classic backtracking where we want to explore all permutation starts from each value and pop it out and explore the next one. The idea is we once we choose to include an index, we can remove it from the array and use the new array for the recursive call so that it only get permutation for the smaller subarray. For the following example: If choose index 0, the recursive call will only operate on the subarray without the index 0.
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
## Different vs Dynamic Programming
- Unlike [[DP | Dynamic Programming]], backtracking is typically not looking for one optimal situation, but is instead looking for **all that satisfy some criteria**.

## Resources
https://betterprogramming.pub/the-technical-interview-guide-to-backtracking-e1a03ca4abad
https://guides.codepath.com/compsci/Backtracking