
This is a REALLY good post: https://leetcode.com/tag/two-pointers/discuss/1122776/Summary-of-Sliding-Window-Patterns-for-Subarray-Substring
https://leetcode.com/tag/sliding-window/discuss/490184/Sliding-Window-Understanding-the-pattern.-Resources

- Intuition/Idea
	- We have a "window" of 2 pointers, `left` and `right`, and we keep increasing the right pointer
		- If the element at the right pointer makes the window not valid, we keep moving the left pointer to shrink the window until it becomes valid again
		- Then, we update the global min/max with the result from the valid window
		- To check if it is valid, we need to store the "state" of the window (ex: frequency of letters, num of distinct elements, etc)
	- ==It's important that we be able to define *the state* of the window, this state need to be valid at all time and move the window accordingly when it becomes invalid till it becomes valid again== 
- Template:
```
for(right = 0; right < n; right++):
    update window with element at right pointer
    while (condition not valid):
        remove element at left pointer from window, move left pointer to the right
    update global max
```

- Pattern 1: Length of Substring/Subarray questions
	- [3. Longest substring without repeating characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
	- [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
	- [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/solution/)
- Pattern 2: Max/Min Length Sliding Window questions
	- May look different from the previous pattern, but are just worded differently, the main idea is the same
	- [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)
	- [904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/)
	- [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
- Pattern 3: Number of Subarrays questions
	-  Before, we were finding the *min/max length* of subarray, now we want the *number of subarrays*
	- [930. Binary Subarrays with Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)
	- https://leetcode.com/problems/subarray-product-less-than-k/
	- **For these type of questions, we usually get ask to return the number of valid subarrays, we can calculate these easily by `res += right - left + 1`, the reason being by introducing a new element at `nums[right]`, we are adding that many new valid subarrays. E.g: [1,2] has 3 valid subarrays and [1,2,3] has 6 
- Pattern 4: Prefixed Sliding Windows
	- TBD
	- [930. Binary Subarrays with Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)
	- [992. Subarrays with K different integers](https://leetcode.com/problems/subarrays-with-k-different-integers/)
	- [Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/)
	- [Subarrays containing all three characters](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters)

- **When it fails**
	- If knowing one element at the edges of the window does not tell you how to update the state of the window, or whether it becomes valid
	- If adding one element could either increase or decrease the window' state
	- If it is hard to check whether adding or remove from only one end at a time would ever make it valid
	- [General summary of what kind of problem can/ cannot solved by Two Pointers](https://leetcode.com/problems/subarray-sum-equals-k/solutions/301242/General-summary-of-what-kind-of-problem-can-cannot-solved-by-Two-Pointers/)