# Updating state using prevState in React

A very simple counter example:

```
const [count, setCount] = useState(0);

const increment = () => {
  setCount(count + 1);
};

const decrement = () => {
  setCount(count - 1);
};

```
**NOTE** using `setCount(count + 1)` to set tha count is NOT WRONG! Even though some opinions on the internet say it is wrong. The first example of hooks in the React docs uses `setCount(count + 1)` as an example. 

## Using previous state

Even though it works in the above example, it is not the ideal way of setting state in general. There is a tool we can use which is the **previous state**. Any `setState` function can take an anonymous function which takes the previous state as an argument, often written as `prev`.

`setCount( (prev) => prev + 1)`

This will work exactly the same as our previous way of writing it, for this simple example. 

## Why is prevState the preferred method of setting state?

We can demonstrate this by setting the state more than once within the same function:

```
const increment = () => {
  setCount(count + 1); 
  setCount(count + 1);
  setCount(count + 1);
};
```

Based on Vanilla JS, we would expect that clicking the increment button will add three to the current value of count. However, **this does not work in React!** React updates state in **batches**.

What happens here, if the initial value of `count` is 0: 

```
const increment = () => {
  setCount(count + 1);  // 0 + 1 = 1
  setCount(count + 1);  // 0 + 1 = 1
  setCount(count + 1);  // 0 + 1 = 1
};
```


Therefore the value of `count` at the end of this will still only be 1!

What if we use the previous state method instead?

```
const increment = () => {
  setCount( (prev) => prev + 1);  // 0 + 1 = 1
  setCount( (prev) => prev + 1);  // 1 + 1 = 2
  setCount( (prev) => prev + 1);  // 2 + 1 = 3
};
```
This time, it works as we expect. The `prev` value is used as a running total for our `count`. Although it's probably not very often that we would update the state twice within the same function.


## Previous state when updating arrays


```
const [values, setValues] = useState([]);

const updateValues = (newVal) => {
  setValues([...values, newVal]);   
};

```

A new array is created in order to avoid mutating the original array. This is the standard method that doesn't use the previous state. 




## Sources
https://www.youtube.com/watch?v=yvTGXH7uybA&ab_channel=DaveGray


