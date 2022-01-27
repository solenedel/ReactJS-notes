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

**note about props:** only pass props that you are going to directly reference in the component. If you only use the `setState` function, don't also pass in the `state` variable as a prop, and vise versa. 

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



 ## Lesson 21: The react router

  ### typical (non-react) webistes
  If the user clicks on a link to another page, it will send a brand new request to the server. Requests are constantly made to the server each time the user navigates to a different page. 

  ### React websites
  In react, all changing of page content is delegated to the browser only. We do make an initial request to the server, which the server responds to. Besides returning the html page, it also sends a bundle of compiled react code. React injects content dynamically using the components we create. 

  ### The React Router

  If we navigate to a new page, the React Router steps in and intercepts the request to stop it from going to the server. Instead, it will inject the required content (component) on the screen. 

  We assign a top-level component for each route or page. For example: 
   /  -> Home.js
   /contact -> Contact.js
   /about -> About.js

   Thanks to this, we make less requests to the server and the whole website feels faster. 

   Installing and using the React Router:

   1. `npm install react-router-dom@5` 

   NOTE: we install version 5 as it is stable -> however at this time, there have been many changes to how we use React Router (mostly naming convention) so this part of the tutorial is a bit outdated.


   2. In the root component (App.js) we need to import several things:
   `import { BrowserRouter as Router, Route, Switch } from 'react-router-dom'`

   3. Surround the entire application with the <Router> component. All componets nested in the Router component can have access to the router. 

   4. All of the routes go inside the <Switch> component. Create a route for each page we have using a <Route> component for each page. The <Route> component should have a `path` attribute which specifies the relative path to the page.
```
   function App() {

    return (
      <Router>
        <div className="App">
          <Navbar />
          <div className="content">
            <Switch>
              <Route path="/">
                <Home />
              </Route>
            </Switch>
          </div>
        </div>
      </Router> 
    );
  }  
```

Note that the Navbar component always shows on every page, because it is not nested inside the Switch component. 



 ## Lesson 22: Exact path matches

 Let's add another route for a new component Create.js- this page will let users create a new blog post. Our routes should now look like this:

 ```
 <Switch>
  <Route path="/">
    <Home />
  </Route>
    <Route path="/create">
    <Create />
  </Route>
 </Switch>
```

At this point, clicking on the 'new blog' link in the Navbar will bring us to the home page, which is not what we want. This is because of the way React looks at route paths and matches them against the routes we create. It stops at the first route it finds to be a correct match. Reach counts "/" as a match for "/create" because the "/" route exists inside  "/create". Similarly, a route "/c" would match "/create".

In order to get around this, all we need is to specify react to only match an exact match, by adding the `exact` prop before the path attribute like so: 
`<Route exact path="/create">`


### The Switch component

The switch component ensures that only one route shows in the browser at a given time. Without this component, we could end up with more than one route showing up at one time.

So far, our code is actually still making a request to the server when we change the routes. How can we get react to intercept the requests as it should?



 ## Lesson 23: Router links


In order to get react to handle the content changes in the browser without making new requests, we need to use a special <Link> tag instead of a normal <a> tag in the Navbar where the links to different pages are located. We need to import Link from the react-router-dom package. 

<Link> components don't have an `href` attribute like anchor tags do. Instead they use the attribute `to`. 

It should now look like this:

```
 <div className="links">
    <Link to="/">Home</Link>
    <Link to="/create">New blog</Link>
 </div>
```

If we quickly navigate away from Home while the fetching of data with our custom hook is still in progress, we will see a warning in the console: "can't perform a react state update on an unmounted component". This is because our custom fetch hook is still running in the background even though we have navigated away from the page. It's trying to update the state of our Home component after it's been unmounted from the DOM and replaced with the new component we navigated to. 



 ## Lesson 24: useEffect cleanup

 We want to stop the useFetch hook from continuing to run when we navigate away from the Home page. We will use two things to fix this: 1. A cleanup function in the useEffect hook, and 2. an abort controller. 

 ### Cleanup functions

 A cleanup function is returned by the useEffect hook and fires when the component unmounts. 

 ```
    // previous useEffect code
     .catch((err) => {
      setError(err.message);
      setIsLoading(false);
    });

    // cleanup function
    return () => console.log('cleanup');


  }, [url]);
```

