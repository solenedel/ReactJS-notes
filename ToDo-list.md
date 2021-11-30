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







