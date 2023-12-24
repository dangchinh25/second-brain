## Though Process
- Step to build solution
	- Decide what we have to do each step to bring us to the next valid state. Write down the recursive calls that implement this behavior
	- Come up with a way to construct the next optimal solution with all the data you got from subsequent recursive calls. Add a base case and complete the recursive function. => Bruteforce solution
	- Notice you have limited set of possible arguments combinations and cache the return value, so you avoid calling the function twice with the same argument => Memoization
	- Convert Memoization solution to use Tabulation (Bottom-up approach)

- Steps to convert a Top-down approach to Bottom-up approach
	- Create a dp table having the size to fit all the cached values of dp function call
	- Replace dp function definition by a loop over all possibles values of argument
		- Sometimes we need to be careful with the looping order. Since it is bottom up so we may need to iterate backward.
	- Replace dp function calls by calls to the dp table we have created before
	- Replace return call by setting dp cache value
	- Remove a base case as it is not needed anymore, and make sure you've initialized the dp table with values that cover the base case

**NOTES**: 
- Sometimes it is easy to think about a bottom up approach rather than recursion. 
- Sometimes if we given a grid or maybe the state involve 2 variable we can use a MxN table for tabulation (2D Dynamic Programming)

## Patterns
### Path to reach a target
- Practice: https://leetcode.com/list?selectedList=o9tcds8v
- Statement: 
> Given a target find minimum (maximum) cost/path/sum to reach the target
- Approach:
> Choose minimum (maximum) path among all possible paths before the current state, then add value for the current state
```python
routes[i] = min(routes[i-1], routes[i-2], ... , routes[i-k]) + cost[i]
```

#### Similar Problem
> You are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.

> You can either start from the step with index `0`, or the step with index `1`.

> Return _the minimum cost to reach the top of the floor_.
```python
 def minCostClimbingStairs(self, costs: List[int]) -> int:
        def dp(index, cost):
            if index > len(costs) - 1:
                return cost
            cur_cost = cost + costs[index]
            
            return min(dp(index+1, cur_cost), dp(index+2, cur_cost))
        
        return min(dp(0, 0), dp(1, 0))
```

>Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

> **Note:** You can only move either down or right at any point in time.

```python
def minPathSum(self, grid: List[List[int]]) -> int:
        rows, cols = len(grid), len(grid[0])
        
        @lru_cache(None)
        def dp(row, col):
            if row not in range(rows) or col not in range(cols):
                return -1
            if row == rows-1 and col == cols-1:
                return grid[row][col]
            right = dp(row, col+1)
            down = dp(row+1, col)
            
            if right == -1:
                return down + grid[row][col]
            elif down == -1:
                return right + grid[row][col]
            else:
                return grid[row][col] + min(down, right)
        
        return dp(0,0)
```

> You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

> Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

> You may assume that you have an infinite number of each kind of coin.
```python
def coinChange(self, coins: List[int], amount: int) -> int:
        @lru_cache(None)
        def dp(amount):
            if amount == 0:
                return 0
            if amount < 0:
                return float("inf")
            res = []
            for val in coins:
                res.append(dp(amount-val))
            
            return 1 + min(res)
        
        res = dp(amount)
        if res == float("inf"):
            return -1
        
        return res
```

### Distinct Ways
- Practice: https://leetcode.com/list?selectedList=o9tc73it
- Statement:
> Given a target find a number of distinct ways to reach the target
- Approach
> Sum all possible ways to reach the current state
```python
routes[i] = routes[i-1] + routes[i-2], ... , + routes[i-k]
```

#### Similar Problem
https://leetcode.com/problems/climbing-stairs/
> You are climbing a staircase. It takes `n` steps to reach the top.

> Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?
```python
def climbStairs(self, n: int) -> int:
        def dp(state):
            if state <= 2:
                return state
            return dp(state-1) + dp(state-2)

        return dp(n)
```

https://leetcode.com/problems/target-sum/
> You are given an integer array `nums` and an integer `target`.

> You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

> For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

> Return the number of different **expressions** that you can build, which evaluates to `target`.

```python
def findTargetSumWays(self, nums: List[int], target: int) -> int:
        def dp(index, cur_sum):
            if index < 0 and cur_sum == target:
                return 1
            if index < 0:
                return 0
            
            positive = dp(index-1, cur_sum + nums[index])
            negative = dp(index-1, cur_sum - nums[index])
            
            return positive + negative
        
        return dp(len(nums)-1, 0)
```

https://leetcode.com/problems/unique-paths/
> There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

> Given the two integers `m` and `n`, return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.

```python 
def uniquePaths(self, rows: int, cols: int) -> int:
        @lru_cache(None)
        def dp(row, col):
            if row not in range(rows) or col not in range(cols):
                return 0
            if row == rows-1 and col == cols-1:
                return 1
            
            return dp(row+1, col) + dp(row, col+1)
        
        return dp(0, 0)
```

