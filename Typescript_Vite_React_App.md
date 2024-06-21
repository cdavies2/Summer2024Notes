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
## Props
* Props are JavaScript objects holding some data.
* You can pass an array for instance to a component instead of hardcoding it.
* If we're using TypeScript, we need to specify the types of data inside the props object (EX: an array of strings, or string[])
```
const Component = (props: {paragraphs: string[]}) => {
  <>
    {props.paragraphs.map((paragraph) => <p>{paragraph}</p>)}
  </>
}

const OtherComponent = () => {
  const paragraphs = ['First', 'Second', 'Third']
  return (
    <Component paragraphs={paragraphs}/>
  )
}
```
## State
* If we want to make an interactive component, we're going to need to store information in the component's state, so it can "remember" it.
* Hooks are functions that let you "hook" into React state and lifecycle features
* React has the useState hook to store and update the number of times a button is clicked
```
import { useState } from 'react'

const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <>
      <span>{count}</span>
      <button onClick={() => setCount(count + 1)}>Increment count</button>
    </>
  )
}
```

# Creating the Project
## Dependencies
* To use Vite, you need node and a package manager, like npm or yarn
## Creating the project
* To create the project in the terminal, navigate to the directory where the project will be created, then run the below create-vite command.
```
npm create vite@latest
```
*  If prompted to install the create-vite package, type y and enter
* Then enter the name of the project when prompted
* Select React as your "Framework"
* Select TypeScript + SWC as your "Variant"
  * SWC (Speedy Web Compiler) is a super-fast TypeScript/JavaScript compiler
* Once the project is created, change to the project directory, install dependencies and run the dev script command
```
cd my-react-project
npm install
npm run dev
```
*  Once your local host appears, open it in your browser, and if the default Vite + React page is there, everything is as at should be and we can work on your app.

# Building the App
## File Structure and Initial Setup
* Open the project in your code editor of choice, and delete the code in the app function (which is contained in App.tsx)
## Styling
* You can use Tailwind CSS, a library that lets you style HTML elements by adding classes to them.
* Install tailwind and its dependencies in your project with these commands
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```
*  Next, add the paths to all of your template files in your "tailwind.config.js" file
```
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```
*  You can also add the @tailwind directives for each of Tailwind's layers to your ./src/index.css file
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```
*  Finally, run your build process, and you can start using Tailwind's utility classes
```
npm run dev
```
## Thinking About Design: Components Layout
* To build an app, first think of the components layout.
* First, mock a basic UI and outline a hierarchy of components involved
* Ideally, our components should be responsible for only one thing, following the "single responsibility principle"
* If two elements are inside each other, there's likely going to be a parent-child relationship
## Props: Building a Static Version
* After we have a sketch, build a static version of the app (just UI elements, but with no interactivity yet)
## Container
* We will have a container that's reused for every section of the app. The container composes different elements by passing them as children
* It takes a props object with a children parameter of type JSX.Element | JSX.Element[]. This means we can compose it with any other HTML element or any other components we create, and it can rendered wherever we want inside the container.
* In our app the container will render each section when we use them inside the App component.
* The Container also takes a string pop named title, which is rendered inside an h2 whenever it exists.
```
// src/components/Container.tsx
const Container = ({
  children,
  title,
}: {
  children: JSX.Element | JSX.Element[];
  title?: string;
}) => {
  return (
    <div className="bg-green-600 p-4 border shadow rounded-md">
      {title && <h2 className="text-xl pb-2 text-white">{title}</h2>}
      <div>{children}</div>
    </div>
  );
};

export default Container;
```
```
// src/App.tsx
import Container from "./components/Container";
import Input from "./components/Input";
import Summary from "./components/Summary/Summary";
import Tasks from "./components/Tasks/Tasks";

function App() {
  return (
    <div className="flex justify-center m-5">
      <div className="flex flex-col items-center">
        <div className="sm:w-[640px] border shadow p-10 flex flex-col gap-10">
          <Container title={"Summary"}>
            <Summary />
          </Container>
          <Container>
            <Input />
          </Container>
          <Container title={"Tasks"}>
            <Tasks />
          </Container>
        </div>
      </div>
    </div>
  );
}

export default App;
```
## Summary
* The first section is a summary (Summary component) showing three items
  * SummaryItem: The total number of tasks, the number of pending tasks, and the number of completed tasks.
