# Using the Google Maps api with a React App

## Part 1

Go to https://developers.google.com/maps/documentation/javascript/get-api-key

Follow the steps to create an API key for the project.
Create a `.env.development` folder in the project root and paste the API key.

## Part 2

NOTE: there are several npm libraries with similar names (ex. google-maps-react, react-google-maps, etc.) we will use the latter. 

Install the google maps react library: `npm install react-google-maps`

Create a component called Map and import: 
`import { GoogleMap } from 'react-google-maps'`

For now, just return <GoogleMap />.

```
import React from 'react';
import { GoogleMap } from 'react-google-maps';

const Map = () => {
  return <GoogleMap />;
};

export default Map;
```
