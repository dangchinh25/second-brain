## Mental Note
- This is just a normal stack where we append and pop from the top of the stack
- The only special thing here is that when we populate the stack, a certain kind of property is maintained (increase, decreasing)
- So when we traverse the data set, if we see an item that break property then we can pop from the stack and do something with the pop and that item.
	- However even if we pop from the stack the item may still break the property if we add it to the stack, so we have to run a *while* loop to pop and process until the item can be safely added to the stack
- After processing all the pop, we can add the item to the stack
- We can think of it like this, for example we need to find the next greater element, when we add item to stack it means that we need to find next greater for that element, and when we pop from stack means that the current item is  the next greater for the element at the top of the stack, but after the pop the current item may also be the next greater for the new top of stack so we have to run a *while* loop. 
	- E.g: [5,4,3,2,1,6], when we process 6, we can see that 6 is the next greater of 1, but it is also a next greater of 2,3,4,5.
- There will be a cases when we cant find element that break the trend so we left we unprocessed item in the stack, we need to handle that depends on the situation

- **In short:**
	- It's a pattern where we see an increasing/decreasing trend.
	- When I see increasing trend, we add to the stack. Then when you see a number that breaks this trend, you pop from the stack and evaluate

## Overview
- Monotonic stacks are generally used for solving questions of the type *next greater element*, *next smaller element*, *previous greater element*, and *previous smaller element*
- There could be 4 types of monotonic stacks
	- **Strictly-Increasing**: every element of the stack is strictly greater than the previous
	- **Non-decreasing**: every element of the stack is greater than or equal to the previous element
	- **Strictly-decreasing**: every element of the stack is strictly smaller than the previous element
	- **Non-increasing**: every element of the stack is smaller than or equal to the previous element

## Template
- We can use the following template to build a stack that keep the monotonous property alive
```python
def buildMonoStack(arr):
  # initialize an empty stack
  stack = [];
  
  # iterate through all the elements in the array
  for i in range(len(arr)):
	while (stack is not empty && stack[-1] `OPERATOR` arr[i]):
		# if the previous condition is satisfied, we pop the top element
		stackTop = stack.pop();
		
		# do something with stackTop here e.g.
		nextGreater[stackTop] = i
  
    if stack.length:
	    # if stack has some elements left
	    # do something with stack top here e.g.
		previousGreater[i] = stack.at(-1)

	# at the ened, we push the current index into the stack
    stack.push(i);
  
  # At all points in time, the stack maintains its monotonic property
```
- We initialize an empty stack at the beginning
- The stack contains the index of items in the array, not the items themselves
- There is an outer *for* loop and inner *while* loop
- At the beginning of the program, the stack is empty, so we don't enter the *while* loop at first
- The earliest we can enter the *while* loop body is during the second iteration of *for* loop. That's when there is at least an item in the stack
- At the end of the *while* loop, the index of the current element is pushed into the stack
- The *OPERATOR* inside the while loop condition decides what type of monotonic stack are we creating

- Time complexity: **O(n)**
- Space complexity: **O(n)** 

