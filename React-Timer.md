# React timer tutorial using hooks

[tutorial link](https://www.youtube.com/watch?v=sSWGdj8a5Fs&ab_channel=CodeBoost)

## Step 1

In App.js, create two states to keep track of: 
```
  const [time, setTime] = useState(0);
  const [timerOn, setTimerOn] = useState(false);
```


## Step 2

Create the buttons for the timer:
```
<button onClick={() => setTimerOn(true)}>Start</button>
<button onClick={() => setTimerOn(false)}>Stop</button>
<button onClick={() => setTimerOn(true)}>Resume</button>
 <button onClick={() => {
          setTimerOn(false);
          setTime(0);
        }}>Reset</button>
```


## Step 3

Add a useEffect function with `[timerOn]` as the dependency array. This is where we include the logic for what happens when the timer turns on and off.

```
   useEffect(() => {
     let interval = null;  // declare the interval

     if (timerOn) {
      interval = setInterval(() => {
        setTime(prevTime => prevTime + 1000);  // add one second (1000ms) to the time
      }, 1000);

     } else {
        clearInterval(interval);  // stop the timer
     }

     // return cleanup function to avoid memory leaks when component unmounts
     return () => clearInterval(interval); 
   }, [timerOn]);
```

The basic timer functionality is now working. Next we style the appearance of the timer displayed timer.


## Step 4

### Displaying the seconds:

```
  <div>
    <span>{('0' + ((time / 1000) % 60)).slice(-2)}</span>
  </div>
```

The above logic prepends a zero for if the seconds are less than 2 digits. If seconds is more than 2 digits, `slice(-2)` is used to slice off the prepending zero. We divide milliseconds by 1000 to get the seconds, and use the modulus 60 to ensure that the number returns to zero after 60 seconds have passed.

### Displaying the seconds:



