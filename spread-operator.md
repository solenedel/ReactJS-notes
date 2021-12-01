# The spread operator in React

### Sources:
https://www.youtube.com/watch?v=L7o0wKjjFi4


 ## Syntax

 We use the spread operator by putting an ellipses (...) right before either an array, or an object.

 ` ...arrayName`

### Example 1: concatenating 

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


### Example 2: destructuring
