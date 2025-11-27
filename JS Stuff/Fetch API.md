# Fetch API

1. fetch is a part of 'promises API' in JavaScript.
2. When you use the `fetch` function to make a <span style="color:orange">network request</span>, it <span style="color:orange">returns a Promise</span> that ==resolves to the Response object representing the response to the request==.
3. it is an **alternate to XMLHttpRequest**.
4. Basic Syntax of 'fetch' request:

```jsx
fetch(url, options) //options - methods, headers, body
  .then((response)=>{
	  //process the response
  })
  .catch((error)=>{
	  //handling any error
  })

```

1. **example**: of making API request to get random jokes using fetch

```jsx
let url = '<https://official-joke-api.appspot.com/jokes/random>';

async function getRandomJoke() {
    const response = await fetch(url)
    const data = await response.json()
    console.log(data)
}

getRandomJoke()

```