# The spread operator in React

### Sources:
https://www.youtube.com/watch?v=L7o0wKjjFi4


 We use the spread operator by putting an ellipses (...) right before either an array, or an object.

 ` ...arrayName`

## Example 1: concatenating arrays

The below code uses spread syntax to achieve the same result as `arr1 = arr1.concat(arr2)`: 

```
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];

arr1 = [...arr1, ...arr2];
//  arr1 is now [0, 1, 2, 3, 4, 5]
// Note: Not to use const otherwise, it will give TypeError (invalid assignment)
```


We could also add other elements besides the arrays we are spreading:
`arr3 = [ -1, ...arr1, ...arr2]` 
This would prepend the resulting array with -1.


## Example 2: destructuring arrays

We can use the spread operator to assign multiple values to a single variable. The example below is from MDN:

```
let a, b, rest;
[a, b] = [10, 20];

console.log(a);
// expected output: 10

console.log(b);
// expected output: 20

[a, b, ...rest] = [10, 20, 30, 40, 50];

console.log(rest);
// expected output: Array [30, 40, 50]
```

We can see that the `rest` variable is automatically assigned the values in the array that come after `b`. We explicitly destructured `a` and `b`, but all remaining values are assigned to `rest`.


## Example 3: objects in React

Above the App.js component function, define an array of objects, `employee`:

```
const employee = [
  {
    name: "Bob",
    id: 1
  },
  {
    name: "Jo",
    id: 2
  }
];
```

If we have a new component called Employee which looks like: 

```
const Employee = (props) => {

  const { name, id } = props;

  return (
    <div>
    <h6>{`Name: ${name}, ID: ${id}`}</h6>
    </div>
  )
}
```

This component just displays the name and id of the employee, which are passed to it as props from App.js. We can do this the standard way: 

```
const firstEmployee = employee[0]; // Bob

return (
  <div className="App>
    <Employee
      name={firstEmployee.name}
      id={firstEmployee.id}
      />
  </div>
)
```

A simpler way to pass the props is using the spread operator. This would be especially useful if there are many more props to pass than just name and id.

```
<Employee {...firstEmployee} />
```

This will spread the attributes of the object into an empty object {}, and pass these attributes as props.

Another example is that we can combine the properties of the firstEmployee object with a base object.

```
const baseEmployeeObject = { 
  role: "employee",
  company: "ubisoft"
}
```

This object has the same properties for all employees. To add these properties to a specific employee, we can do this:

```
const firstEmployee = { ...baseEmployeeObject, ...employee[0] }
```

The resulting firstEmployee object will have all the base employee attributes, plus the attributes for that specific employee.




