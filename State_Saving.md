# Preserving and Resetting State
* State is tied to a position in the render tree.
* React builds render trees for the component structure in your UI.
* React associates each piece of state it's holding with the correct component by where that component sits in the render tree.
* In React, each component on the screen has fully isolated state. For instance, if you render two Counter components side by side, each of them gets their own, independent, score and hover states. When one counter is updated, only the state for that component is updated.
* React will keep the state around for as long as you render the same component at the same position in the tree.
* When you stop rendering a component, its state disappears completely. That's because when React removes a component, it destroys its state.
* If you, for example, click a checkbox that says "Render the second counter", a second Counter and its state are initialized from scratch (score=0) and added to the DOM.
* React preserves a component's state for as long as it's being rendered at its position in the UI tree. If it gets removed, or a different component gets rendered at the same position, React discards its state.
## Same Component at the Same Position Preserves State
* Regardless if you change a component's styling, if it's the same component at the same position, from React's perspective it is the same component so its state does not get reset.
## Different Components at the Same Position Reset State
* If you had a checkbox that replacted a <Counter> component with a <p> (paragraph), you switch between different component types at the same position. When you swapped in the p, React removed the Counter from the UI tree and destroyed its state.
* When you render a different component in the same position, it resets the state of its entire subtree.
* If you want to preserve the state between re-renders, the structure of your tree needs to "match up" from one render to another. If the structure is different, the state gets destroyed because React destroys state when it removes a component from the tree.
## Resetting State at the Same Position
* By default, React preserves state of a component while it stays at the same position. There are two ways to reset state when switching between them:
### Option 1: Rendering a Component in Different Positions
* Each component's state gets destroyed each time it's removed from the DOM. This is why they reset every time you click the button.
* This solution is convenient when you only have a few independent components rendered in the same place.
### Option 2: Resetting State with a Key
* You can use keys to make React distinguish between any components. By default, React uses order within the parent ("first counter", "second counter") to discern between components.
* Keys let you tell React that this isn't just a first or second counter, but a specific one, so React recognizes it wherever it appears in the tree.
* Specifying a key tells React to use the key itself as part of the position, instead of their order within the parent. Even though they are rendered in the same place, React sees them as two different counters, so they never share state.
* Source: https://react.dev/learn/preserving-and-resetting-state
# Methods to Persisting State Between Page Reloads in React
* Source: https://blog.bitsrc.io/5-methods-to-persisting-state-between-page-reloads-in-react-8fc9abd3fa2f
