## Though Process
- Step to build solution
	- **Decide what we have to do each step to bring us to the next valid state**. Write down the recursive calls that implement this behavior
		- This can be think of at each step and each valid state, **what are the choices that we can make**
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
- Sometimes it is easier to think about a bottom up approach rather than recursion. 
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

- [[Blind 75-150#^7db511 | Notes]]
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
> General problem statement for this pattern can vary but most of the time you are given two strings where **lengths of those strings are not big**
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

## 2D DP
- Use **2D DP** if the problems depends on two incides or dimensions
	- **Knapsack problems** where the states depends on the item index and the remaining capacity
	- **String problems**: Where we compare characters of two string
	- **Grid problems**: Path finding
- This explain the thought process behind 2d array pretty well: https://www.youtube.com/watch?v=Mjy4hd2xgrs
	- Ref: [[Blind 75-150#^df7a0c | Coin Change 2]]
- **Though process**: 
	- Define the subproblem => what does `dp[i][j]` represents
	- Identify the dimensions
	- Initialize base case
	- Define recurrence relation using subproblems
	- How to iterate the tables


- https://leetcode.com/problems/longest-common-subsequence/
```python
# At each cell, store the result of the subproblem which is the LCS of the first character in text1 and first character in text2
def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        dp = [[0 for i in range(n+1)] for j in range(m+1)]

        for i in range(m-1, -1, -1):
            for j in range(n-1, -1, -1):
                if text1[i] == text2[j]:
                    dp[i][j] = 1 + dp[i+1][j+1]
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j+1])
        
        return dp[0][0]
```
- https://leetcode.com/problems/edit-distance
```python
# At each cell, store the minimum number of operations to convert the first i character of word1 to first j characters of word2
def minDistance(self, word1: str, word2: str) -> int:
        rows, cols = len(word1), len(word2)
        dp = [[0 for i in range(cols + 1)] for j in range(rows + 1)]

        for i in range(rows + 1):
            dp[i][0] = i
        for j in range(cols+1):
            dp[0][j] = j

        for i in range(1, rows + 1):
            for j in range(1, cols + 1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    insert = dp[i][j-1] + 1
                    delete = dp[i-1][j] + 1
                    replace = dp[i-1][j-1] + 1
                    dp[i][j] = min(insert, delete, replace)
        
        return dp[-1][-1]
```
- https://leetcode.com/problems/delete-operation-for-two-strings/
```python
def minDistance(self, word1: str, word2: str) -> int:
        rows, cols = len(word1), len(word2)
        dp = [[0 for i in range(cols+1)] for j in range(rows + 1)]

        for i in range(rows + 1):
            dp[i][0] = i
        for j in range(cols + 1):
            dp[0][j] = j

        for i in range(1, rows+1):
            for j in range(1, cols+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    del1 = dp[i-1][j] + 1
                    del2 = dp[i][j-1] + 1
                    dp[i][j] = min(del1, del2)
        
        return dp[-1][-1]
```
- https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings
```python
def minimumDeleteSum(self, s1: str, s2: str) -> int:
        rows, cols = len(s1) + 1, len(s2) + 1
        dp = [[0 for i in range(cols)] for j in range(rows)]

        for i in range(1, rows):
            dp[i][0] = dp[i-1][0] + ord(s1[i-1])
        
        for i in range(1, cols):
            dp[0][i] = dp[0][i-1] + ord(s2[i-1])

        for i in range(1, rows):
            for j in range(1, cols):
                if s1[i-1] == s2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    del1 = dp[i-1][j] + ord(s1[i-1])
                    del2 = dp[i][j-1] + ord(s2[j-1])
                    dp[i][j] = min(del1, del2)
        
        return dp[-1][-1]
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
	- E.g: When we at 3, and we meet 5 we can already know what is the maximum that can be obtained after 5 (because we traverse from right to left and populate the *memoization table* )
- The smallest subproblem is the element at the end of the array, at which there is always only 1 result. We traverse from right to left to use this smallest case and gradually build bigger case
- If we need to know the number of times each subsequence length appear, we can have another array to save it, or instead of using a table like normal dp we can use a hashmap with key is the index and value is the length and its number of occurence
- Ref [[Blind 75-150#^850c48 | Longest Increasing Subsequence]]

## Algorithm
### Kadane
- Kadane’s algorithm is a popular method used to solve the **maximum subarray sum problem** efficiently. The goal is to find the maximum sum of a contiguous subarray within a given one-dimensional array of numbers, which can include negative values.
1. **Iterate through the array**: At each position, determine the maximum sum of the subarray ending at that position.
2. **Dynamic programming principle**:
	• Keep track of the maximum subarray sum ending at the current index (current_sum).
		- At each step, we update the current max as if **absolutely have to** include the current value
		- By doing this, the next iteration can build up on this and also update its local maximum
		- This only works because we are dealing with *contiguous subarray* so to update one local maxima we need to know the local maximum **right before it**
	• Update current_sum as the maximum of:
		• The current element itself (starting a new subarray).
		• The sum of current_sum and the current element (extending the existing subarray).
	• Maintain a global max_sum to store the maximum sum encountered so far. ^a4fdb4
3. **Base case**: Start with the first element, as the maximum sum for a subarray including only the first element is itself.

- A popular problem that utilizes this directly is https://leetcode.com/problems/maximum-subarray
```python
 def maxSubArray(self, nums: List[int]) -> int:
	res = float('-inf')
	current = 0

	for num in nums:
		current = max(num, current + num)
		res = max(res, current)
	
	return res
```

> While this algorithm may feels like a specific answer to a Leetcode question, it demonstrate an instance of *Dynamic Programming* principle where use the solution to subproblems to build solution for the overall problem.

- **Why Kadane’s Works**
1. **Local Optimality**: At each step, we make the best local choice (add to the current subarray or start a new one).
2. **Global Optimality**: By tracking the global maximum (max_sum), we ensure that the solution considers all possibilities across the array.
3. **Avoid Redundant Work**: Instead of recalculating subarray sums repeatedly, Kadane’s reuses the computation from the previous step, achieving O(n) efficiency.

- **Mental Model** [[DP#^a4fdb4]]
> Kadane’s algorithm can be thought of as walking through the array while carrying a “bag” of the current subarray sum:
	• If adding an item to the bag makes the total heavier in a good way, keep it.
	• If the item is so heavy that it ruins the value of the bag, dump the bag and start fresh.
	- By the end of the walk, the heaviest bag you carried at any point is your answer.

- **When to Use Kadane’s Algorithm**
	1.	Contiguous Subarrays are Required:
	•	The problem explicitly asks for the maximum sum (or another optimization) of a contiguous subarray.
	2.	Linear Time Complexity is Essential:
	•	If the array size (n) is large and you need a solution in O(n), Kadane’s algorithm is perfect due to its efficiency.
	3.	Problem Involves a Simple Aggregation Metric:
	•	The problem revolves around computing sums, products, or similar metrics that can be handled incrementally with the same greedy approach (e.g., maximum sum, maximum product, etc.).
	4.	Input Contains Both Positive and Negative Numbers:
	•	Kadane’s algorithm efficiently handles arrays with mixed signs, leveraging its decision-making process (extend the current subarray or start fresh).
	5.	Variants That Fit the Framework:
Kadane’s logic can be adapted to problems like:
	•	Maximum product subarray (with some modifications).
	•	Maximum circular subarray sum.
	•	Problems involving maximizing/minimizing a sequence under similar constraints.

When Not to Use Kadane’s Algorithm

Kadane’s algorithm isn’t a universal solution. Be cautious in the following scenarios:
	1.	Non-Contiguous Subarrays:
	•	Kadane’s algorithm assumes subarrays are contiguous. If the problem allows non-contiguous subarrays, other techniques like dynamic programming or combinatorial approaches may be required.
	•	Example: “Find the maximum sum of non-adjacent elements.”
	2.	Constraints Beyond Aggregation:
	•	If the problem has additional constraints (e.g., subarray length, specific elements included or excluded), Kadane’s algorithm might not work directly.
	•	Example: “Find the maximum sum of a subarray of exactly length k.”
	3.	Arrays with Special Structures:
	•	Kadane’s algorithm doesn’t handle:
	•	Multi-dimensional arrays: The logic needs substantial modifications for 2D or higher-dimensional arrays (e.g., maximum sum rectangle in a matrix).
	•	Non-linear structures: If the input isn’t a 1D array (e.g., graphs or trees), Kadane’s doesn’t apply directly.
	4.	Edge Cases to Consider:
	•	All-negative arrays: Kadane’s works, but you should confirm whether the problem allows subarrays of size 0 (in which case the answer might be 0 instead of the largest negative number).
	•	Empty arrays: Ensure the problem definition handles this gracefully.
	5.	Problem Focused on Subsequence, Not Subarray:
	•	Kadane’s doesn’t work when the elements don’t need to be contiguous.
	•	Example: “Find the longest increasing subsequence” or “Find the maximum sum subsequence.”
	6.	Highly Complex Objectives:
	•	If the objective isn’t straightforward (e.g., involving multiple variables or constraints), a more general dynamic programming solution might be better.
	•	Example: “Partition the array into two subarrays such that the difference of their sums is minimized.”

What to Look Out For

Input and Problem Characteristics:
	1.	Are subarrays required to be contiguous?
	•	Yes → Kadane’s works.
	•	No → Look for other algorithms.
	2.	Does the problem involve finding maximum/minimum aggregation?
	•	Yes → Kadane’s may be a good fit.
	•	No → Kadane’s is unlikely to help.
	3.	Are there additional constraints?
	•	Yes → Kadane’s needs modification or might not be suitable.

Common Alternatives to Kadane’s Algorithm

When Kadane’s doesn’t work, consider these alternatives:
	1.	Dynamic Programming:
	•	For problems with additional constraints or involving non-contiguous subsequences.
	•	Example: “Maximum sum of non-adjacent elements.”
	2.	Sliding Window Technique:
	•	For problems with fixed-length subarrays.
	•	Example: “Find the maximum sum of a subarray of length k.”
	3.	Divide and Conquer:
	•	For finding maximum subarray sums but with potential for parallelization.
	•	Example: Divide the array into halves and recursively compute the maximum subarray sum.
	4.	Greedy Algorithms:
	•	For problems involving subsequences or where local optimization leads to global optimization.
	5.	Multi-dimensional Algorithms:
	•	For 2D arrays or higher-dimensional structures.
	•	Example: “Kadane’s for 2D matrices” or “Maximum sum rectangle.”
## Resources
- Must read:
	- https://leetcode.com/discuss/study-guide/458695/Dynamic-Programming-Patterns
	- https://leetcode.com/discuss/study-guide/1308617/Dynamic-Programming-Patterns
- Problems set:
	- https://leetcode.com/discuss/general-discussion/1000929/solved-all-dynamic-programming-dp-problems-in-7-months
	- https://www.reddit.com/r/leetcode/comments/14o10jd/the_ultimate_dynamic_programming_roadmap/
- https://leetcode.com/discuss/study-guide/1490172/Dynamic-programming-is-simple
- https://leetcode.com/discuss/career/1029985/losing-all-hopes-of-coding-please-help
- https://leetcode.com/discuss/study-guide/662866/DP-for-Beginners-Problems-or-Patterns-or-Sample-Solutions
- https://leetcode.com/discuss/general-discussion/475924/my-experience-and-notes-for-learning-dp
- https://leetcode.com/problems/target-sum/discuss/455024/dp-is-easy-5-steps-to-think-through-dp-questions/424058