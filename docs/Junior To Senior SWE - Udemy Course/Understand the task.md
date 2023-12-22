- The first task in completing any task is understanding it. However, it is quite common among people underestimate this part and get to coding too soon.
- There are 3 questions that we need to go through
	1. Why invest time in having a deep understanding of the task before coding
	2. How to tell you understand the task
	3. How to remember what you understand

## Why invest time in having a deep understanding of the task
- Less bugs
	- The better you understand the task, the less probable it is that you'll create bugs
	- The more you deepen your understanding of the task, the more possible you'll be able to find and avoid bugs.
- Minimize changes after coding begins
	- If you're already coding, you miss out on some effects, one part of your task has on another part if it, you'll need to come back to the part you already finished and adjust for that effect.
	- If you find that dependency while you read the specs, you'll able to account for that before you even start coding. And when you start coding, it will be compatible with the future subtask as well.
- Better time management
	- You can't tell how long the task will take if you don't understand it.
	- You know which part of the task you already finished, what's left and if you're more or less on schedule

## How to tell you understand the task
- You understand the task when you have a clear mental image of it. That is, you can see it working in your mind's eye and then just translating that to code or instruction to give it to someone else.
- Another way is to reflecting that understadning to the person that assign you the task is the best way to make sure you understand the task correctly and maybe also get some additionional insight.
- A good explaination of a task basically consists of answers to the next <b>four questions</b>
	1. <b>What does the user see</b>
		- This seem a little bit Client-facing, but in general we can understand it as what is the result of our task (this can be the UI component, or maybe the return of an API call, etc)
		- This may also includes thinking about what kind data is needed, how it is retrieved and calculated
	 2. <b>How does each part behave</b>
		- What state does it have
		- When does it change
	3. <b> What interaction is there with other components </b>
		- How does it change other components 
		- How is it changed by other components
	4. <b> What are the edge cases, and how to handle them </b>
		- Thinking about edge cases really has a lot to do with planning tests
		- We need to find and address all the edge cases that we can think of.

## How to remember what you understand
- Make sure to take note in whatever form that you prefer
- <b> NEVER </b> relies on your memory
- Several methods
	- Write notes directly near the requirement specification
	- Flowchart to understand the flow of logic
	- Make sure to include edge cases and how to handle them