
## setState
- Good read: https://chatgpt.com/share/79fab4bb-a4a9-4a58-8e55-51d216369c16
- React's state update is asynchronous, most of the time it is fine but sometimes it creates some headache due its asynchronous + batching behavior
	- https://dev.to/bytebodger/updating-react-state-inside-loops-2dbf

- *setState* generally has 2 phases: *State update* & *Re-render*
- When we call *setState* -> Schedule *State Update* in the [[Event Loop#^f7064a | Task Queue]] and mark the component as *dirty*
	- All these queued *State Update* will be batched and execute in 1 [[Event Loop#^f7064a | Event Loop]]
	- After executing the batched *State Update*, it will schedule the *Re-render* (also in the [[Event Loop#^f7064a | Task Queue]]) to be picked up by the next available iteration of the [[Event Loop#^f7064a | Event Loop]]

- *setState* has 2 form: *functional* & *non-functional*
- There's no change in scheduling mechanism between 2 forms
- When using *functional form* (updater function), it guarantees to receive the most updated state (event with batching) as the state will be applied sequentially
```javascript
setState(prevState => ({ count: prevState.count + 1 })); // First call
setState(prevState => ({ count: prevState.count + 1 })); // Second call
```
- When both of these `setState` calls are batched:
    1. React doesn’t execute them immediately.
    2. It first processes the first updater function:
        - `prevState.count` is the current `count` value (say, `count = 0`), so it calculates `count + 1 = 1` and updates the state to `{ count: 1 }`.
    3. It then processes the second updater function:
        - **This time, `prevState.count` is 1** (the result of the first updater), so it calculates `count + 1 = 2` and updates the state to `{ count: 2 }`.
- At the end of the batch, React will have updated the state correctly, and only **one re-render** will happen with the new state `{ count: 2 }`.

- When you use *non-functional* `setState` calls (e.g., `setState({ count: 1 })`), React simply overwrites the state with the new object. But, if multiple `setState` calls occur in the same event, React can **batch** these updates as well. In this case, the last update wins, and intermediate updates may be lost if they occur before the batch is processed.
```javascript
setState({ count: 1 }); // Direct update 
setState({ count: 2 }); // Overwrites previous update
```
- In this case, React will batch these, and only the last state `{ count: 2 }` is applied, resulting in one re-render with `count = 2`.