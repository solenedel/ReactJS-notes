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

No matter what, the `renderCount` ref will not cause the component to re-render even when it changes, unlike state. 



### sources 
[useRef](https://www.youtube.com/watch?v=t2ypzz6gJm0)