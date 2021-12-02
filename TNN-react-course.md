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
`<button onClick={handleClick}>`
- note that we don't invoke the function like `onClick={handleClick()}` because that would automatically fire the function without the user clicking on the button.

What if we wanted to pass in an argument to the function? We can't do this `<button onClick={handleClick(argument)}>` since that would invoke the function right away. To get around this, we need to wrap the handleClick function in an anonymous function. 

`<button onClick={() => handleClick(argument)}>`


### The event parameter

We automatically get access to the event parameter when an event occurs. This parameter is found in the `handleClick` function.

`const handleClick = (e) => { console.log(e) }`

When we log `e` to the console, we get a `SyntheticBaseEvent`.

What about when we take in a parameter for the handler function?

```
const handleClick = (name) => {
  console.log('hello' + name);
}
```

We don't automatically get the event parameter as the first argument inside this function, because in our code, it isn't actually getting referenced as the function (it's called by an anonymous function). Instead, the anonymous function gets access to the event parameter:

`<button onClick={(e) => handleClick(argument, e)}>`

We pass `e` as the second argument to the handleClick function.

```
const handleClick = (name, e) => {
  console.log('hello' + name, e.target);
}
```


## Lesson 8: Using state (useState)




