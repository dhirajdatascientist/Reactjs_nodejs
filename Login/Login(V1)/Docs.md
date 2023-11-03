## Backend Setup (Node.js & Express)

### Step 1: Project Directory

```sh
mkdir loginv1
cd loginv1
```

### Step 2: Backend Directory

```sh
mkdir backend
cd backend
```

### Step 3: Initialize Node.js Project

```sh
npm init -y
```

### Step 4: Install Backend Dependencies

```sh
npm install express cors
```

### Step 5: Create Express Server

Create a file named `server.js`:

```js
// server.js

const express = require('express');
const cors = require('cors');

const app = express();
const PORT = 3000;

app.use(cors());
app.use(express.json());

// Simple user data - normally you'd store this in a database
const users = [
  {
    username: 'dhiraj',
    password: 'password' // WARNING: Never store passwords in plain text in production!
  }
];

// POST route to handle login
app.post('/api/login', (req, res) => {
  const { username, password } = req.body;
  const user = users.find(u => u.username === username && u.password === password);

  if (user) {
    res.json({ message: 'Login successful' });
  } else {
    res.status(401).send('Invalid username or password');
  }
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

To start the server, run:

```sh
node server.js
```

## Frontend Setup (React)

### Step 6: Frontend Directory

Go back to the root of your project directory and create the frontend part:

```sh
cd ..
mkdir frontend
cd frontend
```

### Step 7: Create React App

```sh
npx create-react-app .
```

### Step 8: Install Frontend Dependencies

You will likely need to manage routes in your application:

```sh
npm install react-router-dom
```

### Step 9: Create the Login Component

In `frontend/src`, create a `Login.js` file:

```jsx
// Login.js

import React, { useState } from 'react';

function Login() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = async (e) => {
    e.preventDefault();

    const response = await fetch('http://localhost:3000/api/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ username, password }),
    });

    if (response.ok) {
      // Here you would redirect or do some client-side logic
      alert('Login successful');
    } else {
      alert('Login failed');
    }
  };

  return (
    <div>
      <form onSubmit={handleLogin}>
        <input
          type="text"
          placeholder="Username"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <button type="submit">Login</button>
      </form>
    </div>
  );
}

export default Login;
```

### Step 10: Update App.js

Modify `App.js` to include the `Login` component:

```jsx
// App.js

import React from 'react';
import Login from './Login';

function App() {
  return (
    <div>
      <Login />
    </div>
  );
}

export default App;
```

### Step 11: Run the Frontend

In the `frontend` directory, start the frontend server:

```sh
npm start
```

Now, if you visit `localhost:3000` in your web browser, you should see the login form. When you enter the username `dhiraj` and the password `dhiraj`, the frontend will send a request to the backend and alert 'Login successful' if the credentials match.
