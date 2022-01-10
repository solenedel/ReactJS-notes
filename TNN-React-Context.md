## 1 - Introduction

- the React **Context API** gives us an easy way to share state between components without having to resort to prop drilling. 
- React **hooks** allow us to do a bunch of things inside functional components that we normally would only be able to do inside Class components.
- Together, these two things make it easy to use shared data in aour application, without the use of Redux (no need for a 3rd party library). 
 
 ## 2 - The Context API

 The context API allows us to share state up and down the component tree. This can be done without the context API using props, but sometimes it can get messy, especially as the app grows. The Context API gives us a central place to store state and share it between components, without having to pass it as props. The Context API is an alternative to Redux. 

The standard way of sharing state is by passing props, but this leads to prop drilling and sharing data with components that don't even need to use them, which act as 'middlemen'. This approach can get messy when there are a lot of nested components in the tree and/or the application is simply large with a lot of state to manage. 

Instead, we can create a new context in a new file. This is where a shared stte can initially be defined and set up. When we create a context, we have to then provide it to our component tree so that they can access it and get the data. This is done using the **Context Provider** which is a React tag that wraps the components that need access to the data. 

### When to use Context? 

Context is not always the best solution. It is designed to share data that is considered Global for the entire app. Examples include: current authenticated user, global UI theme, global language settings. Context is a good solution if the data is needed in many components of the app, including parallel ones (ie. those without a parent-child relationship). 

However, if for example you just need to pass a prop down to its child component to be used once, it would make more sense to use props. 


 ## 3 - Adding a Context & Provider

We should keep all the context-related files in one place by creating a `context` folder inside the `src` folder. 

In this example we will create `ThemeContext.js` inside the context folder, which will contain the data for the colour theme of our app. 

Inside this file: 

```
import React, { createContext } from 'react';

export const ThemeContext = createContext();
```

This will create a new context for us, but right now this doesn't do much because we haven't defined any data for this context, or provided it to any components. 

In the same file, let's create a class component: 

```
class ThemeContextProvider extends React.Component {
  state = {
    isLightTheme: true,
    light: { text: '#555', ui: '#ddd', bg: '#eee' }, 
    dark: { text: '#ddd', ui: '#333', bg: '#555' }
  }
  render() { 
    return <div></div>;
  }
}
 
export default ThemeContextProvider;
```

Here, we have added some state which represents the theme data we want to share between components of the app. 

Whenever we create a context, whatever name we give it- we are given a `Provider` on that context. This is what will wrap our components so that the theme data can be used inside them. 

We also need to provide a `value` property, which will take in whatever data we want to provide tothe components. The value we passed to it is an object spread from the original state object declared previously. 

```
 render() { 
    return (
      <ThemeContext.Provider value={{...this.state}}>
      </ThemeContext.Provider>
    )
  }
```

Now let's import ThemeContextProvider into App.js and surround the components with it. 

```
function App() {
  return (
    <div className="App">
      <ThemeContextProvider>
         <Navbar />
         <BookList />
      </ThemeContextProvider>
    </div>
  );
}
```

At the moment, nothing will show up on our app. Inside <ThemeContext.Provider> we need to output the components wrapped by <ThemeContextProvider> inside App.js. When we surround components using <ThemeContextProvider>, the children (in this case Navbar and BookList) are attatched to the props of the <ThemeContextProvider>.

We can access these components inside `ThemeContext.js` like so: 

```
 return (
      <ThemeContext.Provider value={{...this.state}}>
        {this.props.children}
      </ThemeContext.Provider>
    )
```

Now, we will see the components show up on the page. It will look like before we added context, but if we look at the component tree using React Dev Tools, we see this: 

```
<ThemeContextProvider>
  <Context.Provider>
    <Navbar />
    <BookList />
```

We can see the props of <Context.Provider>, which include the children components as well as the values which represent the theme data. 



-----------------

## Sources used: 
[The Net Ninja- react context & hooks](https://www.youtube.com/watch?v=CGRpfIUURE0&ab_channel=TheNetNinja)


Notes: 
- useRef
- React life cycle
- prev & spread syntax