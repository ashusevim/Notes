# Week 1.5 Async vs sync

## Async Functions vs Sync Functions

What does synchronous mean?

- Execution occurs one after the other, sequentially, with only one thing happening at a time

What does asynchronous mean?

- Opposite of synchronous
- Execution happens in parts
- Multiple operations can be in progress simultaneously through context switching

Building some intuition:

- Human brain and body is single-threaded
1. We can only focus on one task at a time
2. But we can context switch between tasks, or delegate tasks to other people

Even if you are single-threaded (you can do only one task at a time), you can handle things in parallel by delegating. You can also context switch between tasks when necessary.

The net amount of time taken to complete tasks can be decreased by using these two strategies (delegating and context switching).

What are some common async functions?

1. setTimeout
2. fs.readFile - to read a file from your filesystem
3. fetch - to fetch some data from an API endpoint

This is how reading a file works:

```
hi there from txt file
```

```jsx
const fs = require("fs")

fs.readFile('temp.txt', 'utf-8', function(err, data){
	if(err == null){  //meaning there is no error
		console.log(data); //print "hi there from txt file"
	}
	else{
		console.log(err);
	}
})

```

Call stack in JavaScript keeps track of function calls in your program.

Example: console.log("hi there");

```jsx
/* it is a sync fn, so it is pushed into call stack,
executed, and then popped off the stack */
console.log("HI");

/* setTimeout fn also pushed into call stack, setTimeout is a web API provided by browser, and it schedules the callback function to be executed after the specified delay.
--> once it is called, it registers the callback function with a timer of 5 seconds in the Web API environment.
--> after registering the callback, fn is popped off the call stack.
--> after 5 seconds, the callback fn is moved from web API to the callback queue, where it waits for the call stack to be empty.
*/
setTimeout(()=>{
	console.log("from inside async function")
}, 5000)

// this entire process is handled by call stack
let a = 0;
for(let i = 0; i<10; i++){
	a = a + 1;
}

//fn will be called and logs 10 to it
console.log(a);

```

- The event loop continuously checks the call stack and, if it is empty, moves functions from the callback queue to the call stack

### **Visualization**

- **Call Stack**: Where functions are executed one by one.
- **Web APIs**: External environment provided by the browser for handling asynchronous tasks (e.g., timers, AJAX calls).
- **Callback Queue**: Holds functions that are ready to be executed but are waiting for the call stack to be empty.
- **Event Loop**: Continuously monitors the call stack and the callback queue, ensuring that asynchronous tasks are executed in order.
--> This flow ensures that your synchronous code runs first, and any asynchronous callbacks are executed only when the call stack is clear.