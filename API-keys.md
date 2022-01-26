# Using API keys in React - how to hide API keys

When using an API key in a project, we must make sure to store them safely. Many API keys are used for APIS that charge you based on the number of requests. So if your key gets in the wrong hands, there is a risk that the number of requests will be abused and this could charge you a large sum of money. 

There are 2 things to do in order to safely hide your key:

## 1. DO NOT upload your API key on Github (or anywhere on the internet)!

- Make sure the API key is not hard-coded in any files that will be pushed to GitHub. 
- Also be aware that a key uploaded to GitHub by accident will remain in the commit history even if you remove the key from the latest version of your code. 
- Therefore, if A key is accidentally uploaded, **change the key from the provider ASAP** so that the old one will be rendered useless.


## 2. Use a .env file 

- files containing the key should be **Gitignored**. Any file that starts with `.` will be gitignored and not pushed to GitHub. 
- Info such as keys are generally stored in a `.env` file. 

### IMPORTANT! Storing the key on the front-end 
- even having your key stored in `.env` in React is not super safe. 
- it's okay for quick demos and projects, but not safe for real websites that will be hosted on the internet. 
- if the request to an API is made in the front end (somewhere within the src folder), the code can be seen by inspecting the Dev Tools- in `main.chunk.js` - even the key is displayed. 

### Storing the key on the back-end

- The only truly safe way to keep an API key hidden is to have some form of a back end to the reach project. 
- One way is to build a Node.js back end. 

### NOTE: location of .env
The location of the `.env` file (whether in the root directory, the src folder, or in the backend) does not affect the security. What matters is where the variable we create to hold the value of the key (for example `REACT_APP_API_KEY`) is used. `REACT_APP_API_KEY` should ONLY be referenced in the backend. 

## Tutorial - hiding API keys in Node.js back end

We want to be making any GET requests that use an API key **on the back end**, and then use our front end to get the data from the back end. 

**SEE react-starter-template REPO FOR A SIMPLE EXAMPLE**




### Sources 
https://www.youtube.com/watch?v=FcwfjMebjTU&ab_channel=CodewithAniaKub%C3%B3w
