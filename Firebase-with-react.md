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

NOTE: is it ok to expose the api key???

initializeApp is the function we call to create the connection. It takes the firebaseConfig object as a parameter. 

`const app = initializeApp(firebaseConfig)`

At this point, there may be a connection but we don't have a Firestore database yet, so we need to create one. 

## Creating a Firestore database

8. In Firebase website, go to Firestore Database > create database (start in production mode). For the location of the database hosting, it should be according to where most of the users would be. Default to US-central.

9. Once the database is created, go to the **rules** tab which determines what can happen from a connection to the database. 

10. Allow reading & writing to the database- change `false` to `true`.

Firestore is a NoSQL database. It works with collections, which are similar to SL tables. A document is like a row in the table.

11. Create a collection calles users and documents. (auto-generate the IDs)

Now the database is set up, but not yet connected to our React app. 

## Connecting the Firestore db to React

In firebase-config.js: 


// stopped tutorial at 14:47

https://www.youtube.com/watch?v=jCY6DH8F4oc&ab_channel=PedroTech


