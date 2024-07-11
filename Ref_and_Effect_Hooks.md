# Ref Hooks
* Refs let a component hold some information that isn't used for rendering, like a DOM node or a timeout ID. Refs are useful when you need to work with non-React systems, such as the built-in browser APIs.
## useRef
* Call useRef at the top level of your component to declare a ref.
```
import { useRef } from 'react';

function MyComponent() {
  const intervalRef = useRef(initialValue);
  const inputRef = useRef(null);
```
* initialValue-the value you want the ref object's current property to be initially. It can be a value of any type. This argument is ignored after the initial reader.
* useRef returns an object with a single property
  * current-initially, it's set to the initialValue you've passed. You can later set it to something else. 
*  Unlike state, ref.current is mutable. However, if it holds an object that's used for rendering (like a piece of your state) then you shouldn't mutate that object.
*  Do not write or read ref.current during rendering, except for initialization. This makes your component's behavior unpredictable.
### Referencing a Value with a Ref
* Call useRef at the top level of your component to declare one or more refs
```
import { useRef } from 'react';

function Stopwatch() {
  const intervalRef = useRef(0);
```
*  Changing a ref doesn't trigger a re-render. This means refs are perfect for storing information that doesn't affect the visual output.
*  To update the value inside the ref, you need to manually change its current property, as seen below.
```
function handleStartClick() {
  const intervalId = setInterval(() => {
    // ...
  }, 1000);
  intervalRef.current = intervalId;
}
```
* Later, you can read the interval ID from the ref to clear the interval
```
function handleStopClick() {
  const intervalId = intervalRef.current;
  clearInterval(intervalId);
}
```
* By using a ref, you ensure that...
  * You can store information between re-renders (unlike regular variables, which reset on each render)
  * Changing it doesn't trigger a re-render (unlike state variables, which trigger a re-render).
  * The information is local to each copy of your component (unlike the variables outside, which are shared)
### Manipulating the DOM with a Ref
* It's particularly common to use a ref to manipulate the DOM. React has built-in support for this.
* First, declare a ref object with an initial value of null:
```
import { useRef } from 'react';

function MyComponent() {
  const inputRef = useRef(null);
```
* Then pass your ref object as the ref attribute to the JSX of the DOM node you want to manipulate.
```
return <input ref={inputRef} />;
```
*  After React creates the DOM node and puts it on screen, React will set the current property of your ref object to that DOM node. Now you can access the <input>'s DOM node and call methods like focus()
```
function handleClick() {
    inputRef.current.focus();
  }
```
*  React will set the current property back to null when the node's removed from the screen.
### Passing a Ref to a Custom Component
* By default, your own components don't expose refs to the DOM nodes inside them. To fix this, wrap a component that you want to get a ref to in a forwardRef.
```
import { forwardRef } from 'react';

const MyInput = forwardRef(({ value, onChange }, ref) => {
  return (
    <input
      value={value}
      onChange={onChange}
      ref={ref}
    />
  );
});

export default MyInput;
```
# Effect Hooks
* Effects let a component connect to and synchronize with external systems. This includes dealing with network, browser DOM, widgets using a different UI library, and other non-React code.
## useEffect
* useEffect is a React Hook that lets you synchronize a component with an external system.
* Call useEffect at the top level of your component to declare an Effect.
```
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [serverUrl, roomId]);
  // ...
}
```
* The main parameter is setup, the function with your Effect's logic. Your setup function may also optionally return a cleanup function. When your component is added to the DOM, React will run your setup function. After every re-render with changed dependencies, React will first run the cleanup function (if you provided it) with the old values, and then run your setup function with the new values.
* An optional parameter is dependencies, the list of all reactive values referenced inside of the setup code. Reactive values include props, state, and all the variables and functions declared directly inside your component body. The list of dependencies must have a constant number of items and be written inline like [dep1, dep2, dep3]. If you omit this argument, your Effect will re-run after every re-render of the component.
* useEffect is a Hook, so you can only call it at the top level of your component or your own Hooks. You can't call it inside loops or conditions.
* If some of your dependencies are objects or functions defined inside the component, there is a risk that they will cause the Effect to re-run more often than needed. To fix this, remove unnecessary object and function dependencies.
* If your Effect is caused by an interaction (like a click), React may run your Effect before the browser paints the updated screen. This ensures that the result of the Effect can be observed by the event system.
* Even if your Effect was caused by an interaction (like a click), React may allow the browser to repaint the screen before processing the state updates inside your Effect.
* Effects only run on the client. They don't run during server rendering.
### Usage: Connecting to an External System
* Some components need to stay connected to the network, some browser API, or a third-party library while they are displayed on the page.
* To connect your component to some external system, call useEffect at the top level of your component.
```
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => {
  	const connection = createConnection(serverUrl, roomId);
    connection.connect();
  	return () => {
      connection.disconnect();
  	};
  }, [serverUrl, roomId]);
  // ...
}
```
* You need to pass two arguments to useEffect
    1. A setup function with setup code that connects to the system. It should return a cleanup function with cleanup code that disconnects from that system.
    2. A list of dependencies including every value from your component used inside of those functions.
* React calls your setup and cleanup functions whenever it's necessary, which may happen multiple times:
    1. Your setup code runs when your component is added to the page (mounts)
    2. After every re-render of your component where the dependencies have changed, your cleanup code runs with the old props and state, and then your setup code runs with the new props and state
    3. Your cleanup code runs one final time after your component is removed from the page (unmounts)
* An Effect allows you to keep your component synchronized with some external system, meaning any code that's not controlled by React, such as:
  * A timer managed with setInterval() and clearInterval()
  * An event subscription using window.addEventListener() and window.removeEventListener()
  * A third-party animation library with an API like animation.start() and animation.reset()
### Fetching Data with Effects
* You can use an Effect to fetch data for your component. Note that if you use a framework, using your framework's data fetching mechanism will be a lot more efficient than writing Effects manually.
* If you want to fetch data from an Effect manually, your code might look like this:
```
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);

  useEffect(() => {
    let ignore = false;
    setBio(null);
    fetchBio(person).then(result => {
      if (!ignore) {
        setBio(result);
      }
    });
    return () => {
      ignore = true;
    };
  }, [person]);

  // ...
```
* Note the ignore variable, which is initialized to false, and is set to true during cleanup. This ensures your code doesn't suffer from "race conditions": network responses may arrive in a different order than you sent them.
* Even if you use async/await syntax, you'll still need to provide a cleanup function
```
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);
  useEffect(() => {
    async function startFetching() {
      setBio(null);
      const result = await fetchBio(person);
      if (!ignore) {
        setBio(result);
      }
    }

    let ignore = false;
    startFetching();
    return () => {
      ignore = true;
    }
  }, [person]);

  return (
    <>
      <select value={person} onChange={e => {
        setPerson(e.target.value);
      }}>
        <option value="Alice">Alice</option>
        <option value="Bob">Bob</option>
        <option value="Taylor">Taylor</option>
      </select>
      <hr />
      <p><i>{bio ?? 'Loading...'}</i></p>
    </>
  );
}

```