* This is one way of composing components: use them in the return statements of another component
* Never DEFINE a component inside another component, that can lead to rerenders and bugs
* When you start, just use static data in the components
```
// src/components/Summary/SummaryItem.tsx
const SummaryItem = ({
  itemName,
  itemValue,
}: {
  itemName: string;
  itemValue: number;
}) => {
  return (
    <article className="bg-green-50 w-36 rounded-sm flex justify-between p-2">
      <h3 className="font-bold">{itemName}</h3>
      <span className="bg-green-900 text-white px-2 rounded-sm">
        {itemValue}
      </span>
    </article>
  );
};

export default SummaryItem;
```
```
// src/components/Summary/Summary.tsx
import SummaryItem from "./SummaryItem";

const Summary = () => {
  return (
    <>
      <div className="flex justify-between">
        <SummaryItem itemName={"Total"} itemValue={3} />
        <SummaryItem itemName={"To do"} itemValue={2} />
        <SummaryItem itemName={"Done"} itemValue={1} />
      </div>
    </>
  );
};

export default Summary;
```
* SummaryItem takes two props:
  1. itemName-of type string
  2. itemValue-of type number
*  These props are passed when the SummaryItem component is used inside the Summary component, and then rendered in the SummaryItem JSX.

## Tasks
* The Tasks component renders the TaskItem Components
* You need to pass a task name and status down as props to the TaskItem component to make it reusable and dynamic.
```
// src/components/Tasks/TaskItem.tsx
const TaskItem = () => {
  return (
    <div className="flex justify-between bg-white p-1 px-3 rounded-sm">
      <div className="flex gap-2 items-center">
        <input type="checkbox" />
        Task name
      </div>
      <button className="bg-green-200 hover:bg-green-300 rounded-lg p-1 px-3">
        Delete
      </button>
    </div>
  );
};
```
```
export default TaskItem;

// src/components/Tasks/Tasks.tsx
import TaskItem from "./TaskItem";

const Tasks = () => {
  return (
    <div className="flex flex-col gap-2">
      <TaskItem />
    </div>
  );
};
export default Tasks;
```
## Input
*  The Input component is a form with a label, an input of type text, and a button to "Add Task".
```
// src/components/Input.tsx
const InputContainer = () => {
  return (
    <form action="" className="flex flex-col gap-4">
      <div className="flex flex-col">
        <label className="text-white">Enter your next task:</label>
        <input className="p-1 rounded-sm" />
      </div>
      <button
        type="button"
        className="bg-green-100 rounded-lg hover:bg-green-200 p-1"
      >
        Add task
      </button>
    </form>
  );
};

export default InputContainer;
```

