# Using Firebase with React

## Initial steps

1. `npm install firebase` in the project directory

2. Go to www.firebase.google.com and sign in with a google account, Then click on GET STARTED button. 

3. Click on ADD PROJECT and give it a name. It can be the same as the project name. Google Analytics can be disabled. 

4. Once the project has been created, it will redirect to a page which is basically the dashboard for that project. 

## Connecting Firebase to our React App

5. Go to Firebase project overview > project settings and scroll down to the </> button. Click on the button to register the app. The app nickname can be the same as the project name. Make sure Firebase hosting is deselected. 

6. Back in VScode, in the `src` folder create a file called `firebase-config.js`. Everything related to connecting to Firebase will be here. 

7. Back in Firebase website, it should have taken us to a page that gives us all the configuration code we need to connect to Firebase. 

Copy this code:
```
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";


// Your web app's Firebase configuration
const firebaseConfig = {
 ..........
};
```

initializeApp is the function we call to create the connection. It takes the firebaseConfig object as a parameter. 

`const app = initializeApp(firebaseConfig)`

At this point, there may be a connection but we don't have a Firestore database yet, so we need to create one. 

## Creating a Firestore database


