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



 ## 4 - Accessing context (Part 1)

 How can we access the theme data from within the Mavbar or BookList components? When using class components, there are several ways we can do this. In this video we will use the **contextType** to achieve this. **NOTE: it cannot be used in functional components** 

Example, in the Navbar: 

```
class Navbar extends React.Component {
  static contextType = ThemeContext;

```
Note that we are setting the contextType to the **context itself, NOT the provider**. What this does is look up the component tree, to the first time it finds a provider for the specified context.

It takes the data we passed to the `value` property of the context provider and attaches it to a `context` property inside the component. Let's log it to the console to see if we have the data: 

```
class Navbar extends React.Component {
  static contextType = ThemeContext;

  render() { 
    console.log(this.context);
    
    return ( ....
```

The `this` keyword here refers to the `Navbar` component, and we can see that the `context` property indeed contains the theme data. 

We can destructure the different properties from `this.context` so that each one can be accessed separately. 

`const { dark, light, isLightTheme } = this.context` // inside the render method

Now we can add the logic to conditionally render the theme: 

`const theme = isLightTheme ? light : dark` // inside the render method

` <nav style={{background: theme.ui, color: theme.text}}>`

At this point, we see the colours being applied to the Navbar and we can check/uncheck isLightTheme in the React dev tools to see the theme changes. 

Similarly, adding the theme to the BookList component: 

```
class BookList extends React.Component {

  static contextType = ThemeContext;

  render() { 

    const { dark, light, isLightTheme } = this.context;
    const theme = isLightTheme ? light : dark;

    return <div className="book-list" style={{color: theme.text, background: theme.bg}}>
      <ul>
        <li style={{background: theme.ui}}>Book 1</li>
        <li style={{background: theme.ui}}>Book 2</li>
        <li style={{background: theme.ui}}>Book 3</li>
      </ul>
    </div>;
  }
}
```


 ## 5 - Accessing context (Part 2)

Previously we consumed context inside components using the contextType property. Let's look at another way to do this using the **context consumer**. Like we have the `Provider` given to us when we create a context, we also have a `Consumer`. 

We have to wrap the JSX of the component with <ThemeContext.Consumer> and then pass in a function which takes the context as a parameter. The function returns the JSX we had previously. 

```
  return (
      <ThemeContext.Consumer>{(context) => {

        const { dark, light, isLightTheme } = context;
        const theme = isLightTheme ? light : dark;

        return (
          <nav style={{background: theme.ui, color: theme.text}}>
            <h1>Context App</h1>
            <ul>
              <li>Home</li>
              <li>About</li>
              <li>Contact</li>
            </ul>
          </nav>
        )
      }}</ThemeContext.Consumer>
    );
```

What is happening here? We get access to the context data as the first parameter of the function. Note that we now refer to the context simply as `context` and not `this.context`.

Should we use this method or the previous one? When using class components, prefer the first method (using **contextType**). 

But the second method can also be used in functional components (while the first method cannot). Another benefit of using the **consumer** is that we can consume multiple contexts in one component.  


 ## 6 - Updating context data 

 What is we want to change the shared state data at some point?

 To illustrate this, let's create a new `ThemeToggle` component that we can use to switch between the themes. This will use the `isLightTheme` data and change it when we click on the button. 

 ```
 class ThemeToggle extends React.Component {
  render() { 
    return (
      <button onClick={}>Toggle theme</button>
    )
  }
}
```

The click handler function has not been defined yet. It should be defined in the ThemeContextProvider class where we definied the initial state, since we might need the function in different components. See the `toggleTheme` function below (NOTE: corrected from the original code in the tutorial video). 

```
class ThemeContextProvider extends Component {
  state = {
    isLightTheme: true,
    light: { text: '#555', ui: '#ddd', bg: '#eee' }, 
    dark: { text: '#ddd', ui: '#333', bg: '#555' }
  }

  toggleTheme = () => {
    this.setState(( prevState ) => { return {isLightTheme : !prevState.isLightTheme} });
  };
```

Now we add the toggleTheme function as another property to the object we passed as the `value` property of <ThemeContext.Provider>.

