- Must watch: https://www.youtube.com/watch?v=eiC58R16hb8
- Follow up: https://viblo.asia/p/event-loop-trong-javascript-microtask-macrotask-promise-va-cac-cau-hoi-phong-van-pho-bien-GyZJZjrbJjm

![](https://i.imgur.com/QX6v1Ap.jpeg)


- **Components:** ^f7064a
	- Call Stack: Store the current executing code
	- **Task Queue (Macro Task Queue)**
		- Anything that uses the WebAPI (maybe more) (or generally asynchronous tasks, but only *callback-based*, *Promise-based* tasks will be handled by the **Micro Task Queue** ) will be handled by the Task Queue
		- When we use these WebAPI (again maybe more) we essentially just register/offload the processing of these asynchronous tasks to the Browser/Node. When the execution is done, the  callback will be pushed to the Task Queue, which will be picked up by the Event Loop
			- The function that initiates will be added to the **Call Stack**, then it registers the callback to the Browser/Node and then got poped off
		- One thing to note is that after the registration to Browser/Node part, the main thread will move on to the next thing, letting the processing in the background
		- *Macro Task Queue returns control to Event Loop after execution of each Macro Task. So if we have multiple Macro Task, only the first one will get executed and the other will have to wait for the next available iteration of the Event Loop.*
	- **Micro Task Queue**
		- Anything *Promise-based* will stay in this queue
		- Does not return control to the **Event Loop** but instead try to execute everything in 1 go
		- *Micro Task Queue can trigger another Micro Task, which will get prioritized to execute immediately, this can lead to blocking the Event Loop.*
		- Result from ChatGPT: https://chatgpt.com/share/2947b333-4537-49d3-8dd0-9c539da4465b
	- **Event Loop**
		- Continuously fetch from Task Queue and Micro Task Queue and push them to Call Stack if Call Stack is empty
		- Prioritize Micro Task Queue, always try to empty Micro Task Queue First

- *JS is single threaded, which means each thread have its own Event Loop, Call Stack, etc*