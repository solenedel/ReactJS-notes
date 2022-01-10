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











## Sources used: 
[The Net Ninja- react context & hooks](https://www.youtube.com/watch?v=CGRpfIUURE0&ab_channel=TheNetNinja)


Notes: 
- useRef
- React life cycle