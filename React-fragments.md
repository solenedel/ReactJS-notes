# React Fragments

Fragments are used like JSX tags, except that they **do not render additional nodes to the DOM**. They are useful in many situations where we would normally wrap content in a redundant <div> or <span> tag simply because a react component can only return one (top-level) element. 

A great time to use fragments is when mapping through items in an array. 

There is more than one syntax possible when using fragments. The simplest way is to represent a fragment like this: `<> </>`

For example: 
```
return ( 
  <>
    <li>one</li>
    <li>two</li>
    <li>three</li>
  </> 
  );
```

There is only one attribute that can be currently passed to a fragment tag - the `key` prop needed when mapping through data. Note that in order to pass the key attribute to a fragment, we need to use a different fragment syntax by importing `Fragment` directly from the React library: 

```
import React, { Fragment } from 'react';

const Component = () => {

  return ( 
    <React.Fragment key={id}>
      <li>one</li>
      <li>two</li>
      <li>three</li>
    </React.Fragment> 
  );

}
```

## Fragments & styling

Whereas <div> and <span> can be given `className` and `id` attributes so that they can be styled with CSS, these attributes cannot be given to react fragments. In fact, because fragments do not add nodes to the DOM, there is nothing to style. The children of the fragment can be stylde, but not the fragment itself. This is one reason why we might need to use <div> or <span> instead of fragments. 






## Sources
https://www.youtube.com/watch?v=Du4Hkhj92NU
https://medium.com/@yassimortensen/using-react-fragments-b57a608d9a7f
https://stackoverflow.com/questions/60554716/add-styles-to-react-fragment
