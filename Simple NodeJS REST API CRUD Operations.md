# Simple NodeJS REST API CRUD Operations

REST API - Representational State Transfer Application Programming Interface

- it is a set of rules and conventions for building and interacting with web services
- It allows different systems to communicate over the internet using standard HTTP methods

### 1. **Resources**

- resources are the key components of REST API and represent data entities, such as users, products, or orders.
- Each resource is identified by a unique URL.

### 2. **HTTP Methods**

- **GET**: Retrieve data from a resource.
- **POST**: Create a new resource.
- **PUT**: Update an existing resource or create it if it doesn't exist.
- **PATCH**: Partially update an existing resource.
- **DELETE**: Remove a resource.

### 3. **Stateless**

- The server does not store the state of the client season.

### 4. **JSON/XML**

- Data is sent to and from REST API in json or xml format.

### 5. **Endpoints**

- Endpoints are the URLs where the API's resources are accessible. For example:
    - `GET /users` could retrieve a list of users.
    - `POST /users` could create a new user.

### 6. **HTTP Status Codes**

- These codes are returned by the server to indicate the outcome of the request:
    - **200 OK**: The request was successful.
    - **201 Created**: A resource was successfully created.
    - **400 Bad Request**: The request was invalid or cannot be processed.
    - **404 Not Found**: The requested resource was not found.
    - **500 Internal Server Error**: A generic error occurred on the server.

# Introction to middlewares

- Middleware is software that different applications use to communicate with each other.
- Middlewares acts as a bridge between technologies, tools, and databases so that you can integrate them into a single system.

simple example of a REST API that can perform CRUD operations (Create, Read, Update, Delete) on a list of users. We'll use Node.js with the Express framework.

### Step 1: Set Up Your Project

1. Initialize the project:

```bash
mkdir simple-rest-api
cd simple-rest-api
npm init -y

```

1. Install dependencies:
`npm install express`

### Step 2: Create the API

1. **Create a file named `index.js`:**

```bash
touch index.js

```

1. main code:

```jsx
const express = require('express');
const app = express();
const PORT = 3000;

// Middleware to parse JSON bodies
app.use(express.json());

// In-memory list of users (acting as our database)
let users = [];

// Create a new user (POST /users)
app.post('/users', (req, res) => {
    const user = {
        id: users.length + 1,
        name: req.body.name,
        email: req.body.email,
    };
    users.push(user);
    res.status(201).json(user);
});

// Get all users (GET /users)
app.get('/users', (req, res) => {
    res.json(users);
});

// Get a single user by ID (GET /users/:id)
app.get('/users/:id', (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));
    if (!user) return res.status(404).send('User not found');
    res.json(user);
});

// Update a user (PUT /users/:id)
app.put('/users/:id', (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));
    if (!user) return res.status(404).send('User not found');

    user.name = req.body.name || user.name;
    user.email = req.body.email || user.email;
    res.json(user);
});

// Delete a user (DELETE /users/:id)
app.delete('/users/:id', (req, res) => {
    const userIndex = users.findIndex(u => u.id === parseInt(req.params.id));
    if (userIndex === -1) return res.status(404).send('User not found');

    const deletedUser = users.splice(userIndex, 1);
    res.json(deletedUser);
});

// Start the server
app.listen(PORT, () => {
    console.log(`Server running on <http://localhost>:${PORT}`);
});

```

- Start a server using `node index.js`

### Step: 3 Test the API:

You can test the API using tools like [Postman](https://www.postman.com/) or [curl](https://curl.se/). Here are some example requests:

- **Create a User (POST):**

```bash
curl -X POST <http://localhost:3000/users> -H "Content-Type: application/json" -d '{"name": "John Doe", "email": "john@example.com"}'

```

- Get All Users (GET):

```bash
curl <http://localhost:3000/users>

```

- Get a User by ID (GET):

```bash
curl <http://localhost:3000/users/1>

```

- Update a User (PUT):

```bash
curl -X PUT <http://localhost:3000/users/1> -H "Content-Type: application/json" -d '{"name": "Jane Doe"}'

```

- Delete a User (DELETE):

```bash
curl -X DELETE <http://localhost:3000/users/1>

```