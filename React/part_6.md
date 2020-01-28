# Flux
Facebook developed the Flux- architecture to make state management easier. In Flux, the state is separated completely from the React-components into its own stores. State in the store is not changed directly, but with different actions. When an action changes the state of the store, the views are rerendered.

# Redux
As in Flux, in Redux the state is also stored in a store. The whole state of the application is stored into one JavaScript-object in the store. The state of the store is changed with actions. Actions are objects, which have at least a field determining the type of the action. If there is data involved with the action, other fields can be declared as needed.
The impact of the action to the state of the application is defined using a reducer. In practice, a reducer is a function which is given the current state and an action as parameters. It returns a new state.
Reducer is never supposed to be called directly from the applications code. Reducer is only given as a parameter to the createStore-function which creates the store. The store then uses the reducer to handle actions, which are dispatched or 'sent' to the store with its **dispatch-method**. You can find out the state of the store using the method **getState**.
For example the following code: 
```JavaScript
const store = createStore(counterReducer)
console.log(store.getState())
store.dispatch({type: 'INCREMENT'})
store.dispatch({type: 'INCREMENT'})
store.dispatch({type: 'INCREMENT'})
console.log(store.getState())
store.dispatch({type: 'ZERO'})
store.dispatch({type: 'DECREMENT'})
console.log(store.getState())
```
would print the following to the console:
```
0
3
-1
```

The third important method the store has is **subscribe**, which is used to create recall functions the store calls when its state is changed. If, for example, we would add the following function to subscribe, every change in the store would be printed to the console:
```JavaScript
store.subscribe(() => {
  const storeNow = store.getState()
  console.log(storeNow)
})
```

When the state in the store is changed, React is not able to automatically rerender the application. Thus we have registered a function renderApp, which renders the whole app, to listen for changes in the store with the store.subscribe method. **Note that we have to immediately call the renderApp method. Without the call the first rendering of the app would never happen.**

Reducers must be *pure functions*. Pure functions are such, that they do not cause any side effects and they must always return the same response when called with the same parameters. state.push(action.data) changes the state of the state-object. This is not allowed. The problem is easily solved by using the concat method, which creates a new array, which contains all the elements of the old array and the new element.
A reducer state must be composed of immutable objects. If there is a change in the state, the old object is not changed, but it is replaced with a new, changed, object. 

For testing, we'll add the library deep-freeze, which can be used to ensure that the reducer has been correctly defined as an immutable function. The deepFreeze(state) command ensures that the reducer does not change the state of the store given to it as a parameter. If the reducer uses the push command to manipulate the state, the test will not pass.

It is actually not necessary for React-components to know the Redux action types and forms. We can separate creating actions into their own functions. Functions that create actions are called **action creators**.

Unlike in the React code we did without Redux, the event handler for changing the state of the app (which now lives in Redux) has been moved away from the App to a child component. The logic for changing the state in Redux is still neatly separated from the whole React part of the application. 

Note, responsible for rendering a single note, is very simple, and is not aware that the event handler it gets as props dispatches an action. These kind of components are called **presentational** in React terminology.
Notes, on the other hand, is a **container component**, as it contains some application logic: it defines what the event handlers of the Note components do and coordinates the configuration of presentational components, that is, the Notes.

Again for clarity:  
**Reducer**
```JavaScript
const filterReducer = (state = 'ALL', action) => {
  switch (action.type) {
    case 'SET_FILTER':
      return action.filter
    default:
      return state
  }
}
```
**Action**
```JavaScript
{
  type: 'SET_FILTER',
  filter: 'IMPORTANT'
}
```
**Action Creator**
```JavaScript
export const filterChange = filter => {
  return {
    type: 'SET_FILTER',
    filter,
  }
}
```

## Combined reducers
We can create the actual reducer for our application by combining two existing reducers with the combineReducers function. The state of the store defined by the reducer is an object. The value of the individual properties is defined by the corresponding reducer, which does not have to deal with the other properties of the state.
The combined reducer works in such a way that every action gets handled in every part of the combined reducer. Typically only one reducer is interested in any given action, but there are situations where multiple reducers change their respective parts of the state based on the same action. (So every filter gets executed despite the type of the action)

## Connect
Is a function provided by the *React Redux* library.
In order to use the connect function we have to define our application as the child of the **Provider** component that is provided by the React Redux library. Additionally, the Provider component must receive the Redux store of the application as its store attribute.
```JavaScript
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

The connect function can be used for transforming "regular" React components so that the state of the Redux store can be "mapped" into the component's props.
```JavaScript
const ConnectedComponent = connect()(Component)
export default ConnectedComponent
```

The connect function accepts a so-called **mapStateToProps** function as its first parameter. The function can be used for defining the props of the connected component that are based on the state of the Redux store.
```JavaScript
const Notes = (props) => {
  // ...
}

const mapStateToProps = (state) => {
  return {
    notes: state.notes,
    filter: state.filter,
  }
}

const ConnectedNotes = connect(mapStateToProps)(Notes)

export default ConnectedNotes
```
The Notes component can access the state of the store directly, e.g. through props.notes that contains the list of notes. Contrast this to the previous props.store.getState().notes implementation that accessed the notes directly from the store. Similarly, props.filter references the value of the filter.

The second parameter of the connect function can be used for defining **mapDispatchToProps** which is a group of action creator functions passed to the connected component as props.
```JavaScript
const mapDispatchToProps = {
  toggleImportanceOf,
}

const ConnectedNotes = connect(
  mapStateToProps, mapDispatchToProps
  )(Notes)
```
Now the component can directly dispatch the action defined by the toggleImportanceOf action creator by calling the function through its props. There is no need to call the dispatch function separately since connect has already modified the toggleImportanceOf action creator into a form that contains the dispatch.

Alternatively, you can pass a function definition as the second parameter to connect. In this alternative definition, mapDispatchToProps is a function that connect will invoke by passing it the dispatch-function as its first parameter and the component's props as a second.
```JavaScript
const mapDispatchToProps = (dispatch, ownProps) => {
  return {
    remove: () => {
      dispatch(removeById(ownProps.id));
    }
  };
};
```

Previously mapStateToProps was simply used for selecting pieces of state from the store, but in this case we are also using the notesToShow function to map the state into the desired filtered list of notes. The new version of notesToShow receives the store's state in its entirety, and selects an appropriate piece of the store that is passed to the component. Functions like this are called **selectors**.

--> presentational, container and high order components