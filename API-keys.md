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

### Important! Storing the key on the front-end (in the src folder)
- even having your key stored in `.env` in React is not super safe. 
- it's okay for quick demos and projects, but not safe for real websites that will be hosted on the internet. 
- if the `.env` file is in the front end (in the src folder), the code can be seen by inspecting the Dev Tools- in `main.chunk.js` - even the key is displayed. 





### Sources 
https://www.youtube.com/watch?v=FcwfjMebjTU&ab_channel=CodewithAniaKub%C3%B3w