`<ThemeContext.Provider value={{...this.state, toggleTheme: this.toggleTheme }}>`

Now we should have access to the toggleTheme function inside any component that consumes the theme context. Let's add it to the ThemeToggle component. 

```
class ThemeToggle extends React.Component {

  static contextType = ThemeContext; 

  render() { 
      const { toggleTheme } = this.context;
       
      return (
      <button onClick={toggleTheme}>Toggle theme</button>
    )
  }
}
```

Add the ThemeToggle to App.js. The toggle button now works to change the theme. 



 ## 7 - Creating multiple contexts

 What if we have more data we need to share between components, besides the theme data. For example, user authentication data. Should we put it in the same context as the theme? That wouldn't be a good idea, because the ThemeContext we created is only for the theme. 

 Instead we should create a separate context just for the authentication data. Inside the context folder, create AuthContext.js. 

 ```
export const AuthContext = createContext();

class AuthContextProvider extends React.Component {

  state = {
    isAuthenticated: false, 
  }

  toggleAuth = () => {
    this.setState((prevState) => { return { isAuthenticated: !prevState.isAuthenticated }})
  }

  render() { 
    return (
      <AuthContext.Provider value={{...this.state, toggleAuth: this.toggleAuth}}>
        {this.props.children}
      </AuthContext.Provider>
    );
  }
}
```

Again, we need to nest the components that will use the AuthContext inside <AuthContextProvider>.

```

function App() {
  return (
    <div className="App">
      <AuthContextProvider>
        <ThemeContextProvider>
          <Navbar />
          <BookList />
          <ThemeToggle />
        </ThemeContextProvider>
      </AuthContextProvider>
    </div>
  );
}
```

In this case, it doesn't matter if we switch the positions of AuthContextProvider and ThemeContextProvider because they both wrap the same components. 


 ## 8 - Consuming multiple contexts

 If we want to use two separate contexts in the same component, how do we do this?

 The code below would not work because we would have two static properties with the exact same name `contextType` but different values. 
 ```
 static contextType = ThemeContext;
 static contextType = AuthContext;
```

We have two options: 

1. We can use one context using the `static contextType` method and another using the Consumer tag.

2. We can use two consumer tags -> this is the method we will try now.

In Navbar.js, surround the <ThemeContext.Consumer> with the new <AuthContext.Consumer> tag. We need to provide a function, and this will return all of the JSX we had previously, like so: 

```
 return (
      <AuthContext.Consumer>{(authContext) => (
        <ThemeContext.Consumer>{(themeContext) => {

          const { isAuthenticated, toggleAuth } = authContext;
          const { dark, light, isLightTheme } = themeContext;
          const theme = isLightTheme ? light : dark;

          return (
            <nav style={{background: theme.ui, color: theme.text}}>
              <h1>Context App</h1>
              <div onClick={toggleAuth}>{isAuthenticated ? 'Logged in' : 'logged out'}</div>
              <ul>
                <li>Home</li>
                <li>About</li>
                <li>Contact</li>
              </ul>
            </nav>
          )

        }}</ThemeContext.Consumer>
      )}</AuthContext.Consumer>
    )
```

Note how we changed the names of the parameters from `context` to `AuthContext` and `themeContext` since we now have more than one context being used. We now have access to both contexts in the Navbar component. 


 ## 9 - Intro to hooks

Hooks are special functions which allow us to do things inside functional components in React, that normally we would only need to do inside class components. For example, using state. 

The most common hooks are: 

