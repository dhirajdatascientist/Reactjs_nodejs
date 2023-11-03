# Setting Up a Simple Full Stack Application

This guide will walk you through setting up a basic full stack application using Node.js for the backend and React for the frontend.

## Step 1: Create Project Directory

First, create a main project directory and navigate into it:

```sh
mkdir biodatav1
cd biodatav1
```

## Step 2: Set Up Backend and Frontend Directories

Within the main project directory, create two separate directories for the frontend and backend parts of the application:

```sh
mkdir frontend backend
```

## Step 3: Initialize the Backend

Navigate to the backend directory and initialize a new Node.js project:

```sh
cd backend
npm init -y
```

Then, install the necessary packages:

```sh
npm install express
npm install cors --save
```

## Step 4: Backend Code

Create a file named `server.js` in the backend directory and add the following code:

```jsx
// server.js

// Import express and cors
const express = require('express');
const cors = require('cors');

const app = express();

// Enable CORS for all routes
app.use(cors());

// Create a GET route
app.get('/api/users', (req, res) => {
  // Define the user data
  const userData = {
    name: 'Dhiraj',
    phone: '9999999999',
    country: 'India'
  };

  // Send the userData as a response in JSON format
  res.json(userData);
});

// Define the port
const PORT = process.env.PORT || 3000;

// Start the server
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

To run your server, execute:

```sh
node server.js
```

Your server should start, and you'll see a message indicating it's running on port 3000.

b   Prepare the Frontend

In the `frontend/src` directory, edit the `App.js` file to include the following React code:

```jsx
// App.js

import React, { useState, useEffect } from 'react';

function App() {
  const [userData, setUserData] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Fetch user data from the API
    (async () => {
      try {
        console.log('Fetching user data...');
        const response = await fetch('http://localhost:3000/api/users');
        if (response.ok) {
          const data = await response.json();
          console.log('Data received:', data);
          setUserData(data);
        } else {
          console.error('Response not OK, status:', response.status);
          throw new Error(`Failed to fetch: ${response.status}`);
        }
      } catch (error) {
        console.error('Error fetching user data:', error);
        setError(error.message);
      }
    })();
  }, []);

  return (
    <div>
      {userData ? (
        <div>
          <h1>User Data</h1>
          <p>Name: {userData.name}</p>
          <p>Phone: {userData.phone}</p>
          <p>Country: {userData.country}</p>
        </div>
      ) : error ? (
        <p>Error: {error}</p>
      ) : (
        <p>Loading...</p>
      )}
    </div>
  );
}

export default App;
```

## Enter this URL on web address. 
* localhost:3000/api/users
* Output.

```
{"name":"Dhiraj","phone":"9999999999","country":"India"}
```

## Enter this URL on web address.
* localhost:3001
* Output

```
User Data
Name: Arya
Phone: 9999999999
Country: India
```


