# TypeScript
* TypeScript is used for writing the code for an app
* TypeScript builds on JavaScript via types and interfaces
  * Types and interfaces allow us to define the data types we're using in code, allowing bugs to be caught early on
  * EX: If a function takes a number but we pass a String, TypeScript will let us know immediately
# Vite
* Vite is used for developing and building the app
* Vite uses native ES modules in the browser (Rollup) to bundle our application more efficiently
* Vite also greatly speeds up development time with Hot Module Replacement (when changes are made to source code, only changes are updated, rather than the whole application)
* Vite offers native support for TypeScript, JSX, TSX, and CSS, along with a tool called create-vite,which gives us basic templates for starting a project.

# Diving Into React
## JSX/TSX
* React allows us to add markup directly in code which will later be compiled to plain JavaScript. This is called JSX (JavaScript is saved as .jsx, TypeScript is .tsx)
* It's similar to HTML, but it's embedded in the JavaScript file, allowing us to manipulate markup with programming logic. We can also add JavaScript code inside the JSX, as long as it's inside curly brackers.
```
const element = <h1>Hello, world!</h1>;
```
## React Components
* React components can be written using JavaScript classes or functions (functions is recommended)
* A component is defined by a function that'll return the JSX, which will be compiled and rendered by a browser. Rendering paragraph elements would look like this:
```
// Define the component
const Component = () => {
  const paragraphs = ["First", "Second", "Third"];
  return (
    <>
      {paragraphs.map((paragraph) => (
        <p>{paragraph}</p>
      ))}
    </>
  );
}

// Use the component in the same way you use an HTML element in the JSX
const OtherComponent = () => {
  return (
    <Component />
  )
}
```

