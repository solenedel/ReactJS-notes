# useRef and useHistory

## useRef hook

### Basic example 

**useRef** is the most flexible react hook, but also the most misused. 

What if you wanted to keep track of how many times your component re-renders? There is no way to do this by keeping track of a state variable for number of renders, because it would result in an infinite loop. The solution is to `useRef`. 

A **Ref** is very similar to state in that it persists between renders of your component, but an important difference between refs and state is that a ref does NOT cause the component to re-render when it is changed. 

Let's try to keep track of the number of renders: 

`const renderCount = useRef(0)`
Like with useState, we pass in the initial value as an argument to useRef. This returns an object which has a single property: `{ current: 0 }`
When we update the current property, its value gets persisted between the different renders. 

To update the `current` property every time a component re-renders (due to the changing state of something in the component), we can do this with useEffect:

```
useEffect(() => {
  renderCount.current =  renderCount.current + 1;
});
```
Note that we don't pass in any dependency array. 

No matter what, the `renderCount` ref will not cause the component to re-render even when it changes, unlike state. Also remember that if you want to output the value of a Ref to the page, you need to access the `current` property of the ref object (objects are not valid as a react child).

### The most common case of useRef: to reference elements inside HTML

Each element in the HTML (JSX) has access to a `ref` attribute that you can set to whatever you want. 

For example, if we want to keep track of an input element: 
```
  const [name, setName] = useState(''); 
  const inputRef = useRef();

  <input ref={inputRef} value={name} onChange={e => setName(e.target.value)}>
```

A common scenario is that we want the text input to become focused when clicking on something, for example a button: 

```
  const focus = () => {
    inputRef.current.focus();
  };

  <input ref={inputRef} value={name} onChange={e => setName(e.target.value)}>
  <button onClick={focus}>Focus</button>
```

Now clicking on the button will cause the input to become focused. 

### WHAT YOU SHOULD NOT DO! 

Because this is so easy, many people overuse useRef instead of letting React manage the states, onChanges, etc - people sometimes use Refs for onChange logic (instead of using state and onChange handlers).

For example: 

```
// DO NOT DO THIS
  const focus = () => {
    inputRef.current.focus();
    inputRef.current.value = 'some value';
  };
```

This is a BIG NO NO! NEVER DO THIS!

### Another use case: storing the previous value of a state

There is no way to get the previous value of state (except when actually changing the state using the setState function, where it can be accessed as an argument).

With useEffect, we can store the previous value of a state every time it changes into a new value:

```
  const [name, setName] = useState(''); 
  const prevName = useRef('');

  useEffect(() => {
    prevName.current = name;
  }, [name]);

```

### summary

Refs are great for accessing DOM elements and presisting values across re-renders, without actually causing a re-render. 


## useHistory hook
- this hook is imported from reqct-router-dom. It is often used for page redirections.

https://www.youtube.com/watch?v=0ZRup5eSReo
https://reactgo.com/react-router-usehistory-hook/


## useLocation hook

### sources 
[useRef](https://www.youtube.com/watch?v=t2ypzz6gJm0)