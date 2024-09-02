- Often, when we understand a task, an idea of how to solve it pops into our minds and the way from there to going ahead and coding.
- However, there are many things one should take into consideration when deciding on a solution, so going with the first solution that you had in mind without critically examining it will very often get you to either a solution that is not optimal or a solution that just doesn't work.

## Why spend time on planning a solution
- No reinventing the wheel.
	- If we won't be aware of what we already have in our project abd what is available to us, we might end up writting a solution that already exists and is available to use.
- Write compatible code
- Not waste time on implementing suboptimal solutions
- Not find later that the solution doesn't work

## Solve
- Define technically
	- What are the scope?
		- What already exists / What are your task boundaries
		- What needs to be done
	 - What are the context?
		 - Parts of the system that interact with your code
		 - Code you can learn from or use
			 - Some functions similar to the one you're trying to write, which you can get inspiration from
			 - Maybe someone has already done what you're doing but differently, and reading his code or talking him could help
		 - Coding standard
 - Find optimal solution
	 - What are some relevant solutions?
	 - Which solutions is the best?
		 - Finding all the criteria
			 - Cost (time + money)
			 - Performance
			 - Forward compability => Make future tasks easier
			 - Simplicity
			 - Security
			 - Completeness => Handle all cases
			 - Conventions
			 - ETC
		 - Compare all the solutions by the criteria
> We can also apply this to other people, when we discuss about a problem, we can help them to get to the correct solution by asking them these questions: What are the different ways to solve the problem, which criterias are relevant to this problem and which way does he/she think is the best based on these criterias?
- How can we be certain it will actually work for some solutions?
	- Use a POC, basically means you find the problematic part in your solution and do the minimum needed in order to make sure you know how to handle it
	- Might want to consider building a test for it and test with different input to see if the output matches what we expect

## Documents
- You want to make sure you write down all you know about the tasks and solution just so that you won't forget it by the time you need it.
- [[Understand the task#How to remember what you understand]] 
- Can also you pseudo code