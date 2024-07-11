# Built-In React Hooks
* Hooks let you use different React features from your components. You can use built-in hooks or combine them.

## State Hooks
* State lets a component "remember" information, such as user input. For instance, a form component can use state to store the input value, while an image gallery can use state to store the selected image index.
* To add state to a components, use one of the following Hooks:
  * useState declares a state variable that you can update directly.
  * useReducer declares a state variable with the update logic inside a reducer function.
### useState
* Call useState at the top level of your component to declare a state variable.
  * import { useState } from 'react';
* The convention is to name state variables like [something, setSomething]
  * const [name, setName] = useState('Taylor')
* initialState-parameter, the value you want the state to be initially.
* useState returns an array with exactly two values:
    1. The current state. During the first render, it'll match the initialState you passed
    2. The set function that lets you update the state to a different value and trigger a re-render
* useState is a Hook, so you can only call it at the top level of your component or your own Hooks. You can't call it inside loops or conditions.
* The set function returned by useState lets you update the state to a different value and trigger a re-render.
* nextState-the value you want the state to be.
  * const [name, setName] = useState('Edward');
  * function handleClick() {
    * setName('Taylor');
    * setAge(a => a + 1);
* The set function only updates the state variable for the next render
* Calling the set function during rendering is only allowed from within the currently rendering component.
* To update state based on previous state, you may use an updater function. This takes the pending state and calculates the next state from it, as seen below.
* function handleClick(){
  *  setAge(a => a + 1); // setAge(42 => 43)
  *  setAge(a => a + 1); // setAge(43 => 44)
  *  setAge(a => a + 1); // setAge(44 => 45)
*  }
*  In React, state is considered read-only, so you should replace it rather than mutating existing objects.
*  setForm({
  * ...form,
  * firstName: 'Taylor'
* });
* You can reset a component's state by passing a different key to a component. In the below example, the Reset button changes the version state variable, which we pass as a key to the Form. When the key changes, React re-creates the Form component from scratch, so its state gets reset.
* export default function App() {
  * const [version, setVersion] = useState(0);

  * function handleReset() {
    * setVersion(version + 1);
  * }

  * return (
    * <>
      * <button onClick={handleReset}>Reset</button>
      * <Form key={version} />
    * </>
  * );
* }

* function Form() {
  * const [name, setName] = useState('Taylor');

  * return (
    * <>
      * <input
        * value={name}
        * onChange={e => setName(e.target.value)}
      * />
      * <p>Hello, {name}.</p>
    * </>
  * );
* }
* To store and use information from previous renders, you can call a set function while your component is rendering.
* export default function CountLabel({ count }) {
  * return <h1>{count}</h1>
* }
* Remember, states behaves like a snapshot; updating state requests another render with the new state. If you need to use the next state, you can save it in a variable before passing it to the set function.
* const nextCount = count + 1;
* setCount(nextCount);

### useReducer
* useReducer is a React Hook that lets you add a reducer to your component.
* Call useReducer at the top level of your component to manage its state with a reducer
* import { useReducer } from 'react';
* function reducer(state, action) {
  * // ...
* }
* function MyComponent() {
  * const [state, dispatch] = useReducer(reducer, { age: 42 });
* There are two parameters of useReducer
  * reducer-the reducer function that specifies how the state gets updated. It must be pure, should take the state and action as arguments, and should return the next state.
  * initialArg-the value from which the initial state is calculated.
* useReducer returns an array with two values...
  1. The current state. During the first render, it's set to init(initialArg) or initialArg
  2. The dispatch function that lets you update the state to a different value and trigger a re-render.
* The dispatch function returned by userReducer lets you update the state to a different value and trigger a re-render.
* const [state, dispatch] = useReducer(reducer, { age: 42 });

* function handleClick() {
  * dispatch({ type: 'incremented_age' });
* useReducer is very similar to useState, but it lets you move the state update logic from event handlers into a single function outside of your component.
* By convention, it is common to use a switch startement, and for each case in it, calculate and return some next state.
* function reducer(state, action) {
  * switch (action.type) {
    * case 'incremented_age': {
      * return {
        * name: state.name,
        * age: state.age + 1
     * };
    * }
    * case 'changed_name': {
      * return {
        * name: action.nextName,
        * age: state.age
      * };
    * }
  * }
  * throw Error('Unknown action: ' + action.type);
* }
