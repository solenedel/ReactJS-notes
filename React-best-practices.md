# React best practices 

## 1. Separating the UI and logic of components

### why?
- better readability and maintainability of code
- if a component file has a lot of UI and a lot of logic, it will be large and messy

### how?

- Separate the component into a **presentational component** and a **logical component**. 

**presentational component:**
- components that are purely for UI should **not have states declared** inside them.
- and should not have any logic such as event handler functions - leave only the returned JSX in this component. 

**logical component:**
- this is essentially a **custom hook** which contains all the state and logic needed in a particular presentational component. 
- In terms of organisation, there are two options:

A. Have all hooks of the app in a `hooks` folder that is inside the `src` folder. This makes more sense when a hook needs to be re-used in several different presentational components.

B. Separate each "component set" (aka one presentational + the corresponding logical component) into a different folder. For example:

src/components/SearchBar -> [ useSearchBar.js (logical) & SearchBar.jsx (presentational)]
                       
### EXAMPLE

**Logical component: useCounter.js (hook)**
```
  export default useCounter = () => {
    const [ counterVal, setCounterVal ] = useState(0);

    const increment = () => {
      setCounterVal(counterVal + 1);
    };

    return { counterVal, increment };
  };
```
Note that in the logical component we don't return any JSX related to the UI. 


**Presentational component: Counter.js**
```
  export default Counter = () => {

    const { increment, counterVal } = Counter(); // remember to import the counter component in the file

    return (
      <>
        {counterVal}
        <button onClick={increment}>increment</button>
      </>
    );
  };
```

## 2. Avoid using inline lambdas inside JSX

**Inline lambdas** in JS are basically anonymous function expressions inserted directly where they are to be used. In react, an example of this would be: 

```
<button onClick={() => setCounterVal(counterVal + 1)}>
  increment
</button>
```
As opposed to using the already defined `increment` function. This is fine but there are some potential problems with using inline expressions like this in React. 

- Every time a component is re-rendered in react, the part of the component file that re-runs is only the return statement with JSX. 
- This means any pre-defined functions that are outside the JSX return statement will not be re-run at every render. 
- However if we use anonymous inline lambdas inside the JSX, at every re-render, this lambda expression will be re-created as well, even if there is no need. 
- another reason to pass in pre-defined functions rather than inline lambdas is that the JSX looks cleaner and there is less logic mixed in with it. 


### sources 
https://www.youtube.com/watch?v=ocKqJCYkJCs