- **useState()**: lets us use state inside functional components
- **useEffect()**: lets us run code whenever a component renders or re-renders
- **useContext()**: lets us use context in a functional component


 ## 10 - useState hook

 First, import useState from react. `useState()` accepts a value which is the initial value we want to set a piece of state to. `useState()` returns an array containing two values: 

 1. The piece of state itself
 2. The function we can use to edit the piece of state

 We use array destructuring to get these two values: 

 ```
 const [songs, setSongs] = useState([ 
   { title: 'song one', id: 1},
   { title: 'song two', id: 2}
  ])
```

 Now we can use the songs state in the component. If we wanted to add a new song to the list, we would use the `setSongs` function. But using `setSongs` will completely replace the previous songs array with whatever we passed as an argument to `setSongs`. Thus, if we want to just add a new song to the songs list, we need to use spread syntax:

 ```
 const addSong = (title) => {
   setSongs([...songs, { title: title, id: 3 }]);
 };
 ```

 NOTE: the song `id` is hard coded for this example, if we actually wanted to map through the songs in the app we would need a unique id, for example with the **uuid package**.

 
 ## 11 - useState with forms

When there is a form in react, we usually keep track of what the user types in the form using state. 

```
<form onSubmit={}>
  <input type="text" required onChange={(e) => {}}/>
</form>
```

Not only do we need an event listener `onSubmit` on the form tag, we also want to track what the user types into the input using an `onChange` listener. This one uses the `event` object we have access to.  

We need to create a new piece of state to keep track of what the user types in. 

`const [title, setTitle] = useState('')`

We use `setTitle` to update the `title` to whatever the user has typed in the input at that point in time. Finally, for this to work we need to add a `value` property to the input tag, and set it equal to `title`. 

```
  <input type="text" value={title} required onChange={ (e) => setTitle(e.target.value) }/>
```

Now let's move on to the function that runs when the form is submitted. 

```
const handleSubmit = (e) => {
  e.preventDefault();  // prevent default page refresh on submission

  addSong(title);

  setTitle('');  // clears the text in the input after submission
};
```

`<form onSubmit={handleSubmit}>`



 ## 12 - useEffect hook

 useEffect hook is a bit like a life cycle method that would be used inside a class component in react. We don't have access to lifecycle methods in functional components.

 useEffect can be used in functional components. It takes in a callback function as a parameter, which runs every time the the component renders. Typically, it would be used to connect to a database or API endpoint. 

 A simple example: 

```
useEffect(() => {
  console.log('useEffect ran', songs);
})
```

This runs every time the component (re) renders, AKA every time the data inside the component changes (and on the very first render). But what if we have lots of different data in the component, and useEffect runs every time any one of those changes? We might not want to run the particular callback function in useEffect every time any data changes. 

If we only want to run the function when a specific data changes (for example songs), we pass a second parameter to the useEffect hook. This is the **dependency array** which contains the values we want to watch for. This is the specified data that will trigger a re-render, and therefore useEffect, to run. 

```
useEffect(() => {
  console.log('useEffect ran', songs);
}, [songs]);
```



 ## 13 - React hooks with context (useContext)

 Back to the Book List app. 

 Since we can only use the contextType to consume context in a class component, what should we do if we want to turn the BookList class component into a functional component? 

 We can do this with the **useContext hook**. (We could use the Context Consumer tag, but the useContext hook is much cleaner).

 ```
const BookList = () => {

   const { dark, light, isLightTheme } = useContext(ThemeContext);
   const theme = isLightTheme ? light : dark;

  return ( 
    <div className="book-list" style={{color: theme.text, background: theme.bg}}>
      <ul>
          <li style={{background: theme.ui}}>Book 1</li>
          <li style={{background: theme.ui}}>Book 2</li>
          <li style={{background: theme.ui}}>Book 3</li>
      </ul>
    </div>
   );
}
```

 ## 14 - Multiple contexts with hooks

 We can use `useContext` as many times as we want in a component, to use as many different contexts as we want. 

 Let's turn Navbar into a functional component. 

 ```
 const Navbar = () => {

  const { isAuthenticated, toggleAuth } = useContext(AuthContext);
  const { dark, light, isLightTheme } = useContext(ThemeContext);
  const theme = isLightTheme ? light : dark;
```

 ## 15 - Creating a context with hooks

 So far we've been creating context using class components. But we can use a functional component instead now that we have the useState hook. 

 Let's create a new context called BookContext, using a functional component this time.

 ```
 import React, { createContext, useState } from 'react';

export const BookContext = createContext();

const BookContextProvider = (props) => {
  
  const [books, setBooks] = useState([
    { title: 'book 1', id: 1 },
    { title: 'book 2', id: 2 },
    { title: 'book 3', id: 3 },
  ]);

  return ( 
    <BookContext.Provider value={{books}}>
      {props.children}
    </BookContext.Provider>
   );
}
``` 

