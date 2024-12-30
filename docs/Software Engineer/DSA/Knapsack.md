# Overview
The Knapsack problem is a classic optimization problem in computer science. The core idea is to maximize the total value of items that can fit into a *knapsack* of limited capacity without exceeding it. 

 **Core Idea**
 - Input:
	 - A set of items, each a weight *w*, and a value *v*
	 - A maximum weight capacity *W* for the knapsack
 - Output:
	 - A subset of items that maximize the total value *V* such that total weight <= *W*

**Resources**
- https://usaco.guide/gold/knapsack?lang=py
# Variations
## 0/1 Knapsack
- Each item can either be included (1) or excluded (0)
- [[DP]] or [[Backtracking]] is used to decide the inclusion/exclusion of items
- **Example**
https://leetcode.com/problems/partition-equal-subset-sum
https://leetcode.com/problems/target-sum/
```python
def findTargetSumWays(self, nums: List[int], target: int) -> int:
        @lru_cache(None)
        def dp(index, cur_sum):
            if index >= len(nums) and cur_sum == target:
                return 1
            if index >= len(nums) and cur_sum != target:
                return 0
            
            positive = dp(index+1, cur_sum + nums[index])
            negative = dp(index+1, cur_sum - nums[index])
            
            return positive + negative
        
        return dp(0, 0)
```

## Fractional Knapsack
- Items can be broken into smaller parts, allowing a fractional amount of an item to  be included.
- Solved using [[Greedy]] as the optimal solution involves taking items with the highest value-to-weight ration until the capacity is reached
## Unbounded Knapsack
- Each item can be included multiple times
- Often solved using [[DP]]
- **Example**
 [https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)  
 https://leetcode.com/problems/coin-change-ii/description/
## Bounded Knapsack
- Each item has a limit on how many items it can be included
- Often solved using [[DP]]
- **Example**
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/
https://leetcode.com/problems/profitable-schemes
## Multi-dimensional Knapsack
- The knapsack has multiple constraint (e.g weight, volume)
- Solved using [[DP]] or **approximation technique**
- **Example**
https://leetcode.com/problems/ones-and-zeroes
## Subset Sum Problem
- A special case of the [[Knapsack#0/1 Knapsack]] where each item's value equals its weight, and the goal is to determine if a subset of items sums to exactly *W*
- Can be solved using [[DP]] or [[Backtracking]]
- **Example**
https://leetcode.com/problems/partition-equal-subset-sum
## Knapsack with Conflicts
- Certain items cannot be included together.
- Requires more advanced algorithms or modification to [[DP]]/[[Backtracking]]
- **Example**
https://leetcode.com/problems/house-robber
https://leetcode.com/problems/house-robber-ii