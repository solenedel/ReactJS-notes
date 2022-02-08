# Using local storage in React

```
// get item from local storage
   useEffect(() => {
    const jsonData = localStorage.getItem('users');
    const savedUsers = JSON.parse(jsonData);

    if (savedUsers) {
      setApiData(savedUsers);
    }
  }, []);

// set item to local storage
  useEffect(() => {
    const jsonData = JSON.stringify(apiData);
    localStorage.setItem('users', jsonData);
  }, [apiData]);
```