Since we only need this context inside the BookList component, that is the only thing we need to wrap <BookContextProvider> with in App.js.

```
 <div className="App">
      <AuthContextProvider>
        <ThemeContextProvider>
          <Navbar />
          <BookContextProvider>
            <BookList />
          </BookContextProvider>
          <ThemeToggle />
        </ThemeContextProvider>
      </AuthContextProvider>
    </div>
```

Now we need to consume the context inside the BookList component: 

```

const BookList = () => {

   const { dark, light, isLightTheme } = useContext(ThemeContext);
   const theme = isLightTheme ? light : dark;

   const { books } = useContext(BookContext);

  return ( 
    <div className="book-list" style={{color: theme.text, background: theme.bg}}>
      <ul>
        {books.map( book => {
          return (
            <li key={book.id} style={{background: theme.ui}}>{book.title}</li>
          )
        })}
      </ul>
    </div>
   );
};
```


 ## 16 - Reading List App (Part 1)

 We will create a brand new Reading List Project. 
 See the **book-list** repo.

 ## 17 - Reading List App (Part 2)

  See the **book-list** repo.

 ## 18 - Reading List App (Part 3)

  See the **book-list** repo.


 ## 19 - Reducers, Actions & State

 A **reducer** is a pattern of coding which centralises all methods for changing state into a single function. For example in our Book List app, both `addBook` and `removeBook` interact with the state of `books`. In a bigger app, we could have maybe 4-5 different ways to change this state. 

 Then, we would have to pass each of those functions inside the `value` attribute of the Context.Provider, and then to consume the context we would have to destructure all of them one by one in our components that use them. 

 Instead, what we could do is move all our functions that change the same state, into a single reducer function. This makes our code easier to maintain in the long run. 

- The **reducer function** interacts with the state and changes the data. 
- The **Action object** describes the type of change we want to make inside the reducer function. Ex. { type: 'ADD_BOOK' }
- The **dispatch function** sends the action to the reducer. 

The reducer takes in the state of whatever data we are manipulating as the first parameter, and the action (object) as a second parameter. 

`reducer(state, action)`

When the reducer runs, it looks at the type of action, and based on that, runs different logic to update the state. Once updated, we return the state, which goes back into the provider value. 

In order to use reducers, we need to import the `useReducer` hook. 

Example: (this is inside a context provider file)

Initially we don't use a reducer, and we have:
`const [age, setAge] = useState(20)`

We import useReducer and modify this code to: 
`const [age, dispatch] = useReducer(ageReducer, 20)`

This says we want to use a reducer called `ageReducer` to control the `age` state. The second argument is still the initial value of the state. 

The dispatch method will be the function we use to send our action to the `ageReducer`.

Now let's define the `ageReducer` (outside the functional component):

```
const ageReducer = (state, action) => {
  switch (action.type) {
    case 'ADD ONE':
      return state + 1;
    case 'ADD FIVE':
      return state + 5;  
    case 'ADD NUM':
      return state + action.num; 
    default:
      return state; 
  }
};
```

If we were not using a reducer, inside our <AgeContextProvider> functional component, we would have defined our functions like so:

```
const addOneToAge = () => setAge(age + 1);
const addFiveToAge = () => setAge(age + 5);
const addNumToAge = (num) => setAge(age + num);
```
We would also pass in each of these functions to the <AgeContext.Provider> in the `values` attribute. 

Now that we are using a reducer, we only need to pass in the dispatch: 

`<AgeContext.Provider value={age, dispatch}>`

The dispatch will be called to dispatch an action to the reducer. For example: 

`dispatch({ type: 'ADD_ONE' })`
`dispatch({ type: 'ADD_NUM', num: 7 })`

Note that in the second example, we provide the `num` property required by ADD_NUM as the second property of the action object. This second property is called the **payload**.

