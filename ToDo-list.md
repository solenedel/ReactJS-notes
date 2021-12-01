These notes are following [this tutorial](https://www.youtube.com/watch?v=nUl5QLkVdvU)

- JSX is a shorthand way of writing out a tree of HTML elements.
- React uses JSX and react elements to conrtsuct a virtual DOM. 
- the DOM is the underlying tree of HTML elements that make up a page.
- For react to upate the real DOM, it has to build a new virtual DOM, compare it to the old virtual DOM, and update only the elements that have changed in the actual DOM. 
- React is a declarative UI framework: meaning there is a layer of abstraction between what the you code in JavaScript, and what is output to the DOM. 

## Components in react

- components have their own structure and APIs associated with them. 
- components are powerful because they can be reused to reduce repetition in the code.


## State in react

- State can be an object or a specific data type containing information for our component(s).
- React uses this state information to decide when it should re-render certain components. 
- In react, **state is immutable** meaning it cannot be modified directly. 

## React hooks

- The most common React hooks are useState and useEffect.
- **useState** is a function that takes in an initial state as a property, and outputs an array of 2 items.

`const [state, setState] = useState([])`

 - The first item `state` is the actual state, which in this example is an array.
 - The second item `setState` is a function used to update the state. This function is needed to update the `state` array because state is immutable.


## Introducing the ToDo list app

The app will have 3 main components: 
  1. TodoForm.js -> renders the form needed for us to add todo items to the list.
  2. TodoList.js -> renders the list of todos in an array.
  3. Todo.js -> Renders a specific todo from the list and handles its actions.

### Step 1: define the todos state in App.js

`const [todos, setTodos] = useState([])`

If the todolist is on a specific page of the app, it doesn't have to be in App.js. It can be in a child component of App.js.

### Step 2: create Todo.js, TodoForm.js and TodoList.js in the components folder

Let's start with TodoForm.js. The function should return a <form> with an <input> and a <button> inside it.

Inside TodoForm, we need to define some state to keep track of input from the user. The `todo` state refers to a single task in the todo list. Its state is represented as an object with an id, the description of the task and whether or not it has been completed.

```
 const [todo, setTodo] = useState({
    id: '',
    task: '',
    completed: false,
  });
  ```

  ## Event handlers in React

  - React handles events differently from other front-end frameworks.
  - In react, all JSX elements have default camelCase properties on them that correspond to the standard DOM event handlers. 
  - for example, the click event is named `onClick`, submit event is `onSubmit`, and so on.
  - event handler functions in React are passed a single parameter which is a type of **SyntheticEvent** that holds data from the DOM event.
  - In our case, the only properties we care about from this parameter are `target` and `preventDefault()` 



### Step 3: Define a function to handle when the user types in the input, to keep track of the todo state

Let's define this function that takes the event `e` as a parameter:

```
const handleInputChange = (e) => {
  setTodo({ ...todo, task: e.target.value });
};
```

This function will update the `task` property on the todo object. It calls `setTodo` and passes it a new object with the old `todo` object spread onto it, and update the task property with the new value that we get from `e.target.value` - whatthe user types in the input field.

### Step 4: Define when the handleInputChange fuction should run

We want the function to run every time there is a change in the input value, so we use the `onChange` event handler on the <input> element. We also set the `value` attribute to be `todo.task` because that is the value to be updated every time `handleInputChange` is called.

```
<input type="text" name="task" placeholder="add new task" onChange={handleInputChange} value={todo.task}  />
```


### Step 5: Adding a new todo to the list

In App.js, let's create a function `addTodo` that takes in a `todo` task and adds it to the `todos` array.

```
const addTodo = (todo) => {
  setTodos([todo, ...todos]);
};
```

Here what we pass to setTodos is a new todos array with the new todo added to the beginning, followed by the old todos.


 ## Properties in React

 - Properties are parameters that live on JSX elements, and look identical to HTML properties.
 - The only difference is that we are writing the properties in JavaScript.
 - in React, most of the properties we write will be in camelCase and will in some cases differ from their corresponding HTML properties.  

## Props 

- Props are properties that we pass to components ourselves. 
- These props are arbitrary values and describe what should appear on the screen when the component renders.
- PROPS ARE UNI-DIRECTIONAL!! They only flow in one direction: top-down. Parent components can only pass props to child components, and child components can only receive props from parent components.
- Similarly, props cannot be passed directly between sibling components (components with the same direct parent). In order for sibling components to share props, we must lift up the state to their nearest parent component.


### Step 6: Pass addTodo as a prop from App.js to TodoForm.js

In App.js, the parent component, we pass it as a prop like so:

```
 <TodoForm addTodo={addTodo} />
 ```

 In TodoForm.js, the child component, the prop is received like so: 
 ```
 const TodoForm = ({ addTodo }) => { etc }
 ```
There are several ways to accept props form parent components. Another syntax is:
 ```
 const TodoForm = ({ props }) => { 
   const { prop1, prop2, prop3 } = props;
  }
 ```


### Step 7: Handle user submission of the form

When the user submits the form, we want to add the form's `todo` from the state to the list of `todos`. We use a handleSubmit function for this:

```
const handleSubmit = (e) => {
  e.preventDefault();

  // check that the input is not blank
  if (todo.task.trim()) {
    addTodo({ ...todo, id: uuid.v4() });

  // reset task input
  setTodo({ ...todo, task: '' });
  }
};
```

We get the id from a package called UUID. Use `npm i uuid` to install the package and import uuid at the top of the file.

Once handleSubmit is complete, add it to the <form> element's `onSubmit` event.
`<form onSubmit={handleSubmit}>`


### Step 8: TodoList component

Now it's time to start working on the TodoList component. TodoList will take in the `todos` prop from App.js.

```
const TodoList = ({ todos }) => {
  return (
    <ul>
      {todos.map((todo) => (
        <Todo key={todo.id} todo={todo} />
      ))}
    </ul>
  );
};
```

We map through the todos array and render a separate <Todo> component for each todo. When rendering a JSX element using the array map method, each item should have a unique key. In this case we use `todo.id` as the key since we know it is unique. Note: it is bad practice to use the array index as the key!


### Step 9: Define the structure of a Todo

Todo.js takes in `todo` as a prop from TodoForm. 

```
const Todo = ({ todo }) => {
  return (
    <div style={{ display: 'flex' }}>
      <input type="checkbox" />
      <li
        style={{
          color: 'white',
          textDecoration: todo.completed ? 'line-through' : null,
        }}
      >
        {todo.task}
      </li>
      <button type="button">X</button>
    </div>
  );
};
```

Currently, we can add several todos to the list, but refreshing the page will cause all of them to be lost. To fix this, we need to use the browser's **local storage** which will prevent our state from geting reset. 


## The useEffect hook

For this, we must use the `useEffect` hook. This hook lets us provide functionality that responds to certain data or functions in our code. 

useEffect takes in 2 parameters: a function (which is the effect), and a dependency array. 

```
useEffect(() => {
  // effect
  return () => {
    // clean up
  }
}, [dependencyArray])
```

The dependency array is a parameter that gets used to determine if the effect should be fired or not. If one or more variables in the dependency array changes, the effect will be fired. If an empty dependency array is provided, the effect will only fire when the component is initially rendered.



### Step 10: useEffect to persist state on reload

Every time the `todos` array changes, we want to store the new array inside local storage. 

Outside the component functionh in App.js, define a unique local storage key:
`const LOCAL_STORAGE_KEY = 'todaylist-todos'`

Next, inside the component function, we will use useEffect:

```
 // save todos array to local storage
  useEffect(() => {
    localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(todos));
  }, [todos]);
```

The reason we need to stringify the todos is that the only data type that can be saved into local storage is strings.

We also want to populate our todos array when the app initially renders. So we need another useEffect, with an empty dependency array this time. This useEffect should be above the previous one.

```
  useEffect(() => {
    const storageTodos = JSON.parse(localStorage.getItem(LOCAL_STORAGE_KEY));

    if (storageTodos) {
      setTodos(storageTodos);
    }
  }, []);
```

Now, the todos persist when refreshing the browser.


### Step 11: implement toggle complete action

Under `addTodo` in App.js, let's make a `toggleComplete` function.

```
 const toggleComplete = (id) => {
    setTodos(
      todos.map((todo) => {
        // find the selected todo in the todos array
        if (todo.id === id) {
          return {
            ...todo,
            completed: !todo.completed, // toggle completion status
          };
        }
        return todo;
      })
    );
  };
```

Then, pass the function as a prop to the TodoList component:
`<TodoList todos={todos} toggleComplete={toggleComplete} />`

Add the prop in TodoList.js:
`const TodoList = ({ todos, toggleComplete }) => { etc }`

In TodoList.js, we also need to pass toggleComplete down to the Todo component so that we can use it when we map over the todos. Don't forget to accept the prop in Todo.js.

We want the toggleComplete function to fire when we click on a todo's checkbox. Inside Todo.js, let's make a new function:

```
// handle checkbox click to trigger completion toggle
  const handleCheckboxClick = () => {
    toggleComplete(todo.id);
  };
```

Add the function to a click event:
`<input type="checkbox" onClick={handleCheckboxClick} />`



### Step 12: implement delete action

In App.js, create a new function `removeTodo`.

```
 const removeTodo = (id) => {
    setTodos(todos.filter((todo) => todo.id !== id));
  };
```

We want to keep a todo in the array if its id is NOT the one we clicked on. If the id does match, it will be removed from the array.

Pass this function to the TodoList component and accept it as a prop in TodoList.js. Also pass it on to the Todo component. 

Similar to the previous action, we need a function `handleRemoveClick` in Todo.js.

```
  const handleRemoveClick = () => {
    removeTodo(todo.id);
  };
  ```

  Now add this function to the onClick event of the <button>.


`<button type="button" onClick={handleRemoveClick}>`



## Section B: extra features (not in linked tutorial)

### Task: let the user create more to-do lists

We need to introduce a new layer- there will now be a component that wraps <TodoList> and <TodoForm>, let's call it <ListContainer>. In App.js (or whichever page originally rendered the <TodoList> and <TodoForm> components) we will need to map through each <ListContainer>.

Let's move the current JSX structure of App.js into the new ListContainer.js. Don't forget to pass the props from App.js to ListContainer.js

```
const ListContainer = ({ addTodo, todos, removeTodo, toggleComplete }) => {
  return (
    <section>
      <div className="today list">
        <h2>Today</h2>
        <TodoForm id="today" className="today" addTodo={addTodo} />
        <TodoList todos={todos} toggleComplete={toggleComplete} removeTodo={removeTodo} />
      </div>
    </section>
  );
};

```

App.js should now look like:
```
return (
    <main className={className} id="home-page-container">
      <ListContainer
        todos={todos}
        addTodo={addTodo}
        removeTodo={removeTodo}
        toggleComplete={toggleComplete}
      />
    </main>
  );
```

Currently, the app still compiles and works properly. There is no new logic, but we have introduced a new layer between App.js and TodoList and TodoForm.