Now we can see in the console that the cleanup function runs every time we navigate away from the Home page. However, if we navigate away quickly we can still see the same warning, so it's time to use an abort controller.

At the top of the useEffect function:
`const abortCont = new AbortController()`

An abort controller is associated with a specific fetch request, and it can be used to stop the fetch. We do this by adding a second parameter to the `fetch()` function, which is an options object- and we set the `signal` property:

`  fetch(url, { signal: abortCont.signal }) `

To stop the fetch, we go in the cleanup function: 

`return () => abortCont.abort();`

If again navigate too fast, we can still see the same error in the console! This is because when we abort a fetch, it throws an error and we are catching an error -> which means we are still updating the state! Even if we are not updating the data state, the isLoading and error states are trying to be updated. This means we are still trying to update the state of the Home component. 

To go around this we need to look for a specific abort error, and specify that in this case, the state should not be updated. 

```
  .catch((err) => {
      if (err.name === 'AbortError') {
        console.log('fetch aborted');
      } else {
        setError(err.message);
        setIsLoading(false);
      }
    });
```


 ## Lesson 25: Route parameters

 Sometimes we need to pass dynamic values as part of a route - in other words, a route where a certain part of the url is changeable. For example: `/blogs/12` or `/blogs/45`. Regardless of what the changeable part is, we still want to render the same page/component. The only difference is the blog that is shown on the page, based on its id. 

 The changeable part of a route is a **route parameter** which in this case will correspond to the id of the blog. We will first create a new BlogDetails component which will show the details for a specific blog. Then create a corresponding route for this page. 

 We can't hard-code an id in the Route path. To signify a route parameter, we use a colon, and then the (arbitrary) name of the variable part. For example: 

 `<Route exact path="/blogs/:id">`

Now, if we go to `/blogs/ANYTHING` we will see the correct page, no matter what number (or letters) we put after the slash. 

Inside the BlogDetails component, we need to be able to access whatever the id is, in order to render the correct blog post. To do this we use a hook from `react-router-dom` called `useParams`.

```
const BlogDetails = () => {
  const { id } = useParams(); // destructure the param(s) as we named them in the path in App.js

  return ( <div className="blog-details">
    <h2>blog details - {id}</h2>
  </div> );
}
```

We want to be able to click on any blog in the list of all blogs, and it will take us to the blog details for that specific blog id. In the BlogList component, let's wrap the blog details in a <Link> tag:

```
<h2>{title}</h2>
      {blogs.map((blog) => (
        <div className="blog-preview" key={blog.id}>
          <Link to={`/blogs/${blog.id}`}>
            <h2>{ blog.title }</h2>
            <p>Written by { blog.author }</p> 
          </Link>   
        </div>
      ))}
```

Note the template string used as the path in the `to` attribute of the Link tag. Now when we click on a specific blog in the list, it takes us to the details for that specific blog id. 



 ## Lesson 26: Reusing custom hooks

We can reuse our custom hook `useFetch` to fetch the data for a specific blog id and display it to the page. In side BlogDetails, import the useFetch function and destructure the values we need from it:

```
const BlogDetails = () => {
  const { id } = useParams();
  const { data: blog, error, isLoading } = useFetch('http://localhost:8082/blogs/' + id);

```

Then conditionally render the content we need:

```
 return ( 
  <div className="blog-details">
    { isLoading && <div>loading...</div> }
    { error && <div>{error}</div> }
    { blog && (
      <article>
        <h2>{blog.title}</h2>
        <p>Written by: {blog.author}</p>
        <div>{blog.body}</div>
      </article>
    )}
  </div> );
```


 ## Lesson 27: Controlled inputs in React

 Controlled inputs in react are a way to set up input fields in forms, so that we can track their values. We can store the value of what a user types in state. If the state changes, that will also change what we see in the input field. What we see in the input field and the state should always be in sync with each other. 

 In Create.js, let's set up a form.

 ```
 <form>
  <label>blog title:</label>
  <input 
  type="text"
  required />
    <label>blog body:</label>
  <textarea
  required></textarea>
  <label>blog author:</label>
  <select>
    <option value="mario">mario</option>
    <option value="luigi">luigi</option>
  </select>
  <button>add blog</button>
</form>
 ```

 Currently we can type stuff in the inputs, but we are not keeping track of these values. We need to set up some state in order to do this. 

 For the title input, this will be our state: 
 `const [title, setTitle] = useState('')`

 We must tell react to associate what in typed in the title inuput, with the `title` state. To do this, we add a value attribute to that input: 

 ```
 <input 
  type="text"
  required
  value={title} />
```