Thanks to this, we only ever need to pass in one function (the dispatch) to the Provider. We also keep all our state changing logic inside one reducer function. 


 ## 20 - Adding a reducer

Let's make a reducer for our Book List application. To kee things organised, create a `reducers` folder inside the `src` folder. 

Inside it, create BookReducer.js:

```
export const BookReducer = (state, action) => {

  switch(action.type) {
    case 'ADD_BOOK': 
      return [...state, {
        title: action.book.title,
        author: action.book.author,
        id: v4(),
      }]

    case 'REMOVE_BOOK': 
      return state.filter(book => book.id !== action.id);
      
    default: 
      return state;
  }

};
```

Note that wherever we initially mentioned the variable `books`, we have now replaced it with `state` which is the equivalent inside a reducer function. 

⚠️ How do we incorporate the `prev` logic inside the reducer function ??


Now in BookContext.js, import the useReducer hook and remove the useState hook. 

The file should now look like this: 

```
export const BookContext = createContext();

const BookContextProvider = (props) => {

  const [books, dispatch] = useReducer( BookReducer, []);

  return ( 
    <BookContext.Provider value={{books, dispatch}}>
      { props.children }
    </BookContext.Provider>
   );
}
```

NOTE: we removed the initial books in the starting `books` array so now we start with no books in the list.

At this point, nothing works yet because we are still referencing the old functions `addBook` and `removeBook` in other components, but they no longer exist, we only have the dispatch method now. All we have to do is replace whatever function we had before, with `dispatch`.

```
const BookForm = () => {

  const { dispatch } = useContext(BookContext);
  const [title, setTitle] = useState('');
  const [author, setAuthor] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    
    dispatch({ type: 'ADD_BOOK', book: {
      title,
      author,
    }}
    );

    setTitle('');
    setAuthor('');
  }
```

Similarly: 

```
const BookDetails = ({ book }) => {

  const { dispatch } = useContext(BookContext);

  return ( 
    <li onClick={() => {
      dispatch({ type: 'REMOVE_BOOK', id: book.id});
      }}>
```

 ## 21 - Adding local storage

 Currently, refreshing our app will cause us to lose our state changes. To make them persist, we will use **local storage** (a way to store data in a user's browser).

 The local storage can be viewed in the **applications tab** of the dev tools. We can see that local storage comes in the form of key-value pairs. The local storage can be cleared in the same tab. However, the value MUST BE A STRING. We can check our browser's local storage in the console:

 `localStorage`

 If there is currently nothing stored locally, it will say that the length is zero. If we expand the Prototype section, we can see a bunch of methods. We use `setItem` to add to local storage, and `getItem` to retrieve from local storage. 

 We can add an item to local storage like so: 
 `localStorage.setItem('name', 'bob')`

 But our `books` data come in the form of objects. which can't save directly as objects in local storage. To solve this, we will `JSON.stringify()` our objects in order to store them. Then when we retrieve them, we will `JSON.parse()` them back into JS objects. 

In our Book List app, we want to save the book to local storage every time we add one. We will use `useEffect` inside the BookContextProvider component to do this.

To save the books to local storage: 

```
  useEffect(() => {
    localStorage.setItem('books', JSON.stringify(books));
  }, [books]);
```

At this point, if we refresh the page we will see that the local storage is set back to an empty array. This is because on the first render, we are setting books to be an empty array. We need to change the initial value of books to be whatever is already in local storage. 

We can pass a third argument to useReducer- a function thet returns a value it should take as a default value (even if our second argument is an ampty array).

```
const [books, dispatch] = useReducer( BookReducer, [], () => {
    const localData = localStorage.getItem('books');
    return localData ? JSON.parse(localData) : [];
  });
```

If there is something in localData, this is what will be used as the starting value of books. Otherwise, it will just be an empty array. 









-----------------

## Sources used: 
[The Net Ninja- react context & hooks](https://www.youtube.com/watch?v=CGRpfIUURE0&ab_channel=TheNetNinja)

### To read 
- https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous


Notes: 
- useRef
- React life cycle & class components
- prev & spread syntax