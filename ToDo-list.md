These notes are following [this tutorial](https://www.youtube.com/watch?v=nUl5QLkVdvU)

- JSX is a shorthand way of writing out a tree of HTML elements.
- React uses JSX and react elements to conrtsuct a virtual DOM. 
- the DOM is the underlying tree of HTML elements that make up a page.
- For react to upate the real DOM, it has to build a new virtual DOM, compare it to the old virtual DOM, and update only the elements that have changed in the actual DOM. 
- React is a declarative UI framework: meaning there is a layer of abstraction between what the you code in JavaScript, and what is output to the DOM. 

## Introducing the ToDo list app

The app will have 3 main components: 
  1. TodoForm.js -> renders the form needed for us to add todo items to the list.
  2. TodoList.js -> renders the list of todos in an array.
  3. Renders a specific todo from the list and handles its actions.


## Components in react

- components have their own structure and APIs associated with them. 
- components are powerful because they can be reused to reduce repetition in the code.


## State in react

- state can be an object or a specific data type containing information for our component(s).
- React uses this state information to decide when it should re-render certain components. 
- In react, **state is immutable** meaning it cannot be modified directly. 



