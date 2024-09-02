- Both the stack and heap are parts of memory available to your code to use at runtime, but they are structured in different ways.

# Stack
> The stack memory follows the same data structure as [[Stack]]

- All data stored on the stack must have a known, fixed size. Data with an unknown size at compile time or a size that might change must be stored on the [[Memory#Heap | Heap]] instead.

# Heap
> The heap memory has nothing to do with [[Heap]] data structure.

- When you put data on the heap, you request a certain amount of space. The memory allocator finds an empty spot in the heap that is big enough, marks it as being in use, and returns a pointer, which is the address of that location => `allocation`. 
- Because the pointer to the heap is a known, fixed size, you can store the pointer on the stack, but when you want the actual data, you must follow the pointer. 
- Think of being seated a restaurant, when you enter, you state the number of people in your group, and the host finds an empty table that fits everyone and leads you there. If someone in your group come late, they can ask where you've been seated to find you. 

# Usage
- Pushing to the stack is faster than allocating on the heap because the allocator never has to search for a place to store new data, that location is always at the top of the stack.
- Allocating space on the heap requires more work because the allocator must first find a big enough space to hold the data and then perform bookkeeping to prepare for the next allocation.
  - Accessing the data in the heap is slower than accessing data on the stack because you have to follow a pointer to get there.
  - Processors are better if they don't have to jump around too much in memory. 
	  - Consider a server at a restaurant taking orders from many tables, it's most efficient to get all the orders at one table before moving on to the next table.
	  - Similarly, a processor can do its job better if it works on data that's closer to other data (as it is on the stack) rather than farther away (as it can be on the heap)
