# Notes from the Full React Tutorial by The Net ninja

[link to course](https://www.youtube.com/watch?v=9D1x7-2FmTA&list=PL4cUxeGkcC9gZD-Tvwfod2gaISzfRiP9d&index=3)

## Lesson 3: Components and Templates

- A component is usually a self-contained section of content. 
- Each component contains its own template (JSX) and logic.
- To begin with, there is only one component being rendered: the App component - also known as the Root component (which is rendered in index.js).


## Lesson 4: Dynamic values in Templates

- in JSX, we can output dynamic values instead of hard coding them. 
- We do this by containing valid JavaScript inside curly braces {}
- The only thing that cannot be outputed in JSX is booleans and objects.
- ERROR: "objects are not valid as a react child"


 ## Lesson 5: Multiple components

 - we can nest as many components as we want.
 - the top level of the component tree is always the App (root) component. 
 - All other components must be direct or indirect children of App. 


## Lesson 7: click events

- If we have a button and we want to fire a function when a click event happens, we declare that `handleClick` function above the JSX for that component. 
- to link the handleClick to the button, we add the attribute to the button component:
`<button onClick={handleClick}>`
- note that we don't invoke the function like `onClick={handleClick()}` because that would automatically fire the function without the user clicking on the button.

What if we wanted to pass in an argument to the function? We can't do this `<button onClick={handleClick(argument)}>` since that would invoke the function right away. To get around this, we need to wrap the handleClick function in an anonymous function. 

`<button onClick={() => handleClick(argument)}>`


### The event parameter

We automatically get access to the event parameter when an event occurs. This parameter is found in the `handleClick` function.

`const handleClick = (e) => { console.log(e) }`

When we log `e` to the console, we get a `SyntheticBaseEvent`.

What about when we take in a parameter for the handler function?

```
const handleClick = (name) => {
  console.log('hello' + name);
}
```

We don't automatically get the event parameter as the first argument inside this function, because in our code, it isn't actually getting referenced as the function (it's called by an anonymous function). Instead, the anonymous function gets access to the event parameter:

`<button onClick={(e) => handleClick(argument, e)}>`

We pass `e` as the second argument to the handleClick function.

```
const handleClick = (name, e) => {
  console.log('hello' + name, e.target);
}
```


## Lesson 8: Using state (useState)

State refers to the date being used in a component at that point in time. The state could be an array, a boolean, object, strings, etc. 

If we declare a normal variable inside a component function, for example:
`let name = 'mario'`
This variable is not reactive. Even if we change it in the code, React does not watch this variable for changes and the UI would not be updated (no re-render). 

We can make values reactive with the `useState` hook. A hook is a special type of function in React that does a certiang job. All hooks start with `use`. 

We use array destructuring to store the value of the state, as well as the function used to update the state. 

`const [value, setValue] = useState('state')`

`const [name, setName] = useState('mario');`

Because `name` is now reactive, whenever the value is changed by calling `setName`, this will trigger a re-render with the updated value.

We can use as many reactive values as we want in a component.


## Lesson 9: React Dev Tools

### Components tab
- used more than Profiler tab. Shows the component tree of the app.
- when you click on a component, it shows more info about state, hooks, props, etc. 
- click on the eye icon to see the matching element in the DOM tree. 


## Lesson 10: Outputting lists

When we have a list of something to output (for example, blogs) dynamically, we use the `map` method. We cycle through the array of blogs and return a bit of template for each item. That template is output to the browser. 

### the key property

Whenever we map through items in react, each root element in the template (the outermost element that is returned by the map method) must have a key property. the key property is used by React to keep track of each item in the dom as it outputs it. The key property helps React keep track of items that are deleted from the list. 

If we don't add a unique key to each item, React cannot distinguish between the different items in the list. We usually use an `id` property as the key value. 

Example of mapping through blogs: 

```
  const [blogs, setBlogs] = useState([
    { title: 'my new blog', body: 'lorem ipsum', author: 'mario', id: 1 },
    { title: 'apple', body: 'banana', author: 'luigi', id: 2 },
    { title: 'kiwi', body: 'pear', author: 'peach', id: 3 }
  ]);


  return ( 
    <div className="home">
      {blogs.map((blog) => (
        <div className="blog-preview" key={blog.id}>
           <h2>{ blog.title }</h2>
           <p>Written by { blog.author }</p>
        </div>
      ))}
    
    </div>
   );
```


## Lesson 11: Props

Whenever we have bits of template that are re-used more than once in the app, it's best to turn that template into its own reusable component. Let's turn the template we outputted above into its own <BlogList> component instead of returning the template directly inside the <Home> component.


```
const BlogList = () => {
  return ( 
    <div className="blog-list">
      {blogs.map((blog) => (
        <div className="blog-preview" key={blog.id}>
           <h2>{ blog.title }</h2>
           <p>Written by { blog.author }</p>
        </div>
      ))}
    </div>
   );
}
```

Then replace the mapped blogs in Home.js with <BlogList />.

At this point, there will be an error: `blogs is not defined` because the BlogList component does not have access to the `blogs` data. We will need to pass `blogs` to <BlogList> as a **prop**. 

Props are a way to pass data from a parent component to a child component. In this case, <BlogList> is the child of <Home>. 

In the parent component: pass the prop

 `<BlogList blogs={blogs} />`

In the child component: receive the prop

Inside the component function, we get access to an argument, the `props` object (arbitrarily named). We can pass and recieve as may props as we want. There are several way to receive props in the child function. 


### option 1:

```
const BlogList = (props) => {

  const blogs = props.blogs;
  const title = props.title;
```

### option 2: destructure the props directly (preferred)

```
const BlogList = ({ blogs, title }) => { }
```


## Lesson 12: Reusing components

We can reuse components such as <BlogList> as many times as we want, and we can even pass different information in them:

```
<BlogList blogs={blogs} title="All blogs" />
<BlogList blogs={blogs.filter((blog) => blog.author === 'mario')} title="Mario's blogs" />
 ```

 In the second <BlogList> component, we are filtering through the blogs and only outputting the blogs written by Mario. This is very useful for implementing a search feature. 


 ## Lesson 13: Functions as props

 If we want to delete a blog, we need a delete button associated with each blog post: 
 `<button onClick={() => handleDelete(blog.id)}>delete</button>`
 This would go inside the <div> that is return from `blogs.map()`.

The state of `blogs` is initialised in Home.js. We don't want to directly edit the blogs prop, it is BAD PRACTICE. We should instead use the method `setBlogs` to update the state of blogs.

Therefore, we should define the `handleDelete` function not in BlogList.js where the button that calls this function is located, but in Home.js where the `blogs` state is initialised. Then, we can pass the `handleDelete` function as a prop. (as opposed to passing through both blogs and setBlogs to the BlogList component, and having handleDelete in BlogList.js).

We need to write `handleDelete` in a way that does not mutate the original `blogs` array. 

```
  const handleDelete = (id) => {
    const newBlogs = blogs.filter(blog => blog.id !== id);
    setBlogs(newBlogs);
  };
```
This filters out the id which matches that of the blog we clicked on, and now the newBlogs array does not contain the blog we filtered out. Then we set the new value of `blogs` to be `newBlogs`.



 ## Lesson 14: The useEffect Hook

 This is the most commonly used hook in react besides useState. useEffect runs a function every render of a component. A component renders initially on first page load, and also when its state changes. useEffect takes a function as the first argument - this is the function that will run every time there's a re-render. 

 The example below simply logs the most recent version of the `blogs` array.

```
 useEffect(() => {
    console.log(blogs);
  });
```

### WARNING: changing the state inside useEffect

When we change the state of something inside a useEffect hook, we can end up with infinite re-renders. The ways around this will be explained later.



 ## Lesson 15: Dependencies of useEffect

 We don't always want to fire the useEffect function after wvery single render- sometimes we only want specific renders to trigger it. We do this by passing a `dependency array` as the second argument to useEffect. 


```
 useEffect(() => {
    console.log(blogs);
  }, []);
```

An empty dependency array as shown above will ensure that useEffect only runs after the first initial render. If the state changese thereafter, it won't run the useEffect function.

Another option is to add actual dependencies to the array- any state values that should trigger the useEffect function to run when they change. 

Let's say we have a state called `name`. If we add `[name]` to the dependency array, this will run the function in useEffect whenever `name` changes. We can also pass more than one item to the dependency array - for example `[name, blogs]`.



 ## Lesson 16: using JSON server

 We'll be using the useEffect hook to fetch some data. Inside useEffect is a good place to fetch data inside a component, because we know that it runs the function when the component initially renders. That is generally when we want to go and fetch data. 

 In the current code we have some hard coded blogs in the `blogs` array, but in real life we would most likely be fetching that data from our database. This tutorial will not be going into creatng a database, but we will create a "fake" database in the form of a JSON file. 

 1. In the root directory of the project, dreate a `data` folder and inside it, a file called `db.json` (name is arbitrary).

 2. Paste some JSON data into the file.

 ```
 {
  "blogs": [
      {
      "title": "my new blog",
      "body": "lorem ipsum", 
      "author": "mario",
      "id": "1"
     }, 
    {
      "title": "bloggo",
      "body": "lorem ipsum and stuff", 
      "author": "luigi",
      "id": "2"
    }, 
    {
      "title": "apple",
      "body": "banana", 
      "author": "mario",
      "id": "3"
    }
  ] 
}
```
  
3. use the JSON server package (used directly from the internet here): `npx json-server --watch data/db.json --port 8082`. Make sure to use a port number that is not currently in use.

This will watch the db.json file and wrap it with API endpoints. Every top-level property in the JSON file is treated as an endpoint. So when we run the command above, we will be able to see the JSON data at `localhost:8082/blogs`.



 ## Lesson 17: fetching data with useEffect

 Let's make a fetch request to get all of the blogs on the initial render of the component. First, replace all of the hard coded data from the initial `blogs` state to be null:
 `const [blogs, setBlogs] = useState(null);`

 Make a get request to the db.json file: 
 ```
   useEffect(() => {
    fetch('http://localhost:8082/blogs')   // get request
    .then(res => {
      return res.json()   // parse JSON into JS object
    })
    .then(data => {
      console.log(data)
      setBlogs(data)   // update the blogs state with the returned data
    })
  }, []);
```

At this point, there will be a react error: `TypeError: Cannot read properties of null (reading 'map')`. This is because fetching the blogs data takes a bit of time, but react is trying to render the initial value of blogs which is `null`, which cannot be mapped through. 

We don't want to output this code until we have a value for blogs:
`<BlogList blogs={blogs} title="All blogs" handleDelete={handleDelete} />`

Thus, we use conditional rendering to first check if `blogs` is truthy, and only render the BlogList if it is. 

```
 <div className="home">
    { blogs && <BlogList blogs={blogs} title="All blogs" handleDelete={handleDelete} /> }
 </div>
```

Let's also delete the handleDelete function and the delete button. These are no longer relevant because we ultimately want to be making delete requests to the db.json file. 

 ## Lesson 18: Conditional loading message

 We want to display a loading message while the data is being fetched. Add a piece of state in the Home component called `isLoading`.

`const [isLoading, setIsLoading] = useState(true)`

```
  return ( 
    <div className="home">
      { isLoading && <div>loading...</div> }
      { blogs && <BlogList blogs={blogs} title="All blogs" /> }
    </div>
   );
```

We also need to make isLoading false after we setBlogs:  

```
.then(data => {
      console.log(data);
      setBlogs(data);
      setIsLoading(false);
    })
```


 ## Lesson 19: Handling fetch errors

Let's handle errors that could happen when we are making the fetch request. It could be an error sent back from the server, or a connection error where we can't even connect to the server. We wouldn't get the right data back in these cases, so we need to let the user know about the error. 

Use the `.catch()` method to catch any kind of network error - when we can't connect to the server. We can simulate this by cancelling the process from JSON server. 

```
useEffect(() => {
    fetch('http://localhost:8082/blogs') // get request
    .then(res => {
      return res.json() // parse JSON into JS object
    })
    .then(data => {
      console.log(data);
      setBlogs(data);
      setIsLoading(false);
    })
    .catch((err) => {
      console.log(err.message);
    })
  }, []);
  ```

  The app should now display `loading...` without changing. 

  If there's another type of error, for example the request reaches the server, but the server sends an error back (maybe the endpoint we tried to fetch from doesn't exist). The catch block we added doesn't automatically catch those errors when we use the fetch API. This is because the request is still reaching the server, and the server is still sending back a response object. The response will not contain the data we need, and will have an error status.

  In this case, we need to check the response object when we get it back. Every response object has an `ok` property which means the fetch was okay and we got data back. So we want to check if the `ok` property is false, and if so, throw an error. 

  ```
  fetch('http://localhost:8082/blogs')
    .then(res => {
      if (!res.ok) throw Error('could not fetch the data for that resource');
      return res.json(); 
    })
  ```

When we throw a new error like this, it will be caught in the catch block at the end of the promise chain. To simulate this error, change the endpoint URL to something that doesn't exist. 

Now that we are catching both kinds of error, we want to store them in some kind of state so that we can output them to the browser. 

`const [error, setError] = useState(null)`

```
 .catch((err) => {
      setError(err.message);
    })
```

Then we can output the error message inside the Home component:

```
return ( 
    <div className="home">
      { error && <div>{error}</div> }
      { isLoading && <div>loading...</div> }
      { blogs && <BlogList blogs={blogs} title="All blogs" /> }
    </div>
   );
```

This works except when we get an error, we still see the loading... text, which we don't want tin this case. We can solve this by setting the state of `isLoading` to false if we get an error. Lastly, we should set `error` to be null is we successfully get the data back. 

```
.then(data => {
      console.log(data);
      setBlogs(data);
      setIsLoading(false);
      setError(null);
    })
    .catch((err) => {
      setError(err.message);
      setIsLoading(false);
    });
```



 ## Lesson 20: Making a custom hook

 What is we wanted to re-use the same logic for fetching data, along with the corresponding states, in another components? We would have to re-write all of this code again, which is not efficient nor easy to manage. We can increase reusability by putting the code we need to reuse in its own file. Then we import the file to use it in a comopnent. This way, we are only writing and mainting the code in one place. 

 When we externalise such logic into its own file, we are creating a **custom hook** in react. 

 Making a custom hook:

 1. Inside the src folder, create a new file called `useFetch.js` (remember all hooks start with use-).

 2. The code we need to reuse will be wrapped inside a function `useFetch`, which is the hook itself. Custom hooks must start with `use-` otherwise it won't work.  

 3. Copy and paste the logic to reuse inside `useFetch`. This includes all of the states, as well as the entire useEffect function. 

 4. At the bottom of the useFetch function, we need a return value. The return value can be anything we want- an array, a string or a boolean. In our case the return value will be an object with 3 properties, which correspond to the 3 states we initiated in the function.

 5. Instead of hard-coding the URL endpoint, we take `url` as a parameter to useFetch. This also means we need to make [url] a dependency to the useEffect function, os that if the url changes, it will re-run the data to get that endpoint. 
  
 5. Import React, useState, useEffect in the file, and `export default useFetch` at the bottom of the file. 

 Our useFetch.js file should look like this now:

```
import { useEffect, useState } from "react";

const useFetch = (url) => {

  const [data, setData] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);
 

  useEffect(() => {
    fetch(url) // get request
    .then(res => {
      if (!res.ok) throw Error('could not fetch the data for that resource');

      return res.json(); // parse JSON into JS object
    })
    .then(data => {
      console.log(data);
      setData(data);
      setIsLoading(false);
      setError(null);
    })
    .catch((err) => {
      setError(err.message);
      setIsLoading(false);
    })
  }, [url]);

  return {data, isLoading, error};
};

export default useFetch;
```


To use our custom hook, we need to import it into our Home component. 
`import useFetch from "../useFetch"`

Then we use the hook like so, to make the same request to the blogs as before:
`const {data, isLoading, error} = useFetch('http://localhost:8082/blogs')`

NOTE: since the data we are using in this specific component is the blogs data, we have 2 options: 


Option 1: rename blogs to data in the JSX returned:
`{ data && <BlogList blogs={data} title="All blogs" /> }`

Option 2: Import the data under the name `blogs` in Home.js:
`const { data: blogs, isLoading, error } = useFetch('http://localhost:8082/blogs')`







