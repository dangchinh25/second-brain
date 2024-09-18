**1. Swap all element equal to some value/criteria to the end of the array**
- Have a pointer to hold the first occurence of the value, if the pointer at any given time points to an element with value different from the target value, we can leave it be cause the element will be swap with itself so nothing happens
- [This problem](https://leetcode.com/problems/move-zeroes/description/) is a perfect example
```python
def removeElement(self, nums: List[int], val: int) -> int:    
	firstOccurence = 0
	for i in range(len(nums)):
		if nums[i] != val:
			nums[firstOccurence], nums[i] = nums[i], nums[firstOccurence]
			firstOccurence+=1
	
	return firstOccurence
```

**2. Boyer-Moore Voting algorithm**
- https://leetcode.com/problems/majority-element/solutions/4475857/python-boyer-moore-voting-algorithm-fully-explained-space-o-1

**2. Check palindrome using middle out instead of 2 pointer from 2 side in may reduce the overall time complexity**

**3. When need to count occurence of each element in a string or array and store them in a hashmap => Use collections.Counter**

**4. If need to store the active set of element for existence checking  (e.g in sliding window problems), use a Set instead of hashmap/Couter if dont care about number of appearance**

**5. When iterate an array, can check 2 value at the same time with nums[i] and nums[i-1]/nums[i+1] or both**

**6. When work with array where we need some kind of combination but the array contains duplicate value and the result doesn't allow duplicating combination, we can sort the array to eliminate the duplicating call during iteration => During iteration, at each index we only care about all indexes at the right side of the current index, and if the element at the adjacent index is the same we can skip it**  ^73797d

**7. When working with tree, if there is a need to count the number of possible node, keep in mind that each tree layer has 2x the number of node compare to the previous layer**

**8. When working with graph, if asked about number of connected node or sth that does not necessarily require traversal, maybe try counting indegree-outdegree**

**9. When working with problem that requires validity of parentheses or stuff like, one trick is to use a stack and try to add opening and when meet a closing we can either pop the top of the stack if the top is an opening or add if otherwise. We can also consider save the index of the parentheses when adding to the stack*

**10. Another trick when dealing with validity of parenthesis is to count the number of orphan. We will traverse the string on 2 pass, 1 from left to right to count orphan closing and then from right to left to count orphan opening.**
	- Ref: https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid/solutions/1646594/left-to-right-and-right-to-left/?orderBy=most_votes

**11. When using sliding window, not always need to use left, right with while loop. We can init the left = 0 and increment the right in a for loop, and inside the for loop we do sth to increase the left**
1.  **Permutation**: can be thought of number of ways to order some input.
    -   Example: permutations of ABCD, taken 3 at a time (24 variants): ABC, ACB, BAC, BCA, ...
2.  **Combnation**: can be thought as the number of ways of selecting from some input.
    -   Example: combination of ABCD, taken 3 at a time (4 variants): ABC, ABD, ACD, and BCD.
3.  **Subset**: can be thought as a selection of objects form the original set.
    -   Example: subset of ABCD: 'A', 'B', 'C', 'D,' 'A,B' , 'A,C', 'A,D', 'B,C', 'B,D', 'C,D', 'A,B,C', ...

**12. When dealing with string or array, sometimes it is helpful and make life easier by using string/array slicing/concatenating 
	- For example with [this problem](https://leetcode.com/problems/valid-palindrome-ii/description/) , we can simulate the 'delete character' action by slicing the string and combine it without the specify character
```python
def validPalindrome(s: str) -> bool:
	l, r = 0, len(s) - 1
	
	while l <= r:
		if s[l] != s[r]:
			s1 = s[:l] + s[l + 1 :]
			s2 = s[:r] + s[r + 1 :]
			
			return s1 == s1[::-1] or s2 == s2[::-1]
	
		l += 1
		
		r -= 1
	
	return True
```
