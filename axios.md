# Axios

Axios is the most commonly used library to make HTTP requests from the React front end. Axios can also be used to make requests on the back end (nodeJS server).

Sample axios request:
```
axios
  .get("/URL")
  .then((response) => {
    console.log(response);
    res.json(response);
  })
  .catch((error) => {
    console.log(error.response.status);
    console.log(error.response.headers);
    console.log(error.response.data);
  });
```

Axios has advanced features that make it one of the most used request libraries:

- We can cancel an in-progress request.
- We can set a timeout for a response.
- It supports CSRF protection.


# Ajax

Another HTTP request library, which was made popular by jQuery. The Axios package uses Ajax technology.  