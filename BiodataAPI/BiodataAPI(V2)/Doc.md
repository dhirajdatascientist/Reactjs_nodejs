I'll guide you through setting up SQLite in your Node.js backend and updating your React frontend to handle the data fetched from the database.

## Backend Setup with SQLite3

### Step 4a: Install SQLite3

While in your `backend` directory, install `sqlite3` which is a Node.js driver for SQLite:

```sh
npm install sqlite3
```

### Step 4b: Initialize the Database

Create a new file in the backend directory called `database.js`. This will help initialize the database and provide functions to access the data.

```javascript
// database.js

const sqlite3 = require('sqlite3');
const path = require('path');

// Connect to the database
const dbPath = path.resolve(__dirname, 'users.db');
const db = new sqlite3.Database(dbPath, (err) => {
  if (err) {
    console.error(err.message);
    throw err;
  } else {
    console.log('Connected to the SQLite database.');
    db.run(`CREATE TABLE user (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name text, 
            phone text UNIQUE, 
            country text
            )`,
    (err) => {
      if (err) {
        // Table already created
      } else {
        // Table just created, creating some rows
        const insert = 'INSERT INTO user (name, phone, country) VALUES (?,?,?)';
        db.run(insert, ['Dhiraj', '9999999999', 'India']);
        // Add more initial data if necessary
      }
    });
  }
});

module.exports = db;
```

### Step 4c: Update Backend Code to Use SQLite Database

Now, update your `server.js` to use this database for the `/api/users` route.

```javascript
// server.js

const express = require('express');
const cors = require('cors');
const db = require('./database.js');

const app = express();
app.use(cors());
app.use(express.json()); // For parsing application/json

// Create a GET route to fetch user data
app.get('/api/users', (req, res) => {
  const sql = "select * from user";
  const params = [];
  db.all(sql, params, (err, rows) => {
    if (err) {
      res.status(400).json({"error": err.message});
      return;
    }
    res.json({
      "message":"success",
      "data":rows
    })
  });
});

// ... rest of the server.js code
```

## Frontend Changes

On the frontend, you may want to update the React state to handle an array of users since a database might contain multiple entries.

### Update `App.js` to Handle Multiple Users

```jsx
// App.js

import React, { useState, useEffect } from 'react';

function App() {
  const [usersData, setUsersData] = useState([]);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Fetch user data from the API
    (async () => {
      try {
        const response = await fetch('/api/users'); // Proxy will handle the URL
        if (response.ok) {
          const jsonResponse = await response.json();
          setUsersData(jsonResponse.data);
        } else {
          throw new Error(`Failed to fetch: ${response.status}`);
        }
      } catch (error) {
        setError(error.message);
      }
    })();
  }, []);

  return (
    <div>
      {usersData.length > 0 ? (
        <div>
          <h1>Users Data</h1>
          {usersData.map((user, index) => (
            <div key={index}>
              <p>Name: {user.name}</p>
              <p>Phone: {user.phone}</p>
              <p>Country: {user.country}</p>
            </div>
          ))}
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

### Additional Considerations:

- Error Handling: Make sure to add error handling on both the frontend and backend to gracefully manage any issues that occur when fetching data.
- Database Management: The database initialization code should ideally not be run every time your application starts, as it could slow down your application's startup time. Consider separating it into a different script.
- Security: When developing a real-world application, it’s important to consider the security of your application. For example, never expose sensitive database information, and use environment variables to store your database credentials.
