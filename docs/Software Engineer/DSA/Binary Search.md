- The idea is quite simple but difficult to write bug-free implementation code in just a few minutes
	- When to exit the loop? Should we use `left < right` or `left <= right` as the while loop condition?
	- How to initialize the boundary variable `left` and `right`?
	- How to update the boundary? How to choose the appropriate combination from `left= mid`, `left = mid + 1`, and `rigth = mid`, `right = mid - 1`
- Another misunderstanding of binary search is that people often think this technique could only be used in simple scenario like given a sorted array, find a specific value in it => Can be applied to much more complicated situations.
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

## Idea
- Most of the time, the problem is not just find a target number in a sorted array. In this case the sorted array is a **search space** and target number is the **search target**. In some problem the **search space** and **search target** is not so readily available. => Have to correctly find and declare the **search space** and **search target** for binary search to work.
- *When can we use binary search?* => **If we can discover some kind of monotonicity, e.g if condition(k) is True then condition(k+1) is True** then we can consider binary search

## Example
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

## Resources:
- https://leetcode.com/discuss/study-guide/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems