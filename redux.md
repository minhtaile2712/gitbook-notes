# Redux

### Terminology

#### Actions

An **action** is a plain JavaScript object that has a `type` field. **You can think of an action as an event that describes something that happened in the application**.

```
{
  type: "inc',
  payload: 1
}
```

#### Action Creators

An **action creator** is a function that creates and returns an **action** object.

```
function addTodo() {
  return {
    type: 'todos/todoAdded',
    payload: text
  }
}
```

#### Reducer

A **reducer** is a function that receives the current **`state`** and an **`action`** object, decides how to update the state if necessary, and returns the **new state**.

```
function reducer(state = { value: 0 }, action) {
  switch (action.type) {
    case 'inc':
      return {value: state.value + 1}}
    case 'dec':
      return {value: state.value - 1}
    default:
      return state;
  }
}
```

#### Store

The current Redux application state lives in an object called the **store**.

The store is created by passing in a **reducer** and has a method called **`getState`** that returns the current state value.

```
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}

```

**Dispatch**

The only way to update the state is to call ** `store.dispatch()` ** and pass in an **action** object

```
store.dispatch({ type: 'counter/increment' })

console.log(store.getState())
// {value: 1}

// store.dispatch(actionObject)
```

We typically call **action creators** to dispatch the right action:

```
const increment = () => {
  return {
    type: 'counter/increment'
  }
}

store.dispatch(increment())

console.log(store.getState())
// {value: 2}
```

**Selectors**

**Selectors** are functions that know how to extract specific pieces of information from a store state value**.**

```
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 2
```

```
import { createStore } from 'redux'

let store = createStore(counterReducer)

store.subscribe(() => console.log(store.getState()))

store.dispatch({ type: 'inc' })
// {value: 1}

```

### Redux Toolkit

```
import { createSlice, configureStore } from '@reduxjs/toolkit'

const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    incremented: state => {
      state.value += 1
    },
    decremented: state => {
      state.value -= 1
    }
  }
})

export const { incremented, decremented } = counterSlice.actions

const store = configureStore({
  reducer: counterSlice.reducer
})

store.subscribe(() => cincrementedonsole.log(store.getState()))

store.dispatch(incremented())
// {value: 1}

```
