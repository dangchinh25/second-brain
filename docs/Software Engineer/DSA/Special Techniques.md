## Difference Array
- **Overview**: 
	- https://leetcode.com/discuss/study-guide/2166045/line-sweep-algorithms
		- Good article refers to the same technique but with different name

- A very efficient technique when facing problems where we have to perform *range update operations* on an array. Instead of updating all elements within a range for each operation, you update just two elements (or sometimes three, depending on implementation) in a difference array and use prefix sums to compute the resulting array.

**Intuition Behind Difference Array**
- The main idea is to represent the range update in a compact form:
	•	Instead of updating all elements within a range [l, r] of the original array, you update just the start (l) and end (r+1) indices in a separate array (the difference array).
	•	*Later, you can compute the cumulative effect of these updates by taking the prefix sum of the difference array, which restores the original array with all updates applied.*
- This reduces the time complexity of each range update operation from O(n) to O(1), making it especially useful when there are many such operations.

- This problem demonstrate the technique directly
	- Range Addition: https://leetcode.com/problems/range-addition/
```python
def getModifiedArray(self, length: int, updates: List[List[int]]) -> List[int]:
	diff = [0] * (length + 1)
	for start, end, val in updates:
		diff[start] += val
		diff[end+1] -= val
	
	res = [0] * length
	cur_sum = 0
	for i in range(length):
		cur_sum += diff[i]
		res[i] = cur_sum
	
	return res
```

**When to Use the Difference Array Technique**
1. **Multiple Range Update Queries**:
	• If you have a problem where you need to **update ranges frequently** (e.g., increment or decrement values within a subarray), this technique is ideal.
2. **Static Query on Final State**:
	• If the problem requires querying or printing the final state of an array after applying all range updates, this technique is a good fit.
	• For example, you perform range updates on an array, and at the end, you need to know the array’s values.
3. **Increment/Decrement Operations**:
	• This technique is best suited for additive updates. For other types of updates (like multiplication, setting a specific value, etc.), you may need modifications.
4. **Unordered Queries**:
	• When the order of updates doesn’t matter, you can accumulate all updates and apply them in a single pass, making the technique more efficient.

**Practice problems**
- https://leetcode.com/problems/range-addition
- Car Pooling: https://leetcode.com/problems/car-pooling
	- Each trip adds passengers at the startLocation and removes them at the endLocation.
	- This corresponds to a range update problem:
		- Add numPassengers to the range [startLocation, endLocation - 1].
	- Notices that we initiate the difference array with size of *max end time* instead of length of trips => **We want the difference array to contains all the range the operation can happens, which sometimes we have to define ourselves**
```python
def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        max_time = max(t[2] for t in trips) + 1
        passengers = [0] * (max_time +1)

        for val, start, end in trips:
            passengers[start] += val
            passengers[end] -= val

        cur_pass = 0
        for change in passengers:
            cur_pass += change
            if cur_pass > capacity:
                return False
        
        return True
```

- https://leetcode.com/problems/check-if-all-the-integers-in-a-range-are-covered
```python
def isCovered(self, ranges: List[List[int]], left: int, right: int) -> bool:
        max_range = max([r[1] for r in ranges])
        diff = [0] * (max_range + 2)

        for start, end in ranges:
            diff[start] += 1
            diff[end+1] -=1

        cur_sum = 0
        for  i in range(len(diff)):
            cur_sum += diff[i]
            diff[i] = cur_sum
            if i == left:
                if diff[i] > 0:
                    left += 1
                else:
                    return False
            if left > right:
                return True
        
        return False
```

- https://leetcode.com/problems/meeting-rooms-ii
```python
def minMeetingRooms(self, intervals: List[List[int]]) -> int:
	max_time = max([interval[1] for interval in intervals])
	diff = [0] * (max_time + 2)

	for start, end in intervals:
		diff[start] += 1
		diff[end] -= 1
	
	res = 0
	cur_sum = 0
	for val in diff:
		cur_sum += val
		res = max(res, cur_sum)

	return res
```

https://leetcode.com/problems/brightest-position-on-street/description/
- Can also use HashMap to store difference
```python
def brightestPosition(self, lights: List[List[int]]) -> int:
	diff = collections.defaultdict(int)

	for i, dist in lights:
		diff[i-dist] += 1
		diff[i+dist+1] -= 1

	cur_sum = 0
	max_idx = 0
	max_val = 0

	for idx, val in sorted(diff.items()):
		cur_sum += val
		if cur_sum > max_val:
			max_val = cur_sum
			max_idx = idx
	
	return max_idx
```