### Merging Interval
- Practice: [DP(merging intervals)](https://leetcode.com/list/?selectedList=o9karism#)
- Statement:
> Given a set of numbers find an optimal solution for a problem considering the current number and the best you can get from the left and right side
- Approach
> Find all optimal solutions for every interval and return the best possible answer. Get the best from the left and right sides and add a solution for the current position.

```python
// from i to j
dp[i][j] = dp[i][k] + result[k] + dp[k+1][j]
```
#### Similar Problem

### DP on String
- Practice: [DP on strings](https://leetcode.com/list?selectedList=o9kae6yh#)
- Statement
> General problem statement for this pattern can vary but most of the time you are given two strings where lengths of those strings are not big
> 
> Given two string s1 and s2 return some result
- Approach:
> Most of the problems on this pattern requires a solution that can be accepted O(n^2) complexity. -   DP on strings is usually, if not always, done by comparing two chars at a time.

- Trick
- [[DP#Longest Increasing Subsequence]]]

```c
// i - indexing string s1
// j - indexing string s2
for (int i = 1; i <= n; ++i) {
   for (int j = 1; j <= m; ++j) {
       if (s1[i-1] == s2[j-1]) {
           dp[i][j] = /*code*/;
       } else {
           dp[i][j] = /*code*/;
       }
   }
}
```
> If you are given one string s the approach may vary

```c
for (int l = 1; l < n; ++l) {
   for (int i = 0; i < n-l; ++i) {
       int j = i + l;
       if (s[i] == s[j]) {
           dp[i][j] = /*code*/;
       } else {
           dp[i][j] = /*code*/;
       }
   }
}
```
#### Similar Problem
https://leetcode.com/problems/palindromic-substrings/
> Given a string `s`, return _the number of **palindromic substrings** in it_.
>A string is a **palindrome** when it reads the same backward as forward.
>A **substring** is a contiguous sequence of characters within the string.

```python
 def countSubstrings(self, s: str) -> int:
        @lru_cache(None)
        def helper(left, right):
            if left > right:
                return True
            if s[left] != s[right]:
                return False
            return helper(left+1, right-1)
        
        n = len(s)
        res = 0
        for i in range(n):
            for j in range(i, n):
                if helper(i, j):
                    res +=1
        
        return res
        
```

### Decision Making
- Practice: [DP (decision making)](https://leetcode.com/list?selectedList=o9kao843#)
- Statement
> The general problem statement for this is for given situation decide whether to use or not to use the current state. So the problem requires you to make a decision at a current state.
- Approach
> If you decide to choose the current value use the previous result where the value was ignored; vice-versa, if your decide to ignore the current value use the previous result where value was used

#### Similar Problem
https://leetcode.com/problems/house-robber/
> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

> Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

```python
def rob(self, nums: List[int]) -> int:
        @lru_cache(None)
        def dp(index, money):
            if index > len(nums) - 1:
                return money
            
            rob = dp(index+2, money + nums[index])
            no_rob = dp(index+1, money)
            
            return max(rob, no_rob)
        
        return dp(0, 0)

def rob(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        if len(nums) == 1:
            return nums[0]
        
        gain = [0] * len(nums)
        gain[0] = nums[0]
        gain[1] = max(nums[1], nums[0])
        
        for i in range(2, len(nums)):
            gain[i] = max(gain[i-1], nums[i] + gain[i-2])
        
        return gain[-1]
```

## Variant
### Longest Increasing Subsequence
- Problems
[https://leetcode.com/problems/longest-increasing-subsequence/](https://leetcode.com/problems/longest-increasing-subsequence/)  
[https://leetcode.com/problems/largest-divisible-subset/](https://leetcode.com/problems/largest-divisible-subset/)  
[https://leetcode.com/problems/russian-doll-envelopes/](https://leetcode.com/problems/russian-doll-envelopes/)  
[https://leetcode.com/problems/maximum-length-of-pair-chain/](https://leetcode.com/problems/maximum-length-of-pair-chain/)  
[https://leetcode.com/problems/number-of-longest-increasing-subsequence/](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)  
[https://leetcode.com/problems/delete-and-earn/](https://leetcode.com/problems/delete-and-earn/)  
[https://leetcode.com/problems/longest-string-chain/](https://leetcode.com/problems/longest-string-chain/)
-  Travel from right to left so that we have small case/subproblem already handled as when we process a number, we know that all the number after it has already been processed
- The smallest subproblem is the element at the end of the array, at which there is always only 1 result. We traverse from right to left to use this smallest case and gradually build bigger case
- If we need to know the number of times each subsequence length appear, we can have another array to save it, or instead of using a table like normal dp we can use a hashmap with key is the index and value is the length and its number of occurence
- Ref [[Blind 75-150#^850c48 | Longest Increasing Subsequence]]

## Resources
- https://leetcode.com/discuss/study-guide/1490172/Dynamic-programming-is-simple
- https://leetcode.com/discuss/career/1029985/losing-all-hopes-of-coding-please-help
- https://leetcode.com/discuss/study-guide/662866/DP-for-Beginners-Problems-or-Patterns-or-Sample-Solutions
- https://leetcode.com/discuss/study-guide/458695/Dynamic-Programming-Patterns
- https://leetcode.com/discuss/general-discussion/475924/my-experience-and-notes-for-learning-dp
- https://leetcode.com/problems/target-sum/discuss/455024/dp-is-easy-5-steps-to-think-through-dp-questions/424058