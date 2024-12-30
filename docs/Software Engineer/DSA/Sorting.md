- These are common and foundation knowledge that should be memorized

- Merge sort
```python
def sortArray(nums: List[int]) -> List[int]:
	def merge(arr1, arr2):
		l, r = 0, 0
		res = []

		while l < len(arr1) and r < len(arr2):
			if arr1[l] <= arr2[r]:
				res.append(arr1[l])
				l += 1
			else:
				res.append(arr2[r])
				r += 1

		if l < len(arr1):
			res += arr1[l:]

		if r < len(arr2):
			res += arr2[r:]

		return res

	if len(nums) <= 1:
		return nums

	l, r = 0, len(nums) - 1
	mid = l + (r - l) // 2
	l_res = self.sortArray(nums[0:mid+1])
	r_res = self.sortArray(nums[mid+1:])

	return merge(l_res, r_res)
```