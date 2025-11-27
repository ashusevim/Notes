# Introduction to NodeJS

## What is nodejs?

- Nodejs is a javascript runtime environment that allows you to run javascript on the server-side.
- it allows you to run javascript code outside of a browser.
- Built on Chrome's V8 JavaScript engine.
- JavaScript was used only on the client side (in the browser), but with the advent of Node.js, it's now possible to write server-side applications using JavaScript.

## Key features

### Async and Event-driven

- This design allows Node.js to handle multiple operations concurrently without waiting for one to complete before moving on to the next.
Single threaded
- nodejs operates on a single thread.
- nodejs can handle large number of simultaneous connections due to its non-blocking nature.
NPM(Node package manager)
- NPM is a package manager for nodejs. it provides ton of open source tools and libraries taht you can use in your applications.
Cross platform
- nodejs can run on various operating systems, including windows, macos, linux.

## Common use cases:

- Web servers and APIs
- Real-time applications (e.g., chat, gaming)
- Command-line tools
- Microservices

## A simple web server using nodejs

```jsx
const { createServer } = require('node:http');

const hostname = '127.0.0.1';
const port = 3000;

const server = createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at <http://$>{hostname}:${port}/`);
});

```

- createServer() method of `http` creates a new HTTP server and returns it.
- server is set to listen on specified port and hostname.
- When the server is ready, the callback function is called.

## basics of routing and handling different HTTP methods:

- to handle routes and different HTTP methods there is built-in `http` module.
- We can use basic conditional logic to check the `req.url` and `req.method` properties.
- Here is some code focusing on basics of routes and handling different HTTP methods:

```jsx
const { createServer } = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = createServer((req, res) => {
    const { method, url } = req;

	//checks the url and method to decide how to respond.
    if (url === '/' && method === 'GET') {
        res.end('Hello, World!');
    } else if (url === '/about' && method === 'GET') {
        res.end('About Us Page');
    } else {
	    //returns 404 Not Found for unknown routes
        res.statusCode = 404;
        res.end('404 Not Found');
    }
});

server.listen(port, hostname, () => {
    console.log(`Server running at <http://$>{hostname}:${port}/`);
});

```