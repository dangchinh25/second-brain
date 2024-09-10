- Must watch: https://www.youtube.com/watch?v=Xs1EMmBLpn4

- `Promise.then(() => {})` creates a PromiseReaction object, which waits for the `resolve` 
	-> Upon receive `resolve`, it push the whole function inside `.then` into the [[Event Loop#^f7064a | Micro Task Queue]] 
- By using `Promise.then`, we can ensure that everything chained after it and inside the callback function will be handled by the [[Event Loop#^f7064a | Micro Task Queue]] 
	- This is the same if we use *async/await* (which is just some syntactic sugar)
	- `Promise.resolve().then(someFn)` = `await someFn()`, these 2 are similar as anything after `await` will be push to the queue (including another `await`)