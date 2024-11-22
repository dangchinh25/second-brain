# Action Creators
- In practice, well-written Redux apps don't actually write those action objecs inline when we dispatch them (like `dispatch({type: 'example', payload: payload})`). Instead, we use **action creator** functions
- An **action creators** is a function that creates and returns an action object. We use these so we don't have to write the action object by hand every time

```jsx
const exampleActionCreator = text => {
  return {
    type: 'example',
    payload: text
  }
}

store.dispatch(exampleActionCreator("example text"))
```

## Action Creators with Thunk Function

- Ref: [](https://www.notion.so/Async-Logic-and-Data-Fetching-933b1ec01a514b8a9fb35b767aedfd1c?pvs=21)[https://www.notion.so/Async-Logic-and-Data-Fetching-933b1ec01a514b8a9fb35b767aedfd1c#aa40f4153f854d37bd23841e34d60079](https://www.notion.so/Async-Logic-and-Data-Fetching-933b1ec01a514b8a9fb35b767aedfd1c#aa40f4153f854d37bd23841e34d60079)
- Sometimes we need to use variables outside of the thunk function as arguments for the side effect call or the `dispatch` function, but the thunk function can only accepts `dispatch` and `getState` ⇒ **Write a synchronous outer function that receives the parameter and then return the async thunk function**

```jsx
// Write a synchronous outer function that receives the `text` parameter:
export function outerFunction(arg) {
  // And then creates and returns the async thunk function:
  return async function thunkFunction(dispatch, getState) {
    // ✅ Now we can use the arg (outsider variables)
    ...
    dispatch(exampleActionCreator(arg))
  }
}

// and then we can just call and dispatch the return value of the outerFunction
store.dispatch(outerFunction(someVariables))
```