# State: Adding Interactivity
* To add interactivity in React, store information in the component's state.
* State should contain every bit of information necessary to make our app interactive but nothing more. If we can compute a value from a different value, keep just one of them in state, thus making our code less prone to bugs involving contradictory values
* To track tasks, all you need is an array with objects representing each task and its status (pending or done).
```
const tasks = [
  {
    name: "task one",
    done: false,
  },
  {
    name: "task two",
    done: true,
  },
];
```
*  We also ned state in our form (in the Input component) to make it interactive
## Where the State Should Live
* If a single component needs to access the data we're going to store in state, the state can live in the component itself.
* If it's more than one component that needs the data, then you should find the common parent to these components.
* Because the state necessary to control the Input component only needs to be accessed there, it can be local to this component.
```
const [newTask, setNewTask] = useState(""); // Initialize newTask and setNewTask, put this right after const InputContainer
  return (
```
```
//put this after type="text"
value={newTask} // Set the input value to newTask
          onChange={(e) => setNewTask(e.target.value)} // Set newTask to the input value whenever the user types something
```
*  What this does is display our newTask value in the input, and calls the setNewTask function whenever the input changes (like when the user types something)
*  The state to track tasks has to be access in SummaryItem components (we need to show the number of total, pending, and done tasks) and the TaskItem components (we need to display the tasks themselves). It also needs to be the same state because this information must always be in sync.
*  Because App is the first common parent component for Summary and Tasks,this is where our state for the tasks is going to live.
*  With the state in place, all that will be left will be to pass the data down as props to the components that need to use it.
```
// src/App.tsx
import { useState } from "react";
import { v4 as uuidv4 } from 'uuid'
import Container from "./components/Container";
import Input from "./components/Input";
import Summary from "./components/Summary/Summary";
import Tasks from "./components/Tasks/Tasks";

export interface Task {
  name: string;
  done: boolean;
  id: string;
}

const initialTasks = [
  {
    name: "task one",
    done: false,
    id: uuidv4(),
  },
  {
    name: "task two",
    done: true,
    id: uuidv4(),
  },
];

function App() {
  const [tasks, setTasks] = useState<Task[]>(initialTasks);
  return (
    <div className="flex justify-center m-5">
      <div className="flex flex-col items-center">
        <div className="border shadow p-10 flex flex-col gap-10 sm:w-[640px]">
          <Container title={"Summary"}>
            <Summary tasks={tasks} />
          </Container>
          <Container>
            <Input />
          </Container>
          <Container title={"Tasks"}>
            <Tasks tasks={tasks} />
          </Container>
        </div>
      </div>
    </div>
  );
}

export default App;
```
* Here, we're initializing the tasks value with dummy data (initialTasks) so we can visualize it before the app is finished
* Later, change it to an empty array, so a new user will not see any tasks when opening up the app fresh.
* We're adding the name and done properties, as well as an id to our task objects.
* We're defining an interface with the types of the value in the task objects, and passing it to the useState function.This is needed because TypeScript cannot infer type
* Because we're passing tasks down as props to the Summary and Tasks components, said components will need to be changed to accommodate that.
```
// src/components/Summary/Summary.tsx
import { Task } from "../../App";
import SummaryItem from "./SummaryItem";

const Summary = ({ tasks }: { tasks: Task[] }) => {
  const total = tasks.length;
  const pending = tasks.filter((t) => t.done === false).length;
  const done = tasks.filter((t) => t.done === true).length;
  return (
    <>
      <div className="flex flex-col gap-1 sm:flex-row sm:justify-between">
        <SummaryItem itemName={"Total"} itemValue={total} />
        <SummaryItem itemName={"To do"} itemValue={pending} />
        <SummaryItem itemName={"Done"} itemValue={done} />
      </div>
    </>
  );
};

export default Summary;
```
*  This updates the Summary component so it now accepts tasks as a prop. The values for total, pending, and done are also defined and can be passed down as props.
```
// src/components/Tasks/Tasks.tsx
import { Task } from "../../App";
import TaskItem from "./TaskItem";

const Tasks = ({ tasks }: { tasks: Task[] }) => {
  return (
    <div className="flex flex-col gap-2">
      {tasks.map((t) => (
        <TaskItem key={t.id} name={t.name} />
      ))}
    </div>
  );
};

export default Tasks;


// src/components/Tasks/TaskItem.tsx
import { useState } from "react";

const TaskItem = ({ name }: { name: string }) => {
  const [done, setDone] = useState(false);
  return (
    <div className="flex justify-between bg-white p-1 px-3 rounded-sm gap-4">
      <div className="flex gap-2 items-center">
        <input type="checkbox" checked={done} onChange={() => setDone(!done)} />
        {name}
      </div>
      <button className="bg-green-200 hover:bg-green-300 rounded-lg p-1 px-3">
        Delete
      </button>
    </div>
  );
};

export default TaskItem;
```
* The above code makes it so, for the Tasks component, we take tasks as a prop, and map its name property to TaskItem components.
* As a result, we get a TaskItem component for each object inside the tasks array. We also update the TaskItem component to accept name as a prop.
* Id allows us to pass a unique key every time we have a list of child components
## Adding Inverse Data Flow
*  To finish our app, we need to change the App component state from the Input and TaskItem components.
*  To do that, we can use the functions generated by the useState hook to define event handlers, and pass them down as props. Afterwards, we simply call them during the appropriate user interaction from the child components.
*  Never mutate state whenever you're updating it, always replace the state object with a new one when updating it.
*  Below is our final App component with the handlers declared and passed down as props to the Input and Tasks components.
   *  handleSubmit returns a new array with the old tasks plus the new one.
   *  toggleDoneTask returns a new array with the opposite done property, for the specified id.
   *  handleDeleteTask returns a new array without the task with the specified id.
