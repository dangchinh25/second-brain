
- Ref: [https://redux.js.org/tutorials/fundamentals/part-6-async-logic](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)
- By itself, a Redux store doesn't know anything about async logic. It only knows how to synchronously dispatch actions, update the state by calling the root reducer function, and notify the UI that something has changed. Any asynchronicity has to happen outside the store.
- Redux reducers must never contain "side effects"
    - Logging a value to the console
    - Saving a file
    - Setting an async timer
    - Making an AJAX HTTP request
    - Modifying some state that exists outside of a function, or mutating arguments to a function
    - Generating random numbers or unique random IDs (such as `Math.random()` or `Date.now()`)
- Redux middleware can do _anything_ when it sees a dispatched action: log something, modify the action, delay the action, make an async call, and more. Also, since middleware form a pipeline around the real `store.dispatch` function, this also means that we could actually pass something that _isn't_ a plain action object to `dispatch`, as long as a middleware intercepts that value and doesn't let it reach the reducers.
- Middleware also have access to `dispatch` and `getState`. That means you could write some async logic in a middleware, and still have the ability to interact with the Redux store by dispatching actions.

# Writing an Async Function Middleware

[[Middleware]]
- It would be nice if we had a way to write any async (side effect) logic ahead of time, like a normal function, separate from the middleware itself, and still have access to dispatch and getState so that we can interact with the store

⇒ **Write a middleware that let us pass a function `dispatch` instead of an object.** The middleware check to see if the "action" is actually a function instead, and if it's a function, call the function right away. that would let us write async logic in separate functions. outside the middleware definition

```jsx
const asyncFunctionMiddleware = storeAPI => next => action => {
  // If the "action" is actually a function instead...
  if (typeof action === 'function') {
    // then call the function and pass `dispatch` and `getState` as arguments
    return action(storeAPI.dispatch, storeAPI.getState)
  }

  // Otherwise, it's a normal action - send it onwards
  return next(action)
}

const middlewareEnhancer = applyMiddleware(asyncFunctionMiddleware)
const store = createStore(rootReducer, middlewareEnhancer)

// Write a function that has `dispatch` and `getState` as arguments
const fetchSomeData = (dispatch, getState) => {
  // Make an async HTTP request
  client.get('todos').then(todos => {
    // Dispatch an action with the todos we received
    dispatch({ type: 'todos/todosLoaded', payload: todos })
    // Check the updated store state after dispatching
    const allTodos = getState().todos
    console.log('Number of todos after loading: ', allTodos.length)
  })
}

// Pass the _function_ we wrote to `dispatch`
store.dispatch(fetchSomeData)
// logs: 'Number of todos after loading: ###'
```

- T**his "async function middleware" let us pass a _function_ to `dispatch`!** Inside that function, we were able to write some async logic (an HTTP request), then dispatch a normal action object when the request completed.

# Using Redux Thunk Middleware

- Official version of "async function middleware" ⇒ **Redux thunk**
- The thunk middleware allows us to write functions that get `dispatch` and `getState` as arguments. The thunk functions can have any async logic we want inside and that logic can dispatch actions and read the store state as needed
- Step to use:

1. `npm install redux-thunk`
2. Update the Redux store

```jsx
import { createStore, applyMiddleware } from 'redux'
import thunkMiddleware from 'redux-thunk'
import { composeWithDevTools } from 'redux-devtools-extension'
import rootReducer from './reducer'

const composedEnhancer = composeWithDevTools(applyMiddleware(thunkMiddleware))

// The store now has the ability to accept thunk functions in `dispatch`
const store = createStore(rootReducer, composedEnhancer)
export default store
```

1. Write a thunk function

```jsx
export async function exampleThunkFunction(dispatch, getState) {
  ...
  dispatch({"an actual action object goes here"})
	...
}
```

1. Dispatch the thunk function like we usually do with normal action object

```jsx
store.dispatch(exampleThunkFunction)
```

- **NOTES: Since dispatching an action immediately updates the store, we can also call `getState` in the thunk to read the updated state value after we dispatch.**

## Action Creator with Thunk Function

- Ref: [](https://www.notion.so/Standard-Patterns-08d580e1402b414b8976a616d3e04c50?pvs=21)[https://www.notion.so/Standard-Patterns-08d580e1402b414b8976a616d3e04c50#f7110601b81444a6a80a6a4f9444f7b9](https://www.notion.so/Standard-Patterns-08d580e1402b414b8976a616d3e04c50#f7110601b81444a6a80a6a4f9444f7b9)

## Returning Values from Thunks

- By default, `store.dispatch(action)` returns the actual action object. Middleware can override the return value being passed back from `dispatch`, and substitute whatever other value they want to return.
- The thunk middleware does this, by returning whatever the called thunk function returns. The most common use case for this is returning a promise from a thunk. This allows the code that dispatched the thunk to wait on the promise to know that the thunk's async work is complete. This is often used by components to coordinate additional work

```jsx
const onAddTodoClicked = async () => {
  await dispatch(saveTodo(todoText))
  setTodoText('')
}
```

## Async Logic in Thunks

- Error handling here can be trickier than most people think. If you chain `resPromise.then(dispatchFulfilled).catch(dispatchRejected)` together, you may end up dispatching a "rejected" action if some non-network error occurs during the process of handling the "fulfilled" action. It's better to use the second argument of `.then()` to ensure you only handle errors related to the request itself

```jsx
function fetchData(someValue) {
  return (dispatch, getState) => {
    dispatch(requestStarted())

    myAjaxLib.post('/someEndpoint', { data: someValue }).then(
      response => dispatch(requestSucceeded(response.data)),
      error => dispatch(requestFailed(error.message))
    )
  }
}
```

- With `async/await`, this can be even trickier, because of how `try/catch` logic is usually organized. In order to ensure that the `catch` block _only_ handles errors from the network level, it may be necessary to reorganize the logic so that the thunk returns early if there's an error, and "fulfilled" action only happens at the end.

```jsx
function fetchData(someValue) {
  return async (dispatch, getState) => {
    dispatch(requestStarted())

    // Have to declare the response variable outside the try block
    let response

    try {
      response = await myAjaxLib.post('/someEndpoint', { data: someValue })
    } catch (error) {
      // Ensure we only catch network errors
      dispatch(requestFailed(error.message))
      // Bail out early on failure
      return
    }

    // We now have the result and there's no error. Dispatch "fulfilled".
    dispatch(requestSucceeded(response.data))
  }
}
```