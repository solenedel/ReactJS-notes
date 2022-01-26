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

Presentational component: Counter.js
```
  export default Counter = () => {

    return (
      <>
        {counterVal}
        <button onClick={increment}>increment</button>
      </>
    );
  };
```


Logical component: useCounter.js (hook)
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



### sources 
https://www.youtube.com/watch?v=ocKqJCYkJCs