```
// src/App.tsx
import { FormEvent, useState } from "react";
import { v4 as uuidv4 } from "uuid";
import Container from "./components/Container";
import Input from "./components/Input";
import Summary from "./components/Summary/Summary";
import Tasks from "./components/Tasks/Tasks";

export interface Task {
  name: string;
  done: boolean;
  id: string;
}

function App() {
  const [tasks, setTasks] = useState<Task[]>([]);

  const handleSubmit = (e: FormEvent<HTMLFormElement>, value: string) => {
    e.preventDefault();
    const newTask = {
      name: value,
      done: false,
      id: uuidv4(),
    };
    setTasks((tasks) => [...tasks, newTask]);
  };

  const toggleDoneTask = (id: string, done: boolean) => {
    setTasks((tasks) =>
      tasks.map((t) => {
        if (t.id === id) {
          t.done = done;
        }
        return t;
      })
    );
  };

  const handleDeleteTask = (id: string) => {
    setTasks((tasks) => tasks.filter((t) => t.id !== id));
  };

  return (
    <div className="flex justify-center m-5">
      <div className="flex flex-col items-center">
        <div className="border shadow p-10 flex flex-col gap-10 sm:w-[640px]">
          <Container title={"Summary"}>
            <Summary tasks={tasks} />
          </Container>
          <Container>
            <Input handleSubmit={handleSubmit} />
          </Container>
          <Container title={"Tasks"}>
            <Tasks
              tasks={tasks}
              toggleDone={toggleDoneTask}
              handleDelete={handleDeleteTask}
            />
          </Container>
        </div>
      </div>
    </div>
  );
}

export default App;
```
* Below is the final Input component using handleSubmit to update the App component state
```
// src/components/Input.tsx
import { FormEvent, useState } from "react";

const InputContainer = ({
  handleSubmit,
}: {
  handleSubmit: (e: FormEvent<HTMLFormElement>, value: string) => void;
}) => {
  const [newTaskName, setNewTaskName] = useState("");
  return (
    <form
      action=""
      className="flex flex-col gap-4"
      onSubmit={(e) => {
        handleSubmit(e, newTaskName);
        setNewTaskName("");
      }}
    >
      <div className="flex flex-col">
        <label className="text-white">Enter your next task:</label>
        <input
          className="p-1 rounded-sm"
          type="text"
          value={newTaskName}
          onChange={(e) => setNewTaskName(e.target.value)}
        />
      </div>
      <button
        type="submit"
        className="bg-green-100 rounded-lg hover:bg-green-200 p-1"
      >
        Add task
      </button>
    </form>
  );
};

export default InputContainer;
```
*  Below is the final Tasks component, which we updated to pass the props from App down to TaskItem. We also added a ternary operator to return "No tasks yet!" when there are no tasks.
```
// src/components/Tasks/Tasks.tsx
import { Task } from "../../App";
import TaskItem from "./TaskItem";

const Tasks = ({
  tasks,
  toggleDone,
  handleDelete,
}: {
  tasks: Task[];
  toggleDone: (id: string, done: boolean) => void;
  handleDelete: (id: string) => void;
}) => {
  return (
    <div className="flex flex-col gap-2">
      {tasks.length ? (
        tasks.map((t) => (
          <TaskItem
            key={t.id}
            name={t.name}
            done={t.done}
            id={t.id}
            toggleDone={toggleDone}
            handleDelete={handleDelete}
          />
        ))
      ) : (
        <span className="text-green-100">No tasks yet!</span>
      )}
    </div>
  );
};

export default Tasks;
```  
*  Below is the final TaskItem component, using toggleDone and handleDelete to update the App component state.
```
// src/components/Tasks/TaskItem.tsx
const TaskItem = ({
  name,
  done,
  id,
  toggleDone,
  handleDelete,
}: {
  name: string;
  done: boolean;
  id: string;
  toggleDone: (id: string, done: boolean) => void;
  handleDelete: (id: string) => void;
}) => {
  return (
    <div className="flex justify-between bg-white p-1 px-3 rounded-sm gap-4">
      <div className="flex gap-2 items-center">
        <input
          type="checkbox"
          checked={done}
          onChange={() => toggleDone(id, !done)}
        />
        {name}
      </div>
      <button
        className="bg-green-200 hover:bg-green-300 rounded-lg p-1 px-3"
        type="button"
        onClick={() => handleDelete(id)}
      >
        Delete
      </button>
    </div>
  );
};

export default TaskItem;
```
*  