Since we set the title to be an empty string, even if we type inside the input not, nothing will shod up inside it. If we change the initial state to a non empty string, this is what will show up in ths input. We need to make it so that when we try to change what's in the titel field, it triggers the `setTitle` function to change the state to what is being typed in. 

To do this, we use the `onChange` event. 
```
 <input 
    type="text"
    required
    value={title}
    onChange={(e) => setTitle(e.target.value)} />
```

We can output <p>{title}</p> somewhere at the bottom in order to see the title changing in real time as we type in the input. 

We follow the exact same logic with the blog body, by creating a `body` state. How do we deal with the select input? 

For the `author` state, we want the initial value to be one of the values we provide as an option. 

`const [author, setAuthor] = useState('mario')`
The rest of the logic is exactly the same. 



 ## Lesson 28: Submitting a form

When a button is pressed inside a form, it fires a submit event on the form itself. So we can listen for that submit event and react to it. Another option is to attatch a click event on the button itself, but here we will react to the submit event with a `handleSubmit` function.

By default, submitting a form refreshes the page, and we want to prevent that default action. We also want to create a blog object which will be saved to the database. 

```
 const handleSubmit = (e) => {
    e.preventDefault();

    const blog = { title, body, author};
  };
```


 ## Lesson 29: Making a POST request (Json server)

Since we will only be making post requests once in this app, we can directly have the post request inside `handleSubmit`.

```
 const handleSubmit = (e) => {
    e.preventDefault();

    const blog = { title, body, author};

    fetch('http://localhost:8082/blogs', { 
      method: 'POST', 
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(blog)
    })
     .then(() => {
      console.log('new blog added');
    })
  };
```

With the above code, we can already see that we can post a new blog and it will show up in the blog list on the home page.

Let's add a loading state for when we click on the add blog button.

```
const [isLoading, setIsLoading] = useState(false);

  const handleSubmit = (e) => {
    e.preventDefault();

    const blog = { title, body, author};

    setIsLoading(true);

    fetch('http://localhost:8082/blogs', { 
      method: 'POST', 
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(blog)
    })
    .then(() => {
      console.log('new blog added');
      setIsLoading(false);
    })
  };
```

Then, conditionally load different buttons depending on the loading state:

```
 { !isLoading && <button>add blog</button> }
 { isLoading && <button disabled>adding blog...</button> }
```


 ## Lesson 30: Programmatic redirects with useHistory

 We want to rediredt the user to the home page once the blog submission is complete. To do this, we can use another hook: `useHistory` which allows us to go backwards and forwards in history, and add a new page to the history. This history can be imported from the react-router-dom package.

 We then need to invoke the constant (right after declaring the states of the component).
 `const history = useHistory()`

 An example of going back using the history: 
 `history.go(-1)` 
 This would go back one page through history. To go forward in history, we just pass a positive integer instead. 

 However, we want to redirect the user back to the home page. We do this by using the `push` method, to which we pass the route we want to go to: 
 `history.push('/')` 


 ## Lesson 31: Deleting blogs

 Add a delete button in the BlogDetails component:
 ```
  <button onClick={handleClick}>delete</button>
 </article>
 ```

 the click handler:
 ```
  const handleClick = () => {
    // in the fetch we can either use blog.id or id, same thing here
    fetch('http://localhost:8082/blogs/' + blog.id, { 
      method: 'DELETE'
    })
    .then(() => {
      history.push('/');
    });
  };
 ```

 Don't forget to declare the history before: `const history = useHistory()`



 ## Lesson 32: 404 page

 Creata a new component: NotFound.js

 ```
 const NotFound = () => {
  return ( 
    <div className="not-found">
      <h2>sorry, page not found</h2>
      <Link to="/">Back to home</Link>
    </div>
   );
}
```

We want to show this component when a user goes to a page that doesn't exist. We do this by adding a catch-all route in App.js. 

Add a new route at the bottom, and set the path to be an asterisk *:
```
 <Route path="*">
    <NotFound />
  </Route>
```

The * means "catch any other route" and this route must go at the very bottom, because otherwise it would match any route at all. 


# End of tutorial #

## Not covered:
- persisting state on reload
- prev















