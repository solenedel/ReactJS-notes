# Notes from the Full React Tutorial by The Net ninja

[link to course](https://www.youtube.com/watch?v=9D1x7-2FmTA&list=PL4cUxeGkcC9gZD-Tvwfod2gaISzfRiP9d&index=3)

## Lesson 3: Components and Templates

- A component is usually a self-contained section of content. 
- Each component contains its own template (JSX) and logic.
- To begin with, there is only one component being rendered: the App component - also known as the Root component (which is rendered in index.js).


## Lesson 4: Dynamic values in Templates

- in JSX, we can output dynamic values instead of hard coding them. 
- We do this by containing valid JavaScript inside curly braces {}
- The only thing that cannot be outputed in JSX is booleans and objects.
- ERROR: "objects are not valid as a react child"


 ## Lesson 5: Multiple components

 - we can nest as many components as we want.
 - the top level of the component tree is always the App (root) component. 
 - All other components must be direct or indirect children of App. 

## Lesson 7: click events

- If we have a button and we want to fire a function when a click event happens, we declare that `handleClick` function above the JSX for that component. 
- to link the handleClick to the button, we add the attribute to the button component:
`<button onClick={handleClick}>
- note that we don't invoke the function like `onClick={handleClick()}` because that would automatically fire the function without the user clicking on the button.